# Spring Boot: Deploying to OpenShift with Helm and ArgoCD

[![Docker Repository on Quay](https://quay.io/repository/swinches/spring-boot-helm-openshift-argocd/status "Docker Repository on Quay")](https://quay.io/repository/swinches/spring-boot-helm-openshift-argocd)

## Setup

### Build and deploy from scratch

You'll need a container image registry to push to. I'm using Quay.io here.

Build and push an image first:

    podman build -t quay.io/YOURNAME/spring-boot-helm-openshift-argocd .
    podman push quay.io/YOURNAME/spring-boot-helm-openshift-argocd:latest

    docker build -t quay.io/YOURNAME/spring-boot-helm-openshift-argocd .
    docker push quay.io/YOURNAME/spring-boot-helm-openshift-argocd:latest

Once the image has been pushed, install the app into your cluster with this Helm command:

    helm install \
        --set image.repository=quay.io/YOURNAME/spring-boot-helm-openshift-argocd \
        --set image.tag=latest \
        --set fullnameOverride=myapp-latest \
        myapp-latest helm/myapp

(`fullnameOverride` is a handy value in the Helm chart which lets us choose a name for all the objects - otherwise Helm will use the code in `_helpers.tpl` to generate `myapp.fullname`)

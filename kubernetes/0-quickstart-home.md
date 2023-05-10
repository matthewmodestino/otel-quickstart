# Kubernetes OTel Quickstart Home

In this quickstart, you will be guided through install and deployment of the Splunk OTel Collector for Kubernetes Helm Chart. 

The modules contained here will cover Helm Chart download, prep and Install, using a sample base "values.yaml"

At this time, the modules assume a linux based workstation. Windows users should be able to follow along with minor adjustments. 

# Requirements
- A linux based workstation
- Access to a Kubernetes Environment
- Kubectl
- Helm

To begin, you will need access to a Kubernetes environment via Kubectl. Any environment should suffice. 

Recommended options include, Docker Desktop Kubernetes, MicroK8s or k3s. 

You will also require HELM installed. 

You will know you are ready to begin if you can run the following commands:

```
% kubectl get nodes
NAME             STATUS   ROLES           AGE    VERSION
docker-desktop   Ready    control-plane   321d   v1.24.0
```

```
% helm version
version.BuildInfo{Version:"v3.10.0", GitCommit:"ce66412a723e4d89555dc67217607c6579ffcb21", GitTreeState:"clean", GoVersion:"go1.19.1"}
```

If you can't run these two commands, review your kubectl and helm install and configuration. 

If you can, lets [proceed to the Lab Modules](https://github.com/matthewmodestino/otel-quickstart/blob/main/kubernetes/1-module-one.md)!



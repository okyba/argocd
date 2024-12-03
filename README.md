## Description

This repository contains config files for the following:

* ArgoCD installation (see Argo folder) - installs ArgoCD to control the CD process of applications deployment within K8s cluster
* ArgoCD applications deployments (see Apps folder) - configs for applications deployment in ArgoCD

For details see the structure below:

├── Apps                            # ArgoCD applications catalog
│   ├── helm-charts                 # ArgoCD applications helm-charts
│   │   └── k8s-monitoring-vm
│   │       ├── Chart.yaml          # helm-chart for 'k8s-monitoring-vm' app
│   │       ├── charts              # helm-chart dependencies for 'k8s-monitoring-vm' app
│   │       │   ├── grafana-8.6.2.tgz
│   │       │   ├── victoria-metrics-agent-0.14.8.tgz
│   │       │   └── victoria-metrics-cluster-0.14.12.tgz
│   │       ├── templates           # manifests for 'k8s-monitoring-vm' app deployment in k8s cluster
│   │       │   ├── spam_app.yaml
│   │       │   └── spam_dashboard.yaml
│   │       └── values.yaml         # values for 'k8s-monitoring-vm' app
│   ├── manifests
│   │   └── k8s-monitoring-vm.yaml  # manifest for 'k8s-monitoring-vm' app deployment in ArgoCD
│   └── root.yaml                   # manifest for 'root' app deployment in ArgoCD
├── Argo
│   └── values-ha.yaml              # values for ArgoCD installation in HA mode (could be skipped)


## Prerequisites

Install the following packages & versions: 

* Kubectl v.1.22.* or higher: https://kubernetes.io/releases/download/#kubectl
* helm 3.14.0 or higher: https://github.com/helm/helm/releases


## How to's

1.  ArgoCD installation:
```console
helm repo add argo https://argoproj.github.io/argo-helm
```

* simple installation (non-HA):
```console
helm install argocd argo/argo-cd
```

* High Available installation:
```console
helm install argocd argo/argo-cd -f ./Argo/values-ha.yaml
```

2. Deploy & control custom applications.
The 'root' app looks up for any changes made within the "Apps/manifests" folder containing custom apps manifest files. In case of any commit with the such changes the 'root' app will apply them in ~3 minutes automatically.
It's only required to run the 'root' app deployment within your k8s cluster:
```console
kubectl apply -f Apps/root.yaml
```


## Credits

* https://github.com/argoproj/argo-helm
* https://github.com/argoproj/argo-helm/tree/main/charts/argo-cd
### Argo CD App-of-Apps pattern and set everything up so that Argo CD can update itself.
---
## What is Argo CD?
Argo CD is a GitOps tool to automatically synchronize the cluster to the desired state defined in a Git repository. Each workload is defined declarative through a resource manifest in a YAML file. Argo CD checks if the state defined in the Git repository matches what is running on the cluster and synchronizes it if changes were detected.

For example, instead of running CLI commands to update resources with kubectl apply or helm upgrade we would update an Application manifest inside our Git repository. Argo CD periodically checks the repository for changes. It will recognize that the application manifest has changed and automatically synchronize the resources on the cluster.

With this workflow security is improved too. A connection to the cluster, either from the developers laptop or from a CI/CD system, is no longer needed as changes are pulled from the Git repository by a Kubernetes Operator running inside the cluster.
## Why ArgoCD

https://blog.container-solutions.com/hs-fs/hubfs/Screen%20Shot%202020-10-18%20at%2010.15.20%20AM.png?width=750&name=Screen%20Shot%202020-10-18%20at%2010.15.20%20AM.png

REF: https://blog.container-solutions.com/fluxcd-argocd-jenkins-x-gitops-tools
## Requirements

To follow this tutorial you’ll need:

- A Kubernetes cluster and kubectl (1.19.1)
- Helm (3.4.2)
- A public git repository

## Code struture
```
> tree -L 3
.
├── README.md
├── k8s
│   ├── apps
│   │   ├── Chart.yaml
│   │   ├── templates
│   │   └── values.yaml
│   ├── argocd
│   │   ├── Chart.lock
│   │   ├── Chart.yaml
│   │   ├── charts
│   │   └── values.yaml
│   ├── argocd-notifications
│   │   ├── argocd-notifications-catalog-v1.0.2-install.yaml
│   │   ├── argocd-notifications-v1.0.2-install.yaml
│   │   └── kustomization.yaml
│   ├── dashboard
│   │   ├── clusterrole.yml
│   │   ├── dashboard-adminuser.yaml
│   │   └── dashboard.yml
│   ├── efk
│   │   ├── es-values.yaml
│   │   ├── filebeat-values.yaml
│   │   └── kibana-values.yaml
│   ├── linkerd
│   │   └── linkerd.yml
│   ├── note.txt
│   ├── prometheus-operator
│   │   └── values.yaml
│   └── traefik
│       ├── argo.yml
│       ├── custom-values.yml
│       ├── dashboard.yml
│       └── traefik-ingress.yml
└── project1
    ├── dev
    └── production

14 directories, 22 files
```

## Conclusion

In this tutorial we’ve installed Argo CD from a Helm chart and set it up so that it can manage itself. Updates to Argo CD can be done by modifying the manifest inside the Git repository.

We’ve created a “root” application that uses the app-of-apps pattern to manage our applications in a declarative way.

To show how we can install applications with Argo CD, we’ve added (and then removed) Prometheus from our cluster.

More details about Argo CD can be found on the project page and the GitHub repository.# argocd

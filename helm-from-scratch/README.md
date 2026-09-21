# Helm From Scratch

## Project Overview

This project demonstrates how to create and manage a Helm chart from scratch for a simple Kubernetes application.

The chart deploys a BusyBox-based application and includes the following Kubernetes resources:

* Deployment
* Service
* DaemonSet
* CronJob
* Job
* ConfigMap
* Secret

## Project Structure

```text
helm-from-scratch/
├── charts/
│   └── myapp/
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
│           ├── deployment.yaml
│           ├── service.yaml
│           ├── daemonset.yaml
│           ├── cronjob.yaml
│           ├── job.yaml
│           ├── configmap.yaml
│           └── secret.yaml
├── README.md
├── answers.md
└── outputs/
    ├── helm-install.txt
    ├── helm-upgrade.txt
    ├── helm-history.txt
    └── helm-rollback.txt
```

## Docker Image

The project uses the public BusyBox Docker image.

Initial image:

```text
busybox:1.36
```

The image was later upgraded to:

```text
busybox:1.37
```

## Helm Installation

The chart was installed using:

```bash
helm upgrade --install myapp ./charts/myapp
```

This command installs the release if it does not exist and upgrades it if it already exists.

## Verify the Deployment

```bash
kubectl get all
kubectl get configmap
kubectl get secret
```

## Helm Lint

The chart was validated using:

```bash
helm lint ./charts/myapp
```

The chart passed Helm lint successfully.

## Upgrade

The image version was changed in `values.yaml` and the release was upgraded using:

```bash
helm upgrade --install myapp ./charts/myapp
```

## Helm History

The release history can be viewed with:

```bash
helm history myapp
```

The history records successful and failed revisions and shows the rollback operation.

## Rollback

The release was rolled back to revision 1 using:

```bash
helm rollback myapp 1
```

The rollback completed successfully.

## Configuration

The ConfigMap stores application configuration such as:

* Application message
* Application port

The Secret stores sensitive application data such as the API token.

Both the ConfigMap and Secret are used by the Deployment and DaemonSet through environment variables and mounted volumes.

## Verification

The final deployment was verified using:

```bash
helm status myapp
kubectl get all
kubectl get configmap
kubectl get secret
```

The final Helm release status was:

```text
STATUS: deployed
```


# Helm From Scratch - Answers

## 1. What is Helm?

Helm is a package manager for Kubernetes. It allows Kubernetes resources to be packaged into reusable charts and deployed as releases.

A Helm chart contains templates and configuration values that can be used to create Kubernetes resources consistently.

## 2. Why use `helm upgrade --install`?

The command:

```bash
helm upgrade --install myapp ./charts/myapp
```

is useful because it handles both cases:

* If the release does not exist, Helm installs it.
* If the release already exists, Helm upgrades it.

This makes the deployment command convenient for repeated deployments.

## 3. What is `values.yaml` used for?

`values.yaml` contains configurable values used by the Helm templates.

In this project it contains values such as:

```yaml
image:
  repository: busybox
  tag: "1.36"
```

It also contains application settings, Service settings, Deployment replicas, the CronJob schedule, and Secret values.

Using values makes the chart easier to configure without changing the templates.

## 4. What is the purpose of the Deployment?

The Deployment manages the application Pods and maintains the desired number of replicas.

This project uses:

```yaml
replicas: 2
```

Therefore, Kubernetes maintains two application Pods.

## 5. What is the purpose of the Service?

The Service provides a stable network endpoint for the application Pods.

The project uses a `ClusterIP` Service and forwards:

```text
Service port: 80
Target port: 8080
```

## 6. What is the purpose of the DaemonSet?

A DaemonSet ensures that a Pod runs on each eligible Kubernetes node.

The chart includes a BusyBox container managed by a DaemonSet.

## 7. What is the purpose of the Job?

A Job runs a task until it completes successfully.

In this project, the Job runs a BusyBox command that prints a message and then exits successfully.

Jobs have immutable Pod templates after creation. Therefore, changing the image of an existing Job can cause an immutable-field error during an upgrade. The chart was adjusted so that Job names can change with the image version.

## 8. What is the purpose of the CronJob?

The CronJob creates Jobs according to a schedule.

This project uses:

```text
*/5 * * * *
```

which runs the CronJob every five minutes.

## 9. What is the purpose of the ConfigMap?

The ConfigMap stores non-sensitive configuration.

This project stores:

* Application message
* Application port

The Deployment and DaemonSet access the ConfigMap using environment variables and a mounted volume.

## 10. What is the purpose of the Secret?

The Secret stores sensitive configuration.

This project stores an example API token:

```text
API_TOKEN
```

The Secret is referenced by the Deployment and DaemonSet through environment variables and a mounted volume.

The value used in this assignment is only a demonstration value and should not be used for a real production credential.

## 11. What happened during the image upgrade?

The BusyBox image was changed from:

```text
busybox:1.36
```

to:

```text
busybox:1.37
```

The release was upgraded with:

```bash
helm upgrade --install myapp ./charts/myapp
```

Initial upgrade attempts failed because the existing Kubernetes Job had an immutable Pod template.

After adjusting the Job template and removing the old Job, the Helm upgrade completed successfully.

## 12. What is Helm history?

Helm history shows the revisions of a Helm release.

It provides information such as:

* Revision number
* Status
* Chart version
* Application version
* Description

The command used was:

```bash
helm history myapp
```

## 13. What is a Helm rollback?

A rollback restores a Helm release to a previous revision.

This project used:

```bash
helm rollback myapp 1
```

The rollback completed successfully and created a new Helm revision describing the rollback operation.

## 14. Final verification

The final release was verified with:

```bash
helm status myapp
kubectl get all
kubectl get configmap
kubectl get secret
```

The final Helm release status was:

```text
deployed
```

The Deployment had two running replicas, the DaemonSet was running, the Job completed successfully, the CronJob was configured, and the ConfigMap and Secret were present.


# Kubebuilder
> This repository contains code for the Cronjob (v1) implemented by Kubebuilder. It doesn't contain webhook implementation.

Kubebuilder is the SDK tool and a Framework which is similar to Operator-SDK. It is used to build custom Kubernetes-APIs that deploy controllers for workloads to manage them in a customized way. It is based on the same concept of Kubernetes Operators and Custom Resource Definitions (CRDs). It goes deeper in working and configuration as compared to Operator-SDK.

It mainly runs on the already provided services by Kubernetes. Similar to Operator-SDK, we have to define the types in "types.go" file, define how the operator will run in `Reconcile()` function and define the .yaml file of the workload. Much of the project structure is similar to Operator-SDK but some of it is changed for further configurations and the option to include multi-version API.

Changes that Kubebuilder contains as compared to Operator-SDK:
* Much of the Operator configuration is written from scratch except the file structure instead of directly getting some pre-built operators.
* It contains configurations for roles, webhooks and certifications.
* It supports Multi-Version API.
* It has "Makefile".

## Kubebuilder Cronjob Implementation
This **Kubebuilder Cronjob Implementation** is the direct implementation from the [Kubebuilder Cronjob Tutorial](https://book.kubebuilder.io/cronjob-tutorial/cronjob-tutorial.html) featuring the same file struture. This Operator implementation is done for Cronjob in a way that when we deploy the Cronjob from it's .yaml file, the Operator/Controller gets the information of the Cronjob in a `struct{}` and which contains the Cronjob Schedule, it's starting deadline seconds, job history and concurrency policy. The `Reconcile()` function of the Controller\Operator sets the Clock in which it declares and defines how to deploy the Cronjob and get information about it.

After this, the entire Operator/Controller with its configuration is deployed using `make install` and `make run` locally (outside the cluster). To deploy it inside the cluster, use `make docker-build` and `make deploy`. After deploying the contoller, it starts watching the events from the workload and shows it by logs of the Operator/Controller pod. Now, when the .yaml of Cronjob is applied, the logs start updating with the status about and from the Cronjob and now this Operator\Controller will manage and monitor this Cronjob.

However, Webhooks are NOT implemented in this Operator/Controller configuration due to the error "Undefined v1.beta1.Webhook" delivered by "parser.go".

_For reference, the .yaml file of Cronjob does the work of both "cr.yaml" and "operator.yaml" from Operator-SDK._

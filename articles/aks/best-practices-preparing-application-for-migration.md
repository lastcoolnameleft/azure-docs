---
title: Best Practices - Preparing Application for migration to K8S
description: Learn how to prepare your application for migration to Kubernetes
services: container-service
author: lastcoolnameleft

ms.service: container-service
ms.topic: conceptual
ms.date: 11/5/2018
ms.author: lastcoolnameleft
#Customer intent: As an AKS Cluster Operator, I want to migrate my application to a container so that I can run it on Kubernetes
---

# Preparing Your Application for Migration to Kubernetes

As you prepare your workload for to be run as a Cloud Native application, there are a number of considerations which will make the migration easier.

This best practices article will focus on the journey of taking a single application and migrating it to run in Azure Kubernetes Service.  Steps provided here are intended as a guideline and might not be the full set of steps required to perform the migration.  In this article, you learn a phased approach of how to modernize your application:

> [!div class="checklist"]
> * Create a Dockerfile and a container image
> * Run your container image on your local workstation
> * Auto-build your container image
> * Run the the container image in AKS
> * Optimize your AKS deployment
> * Next steps

## Phase 1: Create a runnable container

By the end of this phase, you will have a Dockerfile and a container image.  This container image can be run inside Docker on your local workstation.

* Containerize App - Build Dockerfile
  * Extract/Inject config data
    * Reference secrets/ConfigMap
  * Choose base version
    * Keep image small
  * Use Container logging
  * Make container stateless and immutable
  * Least Privileged
    * Avoid Privileged containers
    * Avoid running as root

## Phase 2: Run inside local environment

By the end of phase, you will have verified that the container image you created in the previous phase runs in a localized version of Kubernetes.  You will create Kubernetes Manifest files in YAML and deploy to [MiniKube](https://github.com/kubernetes/minikube).

* Test first in MiniKube
  * Write deployment + Service file
  * Config pod storage
  * Bundle everything into Helm chart

## Phase 3:  Auto-build of container

At end of phase:  should have container image pushed to ACR.  Can pull ACR images to local env

* DevOps
  * Choose image version
  * Establish CI/CD Pipeline
    * ACR
    * Link to BC/DR for geo-replication

## Phase 4:  Run in AKS

At end of phase:  Should have publicly exposed service on AKS pulling images from ACR

* Data Store
  * File based - Azure Files/Disk
  * (if applicable) Migrate to PaaS Database 
* App Insights
  * Monitor app
  * Expose Health (liveness + readiness checks)

## Phase 5:  Optimizations

* Test in AKS
  * Write Ingress
    * Different options:  nginx, AppGW, Traffek
  * Enable HTTPS
  * KeyVault for secrets/ConfigMap for Config
At end of phase:  Should have ingress + HTTPS 

## Phase 6: Next Steps

Now that you have gotten your application running inside AKS, there are other areas that you might be interested in pursuing.  This section is a reference to guide you along that path.

* Data store
* Resource Constraints - Limit/requests
* Scaling
* RBAC
* Environment isolation
* Investigate Serverless

# [CKAD](http://www.cncf.io) - Certified Kubernetse Application Devloper 
YAML covering syllabus of CKAD
<img src="https://d33wubrfki0l68.cloudfront.net/69e55f968a6f44613384615c6a78b881bfe28bd6/42cd3/_common-resources/images/flower.svg" height="40%" width="60%">

Kubernetes CKAD Example Questions Practical Challenge Series by Will Boyd (willb@linuxacademy.com) of Certified Kubernetes
Application Developer (CKAD) Study Guide

Content for *CKAD* _Study Guide_

## [1. Core Concepts](https://github.com/engineerbaz/k8s-practice/blob/master/LinuxAcademy/1_Core_Concept.md)
- Kubernetes API Primitives 
- Creating Pods 
- &#9745; Namespaces 
- :heavy_check_mark: Basic Container Configuration

## 2 Configuration

- [x] ConfigMaps 
- [ ] SecurityContexts 
- Resource Requirements 
- Secrets 
- ServiceAccounts 

##Multi-Container Pods
7
Understanding Multi-Container Pods . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 8
Containers within a pod can interact with each other in various ways: . . . . . . . . . 8
Some common design patterns for multi-container pods are: 8
Observability
. . . . . . . . . . . . .
11
Liveness and Readiness Probes . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 11
Container Logging . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 12
Monitoring Applications . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 13
Debugging . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 13
kubectl get . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 14
kubectl describe . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 14
kubectl logs . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 14
kubectl edit . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 14
1Export object yaml
kubectl apply
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 14
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 14
Pod Design
15
Labels, Selectors, and Annotations . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 15
Deployments . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 16
Rolling Updates and Rollbacks . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 17
Jobs and CronJobs . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 18
Services and Networking
20
Services . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 20
Service types .
NetworkPolicies . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 21
Ingress rules . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 22
Egress rules . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 22
To and From Selectors
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 22
State Persistence
22
Volumes . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 23
PersistentVolumes and PersistentVolumeClaims . . . . . . . . . . . . . . . . . . . . . . . . . 23
PersistentVolumes
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 23
PersistentVolumeClaims . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 24
Use a PersistentVolumeClaim in a Pod
. . . . . . . . . . . . . . . . . . . . . . . . . . 25

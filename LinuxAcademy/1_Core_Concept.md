Kubernetes CKAD Example Questions Practical Challenge Series by Will Boyd (willb@linuxacademy.com) of Certified Kubernetes
Application Developer (CKAD) Study Guide

## 1. Core Concept -  Pod Creation

### 1.1 - Creating Simple Pod

Pod with Busybox image and print How are you Wold?

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod_with_output
  labels:
    app: dev
spec:
  containers:
  - name: pod_with_output
    image: busybox
    command: ['sh', '-c', 'echo How are you World?! && sleep 3600']
  restartPolicy: Never
```

### 1.2 - Define a Command and Arguments for a Container

Pod with Busybox image and use of arguments 

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod_with_args
  labels:
    app: dev
spec:
  containers:
  - name: pod_with_arguments
    image: busybox
    command: ['echo']
    args: ['Hi, World!!!']
  restartPolicy: Never    
```
### 1.3 - Container with Nginx port exposed.

Pod with Nginx image & Port 80 exposed

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginxPortExposed
spec:
  containers:
  - image: nginx
    name: nginx_port_exposed
    ports:
    - containerPort: 80
```
Now make Service for it

```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  name: 1tets-ng
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    run: 1tets-ng
status:
  loadBalancer: {}
``` 

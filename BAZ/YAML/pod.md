# [CKAD](http://www.cncf.io) - Certified Kubernetse Application Devloper 
YAML covering syllabus of CKAD

## Core Concept -  Pod Creation

### 1- Creating Simple Pod

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

### 2- Define a Command and Arguments for a Container

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


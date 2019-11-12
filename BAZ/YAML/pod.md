# [CKAD](http://www.cncf.io) - Certified Kubernetse Application Devloper 
YAML covering syllabus of CKAD
<img src="https://d33wubrfki0l68.cloudfront.net/69e55f968a6f44613384615c6a78b881bfe28bd6/42cd3/_common-resources/images/flower.svg" height="50%" width="70%">

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

## 2 - Configuration -  ConfigMaps


### 2.1 - Configure a Pod to Use a ConfigMap

CM with data entry manually

```yaml
apiVersion: v1
kind: ConfigMap
metaData:
  name: my-configmap
data:
  mykey: myvalue
  app: dev
````


### 2.2 -  ConfigMap data to a pod as an environment variable

Add ConfigMap data to a pod as an environment variable:

```yaml
apiVersion: v1
kind: configMap
metadata:
  name: cm_env
spec:
  containers:
  - name: pod-cm
    image: k8s.gcr.io/busybox
    command: ['sh', '-c', "echo $(MY_VAR) && sleep 3600"]
    env:
      - name: MY_VAR
        valueFrom: 
          configMapKeyRef:
            name: my-cm
            key: mkey
```
### 2.3 - 













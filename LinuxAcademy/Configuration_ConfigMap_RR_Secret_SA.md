# [CKAD](http://www.cncf.io) - Certified Kubernetse Application Devloper 
YAML covering syllabus of CKAD

## _Certified Kubernetes Application Developer (CKAD) Study Guide_
 

## 2 - Configuration -  Part A - ConfigMaps


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
### 2.3 - Add ConfigMap data to a pod as a mounted volume:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-config-map-pod
spec:
  - conatainers:
    name: myapp-conatainer
    image: busybox
    command: ['sh', '-c', 'ls /etc/config && sleep 3600']
    volumeMounts:
    - name: config-volume
      mountPath: /etc/config
  volumes:
   - name: config-volume
     configMap:
       name: myConfigMap
````


## 2 - Configuration -  Part B- SecurityContexts

### 2.4 Configure a Security Context for a Pod or Container

Use a pod’s securityContext to specify particular OS-level privileges and permissions for a pod:

```yaml
apiVersion: v1
kind: securityContext
metadata:
  name: my-securitycontext-pod
spec:
  runAsUser: 2000
  fsGroup: 3000
  containers:
  - name: myapp-container1
    image: busybox
    command: ['sh', '-c', "Hello K8s! && sleep 3600"]
    
 ```
  
  ## 2 - Configuration - Part C-  Resource Requirements
  
  ### 2.5 Resource requests and limits of Pod and Container
  
  Resource requests and limits of Pod and Container
  
  ```yaml
apiVersion: v1
kind: Pod 
metadata:
  name: myapp-resource-request
spec:
  containers:
  - name: myapp-container2
    image: busybox
    command: ['sh', '-c', "Hello K8s! && sleep 3600"]
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```
## 2 - Configuration - Part D -  Secret

### Create a secret

```yaml
apiVersion: v1
kind: secret
metadata:
  name: my-secret
stringData:
  myKey: myPassword
```
OR

```yaml
kubectl create secret generic my-secret --from-literal=myKey=myPassword --from-literal=password='S!B\*d$zDsb'
```
now run command to see info about secret `kubectl get secrets my-secret -o yaml`

following output will be generated.

```yaml
apiVersion: v1
data:
  myKey: *bXlQYXNzd29yZA==*
  password: UyFCXCpkJHpEc2I=
kind: Secret
metadata:
  creationTimestamp: "2019-11-17T08:27:28Z"
  name: my-secret
  namespace: default
  resourceVersion: "217117"
  selfLink: /api/v1/namespaces/default/secrets/my-secret
  uid: 9a9ef542-50a3-47ca-999d-69c546d802f2
type: Opaque

```

password crack, it will decode output

```yaml
echo -n ’bXlQYXNzd29yZA==’, then redirect to base64 --decode
```


    








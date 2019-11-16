# [CKAD](http://www.cncf.io) - Certified Kubernetse Application Devloper 
YAML covering syllabus of CKAD

Kubernetes CKAD Example Questions Practical Challenge Series by Will Boyd (willb@linuxacademy.com) of Certified Kubernetes
Application Developer (CKAD) Study Guide
 

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











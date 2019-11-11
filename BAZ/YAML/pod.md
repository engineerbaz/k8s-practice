# [CKAD](http://www.cncf.io) - Certified Kubernetse Application Devloper 

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



```

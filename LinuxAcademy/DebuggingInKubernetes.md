
 # [CKAD](http://www.cncf.io) - Certified Kubernetse Application Devloper 
YAML covering syllabus of CKAD

## _Certified Kubernetes Application Developer (CKAD) Study Guide_
 

## 3 - Debugging in Kubernetes -   


### Find the broken pod and save the pod name to the file



```yaml
kubectl get pods --all-namespaces
```

Once you have located the broken pod, `vi /home/cloud_user/debug/broken-pod-name.txt'
enter the name of the broken pod, and save the file

```yaml
kubectl top pod -n candy-store
```


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
### Get the broken pod's summary data in the JSON format and output it to the file `/home/cloud_user/debug/broken-pod-summary.json`

```yaml
kubectl get pod cart-ws -n candy-store -o json > /home/cloud_user/debug/broken-pod-summary.json
```
### Get the broken pod's container logs and output them to the file `/home/cloud_user/debug/broken-pod-logs.log`

```yaml
kubectl logs cart-ws -n candy-store > /home/cloud_user/debug/broken-pod-logs.log
```

### Fix the problem with the broken pod so that it enters the Running state

```yaml
kubectl describe pod cart-ws -n candy-store 
```
Check event, fix the liveness, probe,

```yaml 
kubectl get cart-ws -n candy-store -o yaml --export > broken-pod.yml
```
Now, edit the descriptor file, and fix the path attribute for the liveness probe: vi broken-pod.yml.
```
kubectl delete pod art-ws -n candy-store 
```


Recreate the broken pod with the fixed probe:

```yaml
kubectl apply -f broken-pod.yml -n candy-store
```


# Pod Design - C 
<hr>

## Create 3 pods with names nginx1,nginx2,nginx3. All of them should have the label app=v1 & Show all labels of the pods

```yaml 
kubectl run nginx1 --image=nginx --restart=Never --labels=app=v1 
kubectl run nginx2 --image=nginx --restart=Never --labels=app=v1 
kubectl run nginx3 --image=nginx --restart=Never --labels=app=v1 


kubectl get pod --show-labels 
```



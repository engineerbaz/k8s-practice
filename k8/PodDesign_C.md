# Pod Design - C 
<hr>

## Create 3 pods with names nginx1,nginx2,nginx3. All of them should have the label app=v1 & Show all labels of the pods

Create 3 Pod by imperative way 
```yaml 
kubectl run nginx1 --image=nginx --restart=Never --labels=app=v1 
kubectl run nginx2 --image=nginx --restart=Never --labels=app=v1 
kubectl run nginx3 --image=nginx --restart=Never --labels=app=v1 


kubectl get pod --show-labels 
```
## Change the labels of pod 'nginx2' to be app=v2 & Remove the 'app' label from the pods we created before


```yaml

kubectl label po nginx2 app-

kubectl label po nginx2 app=v2 --overwrite 
```


## Create a pod that will be deployed to a Node that has the label 'accelerator=nvidia-tesla-p100'


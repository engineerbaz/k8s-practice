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

Create a YAML 

```yaml             
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-nodeselector
spec:
  containers:
  - image: nginx
    name: pod-with-nodeselector
  nodeSelector:
    kubernetes.io/hostname: minikube
```
now run` kubectl apply -f pod-c-dg.yaml` 

<hr>
# Deployment

## Create a deployment with image nginx:1.7.8, called nginx, having 2 replicas, defining port 80 as the port that this container exposes (don't create a service for this deployment)

```yaml
kubectl run nginx4 --image=nginx:1.7.8 --replicas=2 --port=80 --dry-run -o yaml
```
View the YAML of this deployment `kubectl get deploy nginx -o yaml`
Check how the deployment rollout is going `kubectl rollout status deploy nginx`

 ## Update the nginx image to nginx:1.7.9

Use command ` kubectl set image deploy nginx4 nginx4=nginx:1.79`

## Check the rollout history and confirm that the replicas are OK
Run `kubectl rollout history deploy nginx`

## Undo the latest rollout and verify that new pods have the old image (nginx:1.7.8)

Run command `kubectl rollout undo deploy nginx4`






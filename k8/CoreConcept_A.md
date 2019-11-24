# Nginx pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: Pod-1
  labels:
    run: app
  namespace: new-ns
spec:
  containers:
  - image: nginx
    name: nginx-new-ns
    ports:
    - containerPort: 80
```

# Create a busybox pod (using YAML) that runs the command "env". Run it and see the output

```yaml 
apiVersion: v1
kind: Pod
metadata:
  name: bb-a1
spec:
  containers:
  - name: busyBox-a1
    image: busybox
    command: 
      - env
```      
# Get the YAML for a new ResourceQuota called 'myrq' with hard limits of 1 CPU, 1G memory and 2 pods without creating it

```yaml
kubectl create quota myrq --hard=cpu=1,memory=1G,pods=2 --dry-run -o yaml
```


# Change pod's image to nginx:1.7.1. Observe that the pod will be killed and recreated as soon as the image gets pulled

Create a pod with image nginx called nginx and allow traffic on port 80
 
`kubectl run nginx --image=nginx --restart=Never --port=80`
change Image using command 
```yaml
kubectl set image pod/nginx nginx=nginx:1.7.1
```

Note: you can check pod's image by running

`kubectl get po nginx -o jsonpath='{.spec.containers[].image}{"\n"}'`

 # Create a busybox pod that echoes 'hello world' and then exits
 
 `kubectl run bb-a2 --image=busybox -it --restart=Never -- /bin/sh -c 'echo hello world'`
  
 

```


# Multi Container Pods

## Create a Pod with two containers,both with image busybox and command "echo hello; sleep 3600". Connect to the second container and run 'ls'

First run command to creaet YAML file for single container
`kubectl run mc-b1 --image=busybox --restart=Never --dry-run -o yaml -- /bin/sh -c "echo Hello World" > mc-b.yaml'

Open file `mc-b.yaml` in editior by `nano mc-b.yaml` and write commands as mentioned below 

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: mc-b1
  name: mc-b
spec:
  containers:
  - args:
    - /bin/sh
    - -c
    - echo hello; sleep 3600
    image: busybox
    name: mc-b1
    resources: {}
  - image: busybox
    name: mc-b2
    args:
    - /bin/sh
    - -c
    - echo hello; sleep 3600
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}

```

Now create _Multi Containe Pod_ by running `kubectl create -f mc-b.yaml`
Now run and check `ls` in second container

```yaml 
kubectl exec -it mc-b -c mc-b2 -- /bin/sh
ls
exit
```


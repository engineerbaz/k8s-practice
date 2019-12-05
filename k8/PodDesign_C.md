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

## Do an on purpose update of the deployment with a wrong image nginx:1.91

`kubectl edit deploy nginx`
change the image to nginx:1.91
vim tip: type (without quotes) '/image' and Enter, to navigate quickly

Run `kubectl rollout status deploy nginx4`


## Return the deployment to the second revision (number 2) and verify the image is nginx:1.7.9

Run `kubectl rollout undo deploy nginx4 --to-revision=2`

## Scale the deployment to 5 replicas

Run `kubectl scale deploy nginx --replicas=5`

## Autoscale the deployment, pods between 5 and 10, targetting CPU utilization at 80%

``kubectl autoscale deploy nginx --min=5 --max=10 --cpu-percent=80`


# Jobs -C

## Create a job with image perl that runs the command with arguments "perl -Mbignum=bpi -wle 'print bpi(2000)'"

```yaml

 kubectl run job-c-dg --image=perl --restart=OnFailure -- perl -Mbignum=bpi -wle 'print bpi(2000)'
```
However, `kubectl run` for Job is Deprecated and will be removed in a future version. What you can do is:

`kubectl create job pi  --image=perl -- perl -Mbignum=bpi -wle 'print bpi(2000)'`

Wait till it's done, get the output
 
run `kubectl get jobs -w` and run `kubectl logs job-c-dg`

output will be like this 

```
3.14159265358979323846264338327950288419716939937510582097494459230781640628620899862803482534211706798214808651328230664709384460955058223172535940812848111745028410270193852110555964462294895493038196442881097566593344612847564823378678316527120190914564856692346034861045432664821339360726024914127372458700660631558817488152092096282925409171536436789259036001133053054882046652138414695194151160943305727036575959195309218611738193261179310511854807446237996274956735188575272489122793818301194912983367336244065664308602139494639522473719070217986094370277053921717629317675238467481846766940513200056812714526356082778577134275778960917363717872146844090122495343014654958537105079227968925892354201995611212902196086403441815981362977477130996051870721134999999837297804995105973173281609631859502445945534690830264252230825334468503526193118817101000313783875288658753320838142061717766914730359825349042875546873115956286388235378759375195778185778053217122680661300192787661119590921642019893809525720106548586327886593615338182796823030195203530185296899577362259941389124972177528347913151557485724245415069595082953311686172785588907509838175463746493931925506040092770167113900984882401285836160356370766010471018194295559619894676783744944825537977472684710404753464620804668425906949129331367702898915210475216205696602405803815019351125338243003558764024749647326391419927260426992279678235478163600934172164121992458631503028618297455570674983850549458858692699569092721079750930295532116534498720275596023648066549911988183479
````

# Create a job with the image busybox that executes the command 'echo hello;sleep 30;echo world'

```yaml
kubectl run 2job-c-dg --image=busybox --restart=OnFailure -- /bin/sh 'echo hello;sleep 30;echo world'

```
Follow the logs for the pod (you'll wait for 30 seconds)

` kubectl logs 2job-c-dg-h4pnn`

Output will be 

```
Hello
World
```

# Create a job but ensure that it will be automatically terminated by kubernetes if it takes more than 30 seconds to execute

```yaml
kubectl run 3job-c-dg --image=busybox --restart=OnFailure --dry-run -o yaml -- /bin/sh "echo hello;sleep 500;" > 3job-c-dg.yaml
```
Open editor `nano 3job-c-dg.yaml`

add `activeDeadlineSeconds: 30` in Yaml

```yaml 
  GNU nano 2.9.3                    3job-c-dg.yaml                    Modified  

apiVersion: batch/v1
kind: Job
metadata:
  creationTimestamp: null
  labels:
    run: 3job-c-dg
  name: 3job-c-dg
        spec:
  activeDeadlineSeconds: 30 // add this line
  template:
    metadata:
      creationTimestamp: null
      labels:
        run: 3job-c-dg
    spec:
      containers:
      - args:
        - /bin/sh
        - echo hello;sleep 500;
```
Now create Job by `kubectl create -f 3job-c-dg.yaml`


## Create the same job, make it run 5 times, one after the other & Create the same job, but make it run 5 parallel times

First write simple pod by `kubectl run 3job-c-dg --image=busybox --restart=OnFailure --dry-run -o yaml -- /bin/sh "echo hello;sleep 500;" > 3job-c-dg.yaml`

then edit for *running 5 times* as `completions: 5` // for running it 5 times.

```yaml

apiVersion: batch/v1
kind: Job
metadata:
  creationTimestamp: null
  labels:
    run: 4job-c-dg
  name: 44job-c-dg
spec:
  completions: 5
  template:
    metadata:
      creationTimestamp: null
      labels:
        run: 4job-c-dg
    spec:
      containers:
      - args:
        - /bin/sh
        - -c
        - echo hello;sleep 30;
        image: busybox
        name: 44job-c-dg
        resources: {}
      restartPolicy: OnFailure
status: {}


```
then edit for *running parallel 5 * as ` parallelism: 5 ` 
```yaml 

apiVersion: batch/v1
kind: Job
metadata:
  creationTimestamp: null
  labels:
    run: 5job-c-dg
  name: 5job-c-dg
spec:
  parallelism: 5
  template:
    metadata:
      creationTimestamp: null
      labels:
        run: 4job-c-dg
    spec:
      containers:
      - args:
        - /bin/sh
        - -c
        - echo hello;sleep 30;
        image: busybox
        name: 5job-c-dg
        resources: {}
      restartPolicy: OnFailure
status: {}



```



# ConfigMap - D

##  Create a configmap named config with values foo=lala,foo2=lolo

```yaml
kubectl create configmap cm-d-dg --from-literal=foo=lala --from-literal=foo2=lolo

```
Check it values.
```
kubectl get cm config -o yaml --export
```
## Create and display a configmap from a file

Create a file first `echo -e "foo3=lili\nfoo4=lele" > cm.txt` 

```yaml
kubectl create configmap cm1-d-dg --from-file=cm.txt 
```
## Create and display a configmap from a .env file

Create ENV file first  `echo -e "var1=val1\n# this is a comment\n\nvar2=val2\n#anothercomment" > config.env`

```yaml
kubectl create configmap cm3-d-dg --from-env-file=config.env 
```
check output by `kubectl get cm cm3-d-dg -o yaml --export`

## Create and display a configmap from a file, giving the key 'special'

Creating file `echo -e "var3=val3\nvar4=val4" > config4.txt`

```yaml
kubectl create configmap cm4-d-dg --from-file=special=config4.txt 

```
##  Create a configMap called 'options' with the value var5=val5. Create a new nginx pod that loads the value from variable 'var5' in an env variable called 'option'

Create ConfigMap first 'kubectl create configmap option --from-literal=var5=val5'
then create a Pod by `kubectl run cm6-ng-d-dg --image=nginx --restart=Never --dry-run -o yaml > cm6-ng-d-dg.yaml` but dont run it 

Edit pod's file in editor

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: cm6-ng-d-dg
  name: cm6-ng-d-dg
spec:
  containers:
  - image: nginx
    name: cm6-ng-d-dg
    env: # add these lines
    - name: option
      valueFrom:
        configMapKeyRef:
          name: options
          key: var5
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```
Check output by `kubectl exec -it cm6-ng-d-dg -- env | grep option`

## Create a configMap 'anotherone' with values 'var6=val6', 'var7=val7'. Load this configMap as env variables into a new nginx pod

1st make ConfigMap ` kubectl create configmap cm7-d-dg --from-literal=var6=val6 --from-literal=var7=val7`

Then run `kubectl run cm7-pod-d-dg --image=nginx --restart=Never -o yaml --dry-run > cm7-pod-d-dg`
Edit it 

```yaml 
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: cm7-pod-d-dg
  name: cm7-pod-d-dg
spec:
  containers:
  - image: nginx
    name: cm7-pod-d-dg
    envFrom:  # differnet then env
     - configMapRef:
         name: anotherone
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}

```

Run it `kubectl create -f cm7-pod-d-dg`

Check by `kubectl exec -it cm7-pod-d-dg -- env`

## Create a configMap 'cmvolume' with values 'var8=val8', 'var9=val9'. Load this as a volume inside an nginx pod on path '/etc/lala'. Create the pod and 'ls' into the '/etc/lala' directory.

Create ConfigMap `kubectl create configmap cm9-d-dg --from-literal=var8=vl8 --from-literal=var9=val9`

then make YAML of Pod `kubectl run cm9-pod-d-dg --image=nginx --restart=Never --dry-run -o yaml > cm9-pod-d-dg.yaml`

open in editor by `nano cm9-d-dg`

```yaml

apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: cm9-pod-d-dg
  name: cm9-pod-d-dg
spec:
  volumes: # add this 
  - name: cmvolume
    configMap:
      name: cm9-d-dg
  containers:
  - image: nginx
    name: cm9-pod-d-dg
    resources: {}
    volumeMounts: # add here
    - name: cmvolume
      mountPath: /etc/lala
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```
create pod by ` kubectl create -f  cm9-pod-d-dg.yaml` 
 
Now check if volume has all required data 
```yaml 
kubectl exec -it  cm9-pod-d-dg -- /bin/sh 
```
Now check data by  
```shell
cd /etc/lala
ls # will show var8 var9
cat var8 # will show val8
```

# SecurityContext

## Create the YAML for an nginx pod that runs with the user ID 101.  

Run this `kubectl run sc-d-dg --image=nginx --restart=Never --dry-run -o yaml > sc-d-d.yaml`
Edit in Nano by `nano sc-c-cc-.yaml`

```yaml

apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: sc-d-dg
  name: sc1-d-dg
spec:
  securityContext:
    runAsUser: 1000 # add this 
  containers:
  - image: nginx
    name: sc1-d-dg
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}

```


## Create the YAML for an nginx pod that has the capabilities "NET_ADMIN", "SYS_TIME" added on its single container

Run this `kubectl run sc-d-dg --image=nginx --restart=Never --dry-run -o yaml > sc-d-d.yaml`
Edit in Nano by `nano sc-c-cc-.yaml`

```yaml

apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: sc-d-dg
  name: sc1-d-dg
spec:
  securityContext:
    runAsUser: 1000 # add this 
  containers:
  - image: nginx
    name: sc1-d-dg
    resources: {}
    securityContext: # insert this line
     capabilities: # and this
       add: ["NET_ADMIN", "SYS_TIME"] # this as well
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}

```
# 



# Secret

1- From file
  Create file first using `echo -n 'admin' > ./username.txt` then run command `kubectl create secret generic db-user-pass --from-file=./username.txt --from-file=./password.txt`
2- From Literal
  Directly creating secret from command `kubectl create secret generic dev-db-secret --from-literal=username=devuser --from-literal=password='S!B\*d$zDsb`
3- Creating Manually
  -- Store string `echo -n "admin" | base64` then add in YAML 
```yaml
      apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  username: YWRtaW4=
  password: MWYyZDFlMmU2N2Rm
```
then run`kubectl apply -f ./secret.yaml`
4- stringData
 -- Store stringData 
`apiUrl: "https://my.api.com/api/v1"
username: "user"
password: "password"`






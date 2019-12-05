# Configuration 

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

















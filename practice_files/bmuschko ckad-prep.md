# 1- Core Concepts
==================== 



## Creating a Pod and Inspecting it
---------------------------------
 
1-
kubectl create namespace 



2- Configuration (18%)
======================


1- 
Configuring a Pod to Use a ConfigMap
---------------------------------------


nano config.txt
-- WRITE ----
DB_URL=localhost:3306
DB_USERNAME=postgres


kubectl create configmap db-config --from-env-file=config.txt
 kubectl run backend --image=nginx --restart=Never -o yaml --dry-run > pod.yaml
vi pod

---
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: backend
  name: backend
spec:
  containers:
  - image: nginx
    name: backend
    envFrom:
      - configMapRef:
          name: db-config
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}

---


 kubectl create -f pod.yaml


kubectl exec backend -it -- /bin/sh


# env | grep DB
DB_URL=localhost:3306
DB_USERNAME=postgres



## 2- Configuring a Pod to Use a Secret

------------

kubectl create secret generic db-credentials --from-literal=db-password=passwd
 
kubectl get secrets

kubectl run backend --image=nginx --restart=Never -o yaml --dry-run > pod.yaml

---
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: backend
  name: backend
spec:
  containers:
  - image: nginx
    name: backend
    env:
      - name: DB_PASSWORD
        valueFrom:
          secretKeyRef:
            name: db-credentials
            key: db-password
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
---


kubectl create -f pod.yaml

kubectl exec -it backend -- /bin/sh

# env | grep DB
DB_PASSWORD=passwd
# exit

===

## 3- Creating a Security Context for a Pod

---------------------------------------------

$ kubectl run secured --image=nginx --restart=Never -o yaml --dry-run > secured.yaml


--

apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: secured
  name: secured
spec:
  securityContext:
    fsGroup: 3000
  containers:
  - image: nginx
    name: secured
    volumeMounts:
    - name: data-vol
      mountPath: /data/app
    resources: {}
  volumes:
  - name: data-vol
    emptyDir: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}

---

$ kubectl create -f secured.yaml
pod/secured created
$ kubectl exec -it secured -- sh
/ # cd /data/app
/ # touch logs.txt
/ # ls -l
-rw-r--r-- 1 root 3000 0 Mar 11 15:56 logs.txt
/ # exit



## 4 - Defining a Pod’s Resource Requirements
-------------------------------------------


--rq.yaml


apiVersion: v1
kind: ResourceQuota
metadata:
  name: app
spec:
  hard:
    pods: "2"
    requests.cpu: "2"
    requests.memory: 500m

---


 kubectl create namespace rq-demo
kubectl create -f rq.yaml --namespace=rq-demo

kubectl describe quota --namespace=rq-demo


pod.yaml


---

apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: mypod
  name: mypod
spec:
  containers:
  - image: nginx
    name: mypod
    resources:
      requests:
        memory: "1G"
        cpu: "400m"
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}

---
kubectl create -f pod.yaml --namespace=rq-demo


-
Error from server (Forbidden): error when creating "2b4_pod.yaml": pods "mypod" is forbidden: exceeded quota: app, requested: requests.memory=1G, used: requests.memory=0, limited: requests.memory=500m
-

## 5- Using a Service Account

----------------

kubectl create serviceaccount backend-team
kubectl get serviceaccounts backend-team -o yaml --export
kubectl run backend --image=nginx --restart=Never --serviceaccount=backend-team
kubectl get pod backend -o yaml --export
kubectl run backend1 --image=nginx --restart=Never --serviceaccount=backend-team
kubectl exec -it backend1 -- /bin/sh


==============

# 3- MULTI CONTAINER

===============


## 1- 

kubectl run adapter --image=busybox --restart=Never -o yaml --dry-run -- /bin/sh -c 'while true; do echo "$(date) | $(du -sh ~)" >> /var/logs/diskspace.txt; sleep 5; done;' > adapter.yaml


---

apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: adapter
  name: adapter
spec:
  volumes:
  - name: config-volume
    emptyDir: {}
  containers:
  - args:
    - /bin/sh
    - -c
    - while true; do echo "$(date) | $(du -sh ~)" >> /var/logs/diskspace.txt; sleep
      5; done;
    image: busybox
    name: adapter
    volumeMounts:
    - name: config-volume
      mountPath: /var/logs
  - args:
    - /bin/sh
    - -c 
    - 'sleep 20; while true; do while read LINE; do echo "$LINE" | cut -f2 -d"|" >> $(date +%Y-%m-%d-%H-%M-%S)-transformed.txt; done < /var/logs/diskspace.txt; sleep 20; done;'
    image: busybox
    name: transformer
    volumeMounts:
       - name: config-volume
         mountPath: /var/logs
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}

---

$ kubectl exec adapter --container=transformer -it -- /bin/sh
/ # ls -l

--------

======================
# 4 - Observability
======================


## 1- Defining a Pod’s Readiness and Liveness Probe

----

$ kubectl run hello --image=bonomat/nodejs-hello-world --restart=Never --port=3000 -o yaml --dry-run > pod.yaml


----

apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: hello
  name: hello
spec:
  containers:
  - image: bonomat/nodejs-hello-world
    name: hello
    ports:
    - name: nodejs-port
      containerPort: 3000
    resources: {}
    readinessProbe:
      httpGet:
        path: /
        port: nodejs-port
      initialDelaySeconds: 2
    livenessProbe:
      httpGet:
        path: /
        port: nodejs-port
      initialDelaySeconds: 5 
      periodSeconds: 8 
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}

---
$ kubectl create -f pod.yaml
pod/hello created
$ kubectl exec hello -it -- /bin/sh
/ # curl localhost:3000


$ kubectl logs pod/hello
Magic happens on port 3000


2- Fixing
------
$ vim pod.yaml
$ kubectl create -f pod.yaml

----

apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: failing-pod
  name: failing-pod
spec:
  containers:
  - args:
    - /bin/sh
    - -c
    - while true; do echo $(date) >> ~/tmp/curr-date.txt; sleep
      5; done;
    image: busybox
    name: failing-pod
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}


----

 kubectl logs failing-pod


$ kubectl exec failing-pod -it -- /bin/sh
/ # mkdir -p ~/tmp/x
/ # cd ~/tmp/x
/ # ls -l



===================================
# 5- POD DESIGN
===================================

## 1- Defining a Pod’s Readiness and Liveness Probe

------


  kubectl run frontend --image=nginx --restart=Never --dry-run -o yaml --labels=env=prod --labels=team=shy
  kubectl run frontend --image=nginx --restart=Never --labels=env=prod,team=shiny
  kubectl get pod/frontend -o yaml 
  k get po
  kubectl run backend --image=nginx --restart=Never --labels=env=prod,team=legacy,app=v1.2.4
  kubectl run database --image=nginx --restart=Never --labels=env=prod,team=storage
  kubectl get pod/database -o yaml 
  kubectl annotate pod/frontend commit=2d3mg3
  kubectl get pod/frontend -o yaml 
  cc
  kubectl annotate pod/backend contact='Mary Haris'
  kubectl get pod/backend -o yaml 
  cc
 kubectl get pod -l 'team in (shiny, legacy)',env=prod --show-labels 
 kubectl get pod  --show-labels 
 kubectl label pods backend env-
 kubectl get pod  --show-labels 
 kubectl get pods -o yaml | grep -C 3 'annotations:'


## 2- Performing Rolling Updates for a Deployment

-----------

kubectl create deployment deploy --image=nginx --dry-run -o yaml > deploy.yaml


--

apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    tier: backend
  name: deploy
spec:
  replicas: 1
  selector:
    matchLabels:
      app: v1
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: v1
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}
---

$ kubectl create -f deploy.yaml
deployment.apps/deploy created
$ kubectl get deployments

$ kubectl set image deployment/deploy nginx=nginx:latest
deployment.extensions/deploy image updated

$ kubectl rollout history deploy



$ kubectl rollout history deploy --revision=2

kubectl rollout undo deployment/deploy --to-revision=1


----



## 3- Creating a Scheduled Container Operation

------

kubectl run current-date --schedule="* * * * *" --restart=OnFailure --image=nginx -- /bin/sh -c 'echo "Current date: $(date)"'

k get cj -w

 kubectl get pods --show-labels
kubectl logs current-date-1557522540-dp8l9

 kubectl get cronjobs current-date -o yaml | grep successfulJobsHistoryLimit:
  successfulJobsHistoryLimit: 3


 kubectl delete cronjob current-date

==============
# 6- Services & Networking 

======================

## 1- Routing Traffic to Pods from Inside and Outside of a Cluster
-------------------------------------


$ kubectl run myapp --image=nginx --restart=Always --replicas=2 --port=80
$ kubectl expose deploy myapp --target-port=80
Determine the cluster IP and use it for the wget command.


$ kubectl run tmp --image=busybox --restart=Never -it --rm -- wget -O- 10.108.88.208:80


Turn the type of the service into NodePort to expose it outside of the cluster. Now, the service should expose a port in the 30000 range.

$ kubectl edit service myapp

--


---


$ wget -O- localhost:30441


## 2- Restricting Access to and from a Pod
-------------------------------------------

Create file app-stack.yaml

-- 

kind: Pod
apiVersion: v1
metadata:
  name: frontend
  namespace: app-stack
  labels:
    app: todo
    tier: frontend
spec:
  containers:
    - name: frontend
      image: nginx

---

kind: Pod
apiVersion: v1
metadata:
  name: backend
  namespace: app-stack
  labels:
    app: todo
    tier: backend
spec:
  containers:
    - name: backend
      image: nginx

---

kind: Pod
apiVersion: v1
metadata:
  name: database
  namespace: app-stack
  labels:
    app: todo
    tier: database
spec:
  containers:
    - name: database
      image: mysql
      env:
      - name: MYSQL_ROOT_PASSWORD
        value: example
        
--

$ kubectl create namespace app-stack

$ vim app-stack.yaml
$ kubectl create -f app-stack.yaml


Create Network Policy as app-stack-network-policy

---

apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: app-stack-network-policy
  namespace: app-stack
spec:
  podSelector:
    matchLabels:
      app: todo
      tier: database
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    ports:
    - protocol: TCP
       port: 3306
    - podSelector:
        matchLabels:
          app: todo
          tier: backend

---
Create the network policy.

$ vim app-stack-network-policy.yaml
$ kubectl create -f app-stack-network-policy.yaml
$ kubectl get networkpolicy --namespace app-stack



=======================
# 7 - STATE persistence
=========================

## 1- Defining and Mounting a PersistentVolume
----------------------------------------------



--

apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv
spec:
  capacity:
    storage: 512m
   accessModes:
     - ReadWriteMany
   storageClassName: shared
   hostPath:
     path: /data/config
     
---

$ kubectl create -f 7e1_pv.yaml
$ kubectl get pv

Now create YAML for Persistent Volume Claim

---

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc
spec:
  storageClassName: shared
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage:256m
  
---

$ kubectl create -f 7e2_pvc.yaml
$ kubectl get pvc

Now Create Pod to Mount PV

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: app
  name: app
spec:
  containers:
  - image: nginx
    name: app
    volumeMounts:
      - mountPath: "/data/app/config"
        name: configpvc
    resources: {}
  volumes:
    - name: configpvc
      persistentVolumeClaim:
        claimName: pvc
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}

```

You can check the events of a Pod with the kubectl describe command. You should see an entry that indicates the successful mount.
























































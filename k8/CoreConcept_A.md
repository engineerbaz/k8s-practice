# Nginx pod

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
--
# Create a busybox pod (using YAML) that runs the command "env". Run it and see the output

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
      
--



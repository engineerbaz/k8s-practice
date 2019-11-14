# Configuring_Kubernetes_Pods

## Candy-service-config.yml
For Config Map

https://gist.github.com/engineerbaz/ee99de08efad825027553326705d73f2

<script src="https://gist.github.com/engineerbaz/ee99de08efad825027553326705d73f2.js"></script>

## db-password-secret.yml
For Secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-password
stringData:
  password: Kub3rn3t3sRul3s!



```

## Candy-service-pod.yml
For Pod


```yaml

apiVersion: v1
kind: Pod
metadata:
  name: candy-service
spec:
  securityContext:
    fsGroup: 2000
  serviceAccountName: candy-svc
  containers:
  - name: candy-pod
    image: linuxacademycontent/candy-service:1
    volumeMounts:
      - name: config-volume
        mountPath: /etc/candy-service
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
    env:
    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: db-password
          key: password
 volumes:
  - name: config-volume
    configMap:
      name: candy-service-config

```

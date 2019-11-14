# Configuring_Kubernetes_Pods

## Candy-service-config.yml
For Config Map

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: candy-service-config
data:
  candy.cfg: |-
    candy.peppermint.power=100000000
    candy.nougat-armor.strength=10
    candy.lemon.acceptability=0
```

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

## Perform a rolling update of the container version

```yaml
kubectl set image deployment/candy-deployment candy-ws=linuxacademycontent/candy-service:3 --record
```

Check Update 
`kubectl rollout status deployment/candy-deployment`


## Roll back to the previous working state

Get a list of previous revisions.

`kubectl rollout history deployment/candy-deployment`

Undo the last revision.

`kubectl rollout undo deployment/candy-deployment`

Check the status of the rollout.

`kubectl rollout status deployment/candy-deployment` 

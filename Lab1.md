# Lab 1

- Create and consume secrets

## Create a secret

kubectl -n <shahgnp> create secret generic app-secret --from-literal=DB_USERNAME=appuser --from-literal=DB_PASSWORD=mypassword

## Create a pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-demo
  namespace: <yourname>
spec:
  containers:
    - name: nginx
      image: nginx
      env:
        - name: DB_USERNAME
          valueFrom:
            secretKeyRef:
              name: app-secret
              key: DB_USERNAME
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: app-secret
              key: DB_PASSWORD
```

## Verify

```bash
kubectl exec secret-demo -- printenv DB_USERNAME
kubectl exec secret-demo -- printenv DB_PASSWORD
```
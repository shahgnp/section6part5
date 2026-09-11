# Lab 2

- Use Azure Key Vault

## Create a Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-demo-<yourname> # change here
  namespace: demo # Don't change this
  labels:
    azure.workload.identity/use: "true"

spec:
  serviceAccountName: keyvault-sa

  containers:
    - name: app
      image: nginx

      volumeMounts:
        - name: secrets-store
          mountPath: /mnt/secrets
          readOnly: true

  volumes:
    - name: secrets-store
      csi:
        driver: secrets-store.csi.k8s.io
        readOnly: true
        volumeAttributes:
          secretProviderClass: azure-keyvault
```

## Verify

```bash
kubectl -n demo exec secret-demo-<yourname> -- ls /mnt/secrets
kubectl -n demo exec secret-demo-<yourname> -- cat /mnt/secrets/db-username
```
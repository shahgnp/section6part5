# Lab 3

- Create ConfigMap and consume it inside the pod


## Create a Config Map

```bash
kubectl -n <yourname> create configmap app-config --from-literal=APP_ENV=production --from-literal=LOG_LEVEL=info --dry-run=client -oyaml >> configmap.yaml
```

Read the file
```bash
cat configmap.yaml
```

Apply
```bash
k apply -f configmap.yaml
```

## Create a pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: configmap-demo
  namespace: <yourname>
spec:
  containers:
    - name: nginx
      image: nginx
      volumeMounts:
        - name: config-volume
          mountPath: /etc/app-config
          readOnly: true

  volumes:
    - name: config-volume
      configMap:
        name: app-config
```

## Verify

```bash
kubectl -n <yourname> exec configmap-demo -- ls -l /etc/app-config
```

```bash
kubectl -n <yourname> exec configmap-demo -- cat /etc/app-config/LOG_LEVEL
kubectl -n <yourname> exec configmap-demo -- cat /etc/app-config/APP_ENV
```

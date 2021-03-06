# configmap-test.yaml

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: configmap-test
  labels:
    app: configmap-test
data:
  config.json: |
    {
      "config": {
        "message": "v1.1" 
      }
    }
---
kind: Service
apiVersion: v1
metadata:
  name: configmap-test
spec:
  selector:
    app: configmap-test
  ports:
    - port: 80
      targetPort: 80
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: configmap-test
spec:
  selector:
    matchLabels:
      app: configmap-test
  template:
    metadata:
      labels:
        app: configmap-test
    spec:
      containers:
        - name: configmap-test
          image: expcat/configmap-test:1.1
          resources:
            limits:
              memory: "128Mi"
              cpu: "500m"
          ports:
            - containerPort: 80
          volumeMounts:
            - name: config
              mountPath: /app/conf
      volumes:
        - name: config
          configMap:
            name: configmap-test
```

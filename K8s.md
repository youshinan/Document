
## ワークロード
- ### Pod
``` yaml {.line-number .copy}
apiVersion: v1
kind: Pod
metadata:
  name: sample
spec:
  containers:
  - name: nginx
    image: nginx:1.17.2-alpine
    volumeMounts:
    - name: storage
      mountPath: /home/nginx
  volumes:
  - name: storage
    hostPath:
      path: "/data/storage"
      type: Directory
```

- ### ReplicaSet
``` yaml {.line-number .copy}
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
      env: study
  template:                   # Podと同じものを記述
    metadata:
      name: nginx
      labels:
        app: web
        env: study
    spec:
      containers:
      - name: nginx
        image: nginx:1.17.2-alpine

```
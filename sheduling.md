#### Создать под, который будет разворачиваться на определенной ноде
<details>

```bash
kubectl run nginx --image=nginx --dry-run=client -oyaml >nginx.yaml
```
* Поле nodeName имя ноды
```yaml
---
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  nodeName: cks-worker2
status: {}
```
  
</details>

#### Запланировать запуск под на нодах с помощью лейблов
<details>

* Добавить лейбл key=val1 на ноду key=val2 на другую ноду
* Запустить два пода, чтобы они запустились на соответствующих нода
``` bash
kubectl label nodes cks-worker key=val  
kubectl label nodes cks-worker1 key=val1
```
```yaml
---
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  nodeSelector:
    key: val
status: {}

---
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx1
  name: nginx1
spec:
  containers:
  - image: nginx
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  nodeSelector:
    key: val1
status: {}
```

* Сделать то же самое с deployment

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: nginx
  name: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
      nodeSelector:
        key: val
status: {}
```
</details>
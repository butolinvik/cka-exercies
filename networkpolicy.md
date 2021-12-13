#### Network Policy
* Создать три namespase web,monitoring,databases
* Создать под nginx в ns web, prometheus в monitoring, postgresql in databases
* Проверить взаимодоступность подов через curl

<details>

```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: null
  name: web
spec: {}
status: {}
---
apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: null
  name: web
spec: {}
status: {}
---
apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: null
  name: databases
spec: {}
status: {}
```

</details>

* Pods
<details>

```yaml

---
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
  namespace: web
spec:
  containers:
  - image: nginx
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
---
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: prometheus
  name: prometheus
  namespace: monitoring
spec:
  containers:
  - image: prom/prometheus
    name: prometheus
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
---
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: postgresql
  name: postgresql
  namespace: databases
spec:
  containers:
  - image: postgres:10.4
    name: postgresql
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
</details>


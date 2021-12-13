#### Network Policy
* Создать три namespase web,monitoring,databases
* Создать под nginx в ns web, prometheus в monitoring, postgresql in databases
* Проверить доступность подов через curl 
* через IP
* через полное dns имя

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
    run: postgresql
  name: postgresql
  namespace: databases
spec:
  containers:
  - image: postgres:10.4
    name: postgresql
  - image: nginx
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
</details>

* Проверка доступности через curl. По айпишнику
<details>

```bash
kubectl get pods -A -owide
kubectl -n web exec nginx -- curl 10.40.0.2:9090  
curl 10.40.0.2:9090
```

</details>

* Проверка доступности через curl по fqdn
<details>

```bash
kubectl -n web exec nginx -- curl prometheus.monitoring.svc.cluster.local:9090  
```

</details>

* Запретить доступ к подам prometheus,nginx,postgresql на исходящие и входящие коннекты

<details>

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-ns-web
  namespace: web
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-ns-monitoring
  namespace: monitoring
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-ns-databases
  namespace: web
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
  ```

</details>
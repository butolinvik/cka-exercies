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
#StrategyType:
#Поменять образ в Deployment
#укзаать или определить, какое количество подов могут быть включены или должны работать.

`
apiVersion: v1 
kind: Pod 
metadata:
  name: ubuntu-sleeper-3 
spec:
  containers:
  - name: ubuntu
    image: ubuntu
    command:
      - "sleep"
      - "1200"
---
apiVersion: v1 
kind: Pod 
metadata:
  name: ubuntu-sleeper-2 
spec:
  containers:
  - name: ubuntu
    image: ubuntu
    command: ["sleep"]
    args: ["5000"]
`
#Command and arguments


# config map
kubectl create configmap webapp-config-map --dry-run=client --from-literal=APP_COLOR=darkblue -oyaml >conf.yaml

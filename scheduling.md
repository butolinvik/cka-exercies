# Добавить лейбл к одной из нод
kubectl label nodes cks-worker1 key=ddr5

# Создать под с образом prometheus . Принудительно запустить его на отмеченной лейблом ноде

apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: prometheus
  name: prometheus
spec:
  containers:
  - image: prom/prometheus
    name: prometheus
  nodeSelector:
    key: ddr5

kubectl create -f prom.yaml
# Обратите внимнаие, если лейбл добавить на мастер-ноду, под уйдет в режим pending. Смотри в descrigbe пода и там написана причина
jjj
# Узнать, установлены ли taint and tolerations
kubectl describe nodes node01
# Создать taint
kubectl taint node node01 spray=mortein:NoSchedule
# Почему не запускается под. 
Предварительно затолерастить ноды и запустить
# Удалить taint
kubectl taint node controlplane node-role.kubernetes.io/master:NoSchedule-
kubectl taint node controlplane key=value1:NoSchedule-


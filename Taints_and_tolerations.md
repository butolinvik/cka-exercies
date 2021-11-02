#Узнать, установлены ли taint and tolerations
kubectl describe nodes node01
#Создать taint
kubectl taint node node01 spray=mortein:NoSchedule
#Почему не запускается под. 
Предварительно затолерастить ноды и запустить
#Удалить taint
kubectl taint node controlplane node-role.kubernetes.io/master:NoSchedule-
kubectl taint node controlplane key=value1:NoSchedule-

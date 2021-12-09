
#Количество лейблов
kubectl get nodes node01 --show-labels
#Добавить лейбл
kubectl label nodes node01 color=blue
 kubectl describe nodes node01 | grep -i blue
#пример разворачивания пода. Означает, что под будет запланирован для запуска только на ноде , на котором есть ключ disktype со значение ssd
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: disktype
            operator: In
            values:
            - ssd            
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent

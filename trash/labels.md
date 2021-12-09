#Labels and Selectors
Добавить лейблы
Поиск по лейблам
kubectl get pods --selector=env=dev
kubectl get pods --selector=env=dev --show-labels
kubectl get all --show-label
kubectl get all --show-labels --selector env=prod
kubectl get pods --show-labels --selector=bu=finance,env=prod,tier=frontend

#ReplicaSet со ссылками на поды создать.
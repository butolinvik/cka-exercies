# Определить режим авторизации у api-server
cat /etc/kubernetes/manifests/kube-apiserver.yaml
--authorization-mode=Node,RBAC
# Узнать, сколько ролей определено в кластере

# Посмотреть по namespace какие роли определены

# Какие ресурсы определены в кластер ролях
kubectl -n kube-system get role kube-proxy -o yaml
# узнать какие права и на что определены в роли
# Создать роль Назначить права
kubectl create role developer --resource=pods --verb=list,create,delete
# создать RoleBinding
kubectl create rolebinding dev-user-binding --role=developer --user=dev-user
# проверить права пользователя
на чтение подов
kubectl get pods --as dev-user
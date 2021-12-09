#### Определить сертификат, используемый kube-apiserver
<details>
> cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep .crt
/etc/kubernetes/pki/apiserver.crt
</details>
# Сертификат для авторизации kube-apiserver на etcd-server
/etc/kubernetes/pki/apiserver-etcd-client.crt
# Ключ для авторизации kube-apiserver для аутентификации на kubelet сервере
/etc/kubernetes/pki/apiserver-kubelet-client.key
# Сертификат для работы etcd сервера
etcd-cafile=/etc/kubernetes/pki/etcd/ca.crt
# Определить корневой сертификат для etcd сервера. При стандартной установке корневой сертификат etcd и kube-apiserver совпадают. Для etcd может быть отдельный корневой центр сертификации
--etcd-cafile=/etc/kubernetes/pki/etcd/ca.crt
# Посмотреть CN в сертификатах
# kube-apiserver
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text

# Кто запросил сертификат для центра сертификации?
# kube-apiserver /etc/kubernetes/pki/ca.crt
openssl x509 -in /etc/kubernetes/pki/ca.crt -text
# Найти Subject Alternative Name для kube-apiserver
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text
# Common Name in ETCD certificate
openssl x509 -in /etc/kubernetes/pki/etcd/server.crt -text
# Срок действия сертификата узнать
Поле Validaty
# Эмулировать неисправность
Поменять путь к сертификату

# Создать запрос для нового пользователя
https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/
openssl genrsa -out myuser.key 2048
openssl req -new -key myuser.key -out myuser.csr
# Приведем к нормальному виду, чтобы в yaml файл запрос засунуть
cat myuser.csr | base64 | tr -d "\n"
metadata:
  name: myuser
spec:
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZqQ0NBVDRDQVFBd0VURVBNQTBHQTFVRUF3d0dZV3R6YUdGNU1JSUJJakFOQmdrcWhraUc5dzBCQVFFRgpBQU9DQVE4QU1JSUJDZ0tDQVFFQXdsOXBXcE5UOGxVM1JSVi9vd0RISmMzeHRqUmJXUytTN0hOWmNYUWNRdUt3CmNhZTd6bjYzU1pLYk80VUdabHAwY1Y3eDA5K2xSaTE0NndVaW00eHQzZ2pVb1ZTYVlFQ1U0ZjZKa1FNaFRDSGcKZ0hnREdITitVQ2xBeDhLWStIOGxLTDhTWE9PamliVUs4MXVFRHdFRVRFUHc2T3pNYzVPcXFMRmdlUldQQmVheApQWlMzM0hvSEZWaXBUR1VVZXpiZ0ZsR3pacGdueTYxOHVldCtqMUdUdUxyMVZML0ZYbTU3YTJTTW93Q1VSU3VaCkd1RjdaZ3BoVHNtYnU0d0ZLamYwbEp6QzllU3BLTVNSVXpNZGY5ZU9JZEE3bDFYYWZ6eHFuOTY1M0VIWVVwOWYKYm1iQnpYT3VwR2xLVmlpdGp5bVgrVE4yUkF1amc5TzRHTWVaVFp1MTF3SURBUUFCb0FBd0RRWUpLb1pJaHZjTgpBUUVMQlFBRGdnRUJBQ0ZmaU5tWnVHLzZsYWVyeGM5ZVI0NG1ZcVFCN0ZkL1VGOFNNMWttbC9jUlpGTGw2NG1LCnVmalhQQnp4UTN4YW5MaXg3dVo4blovREY3ejA5U1RCYkh6aUpHcC9tYWhQV3pvTC84MzlqYjlXSjBlVmdtY2IKZjlUcGtubG4vVnVhQWRWeGVkRUh2a1JZRWNxTE8zcXpzSCtKMUJLMkxvTTZKN0pFbDlzU1ZoZmQxakJKRk8wKwpnT3cxZGZ1MGlJOGlTdW5OT1ZiTHM4RkdIZVd0NE42M2Fwc01ValI5UjJvUFZJbEljUUVWYkhLK1hqKzQwVHZTCjl4emRKOTF3SzFST0tjWDFiM2xlcS9OdDZsVDlvTWo2S1ZEaFBGb210UGVtQzQ5SUtVTmU2ZmxFcHFyWS9oN3EKOGx0Rm1Gd3J0L3AxdFBsaHdXRmgvdzZGK2RtRjNkQWhTQlE9Ci0tLS0tRU5EIENFUlRJRklDQVRFIFJFUVVFU1QtLS0tLQo= 
  signerName: kubernetes.io/kube-apiserver-client
  
  usages:
  - client auth

# Проверить состояние запрос
kubectl get csr
# Разрешить получить сертификат
kubectl certificate approve myuser
# просмотреть существующие запросы

# Узнать. к каким группам был сделан запрос
kubectl get csr myuser -o yaml
# Удалить запрос

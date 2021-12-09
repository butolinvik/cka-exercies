#### Определить сертификат, используемый kube-apiserver
<details>
 cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep .crt /etc/kubernetes/pki/apiserver.crt
</details>

#### Какой сертификат используется для авторизации kube-apiserver на etcd-server
 <details>
/etc/kubernetes/pki/apiserver-etcd-client.crt
</details>

#### Какой ключ для авторизации kube-apiserver для аутентификации на kubelet сервере
<details> 
/etc/kubernetes/pki/apiserver-kubelet-client.key
</details>

#### Определить корневой сертификат для etcd сервера. При стандартной установке корневой сертификат etcd и kube-apiserver совпадают. Для etcd может быть отдельный корневой центр сертификации
<details>
--etcd-cafile=/etc/kubernetes/pki/etcd/ca.crt
</details>

#### Сгегенрировать ключ центра сертификации. (Ключ для корневого центра сертификации)
<details>
openssl genrsa -out ca.key 2048
</details> 

#### Сгегенрировать сертификат центра сертификации. (С ключом, который только что сгенерировали) на 600 дней
<details>
openssl req -x509 -new -nodes -key ca.key -subj "/CN=kubernetes" -days 600 -out ca.crt  

Возможно, придется закоментировать строку vim /etc/ssl/openssl.cnf  
Обратите внимание на subj. Это common name. Должен быть действительным именем доступным по dns. Иначе не заработает  

RANDFILE   = $ENV::HOME/.rnd  
</details> 


#### Посмотреть информацию о сертификатах и ответить на вопросы:
 1) Кто сделал запрос
 2) Какой Common Name
 3) По каким dns имена можно подключиться?
 4) Срок действия сертификата
<details>
Поля  

Issuer: CN =  

Subject: CN =  

x509v3 Subject Alternative Name:    
  DNS:  

Validity    
   Not Before:  
   Not After:  
</details>

### Создать запрос сертификата для для нового пользователя
<details>
https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/ 

```bash
openssl genrsa -out myuser.key 2048  
openssl req -new -key myuser.key -out myuser.csr  
```
* Переведем полученный сертификат в Base64 формат  
```bash
cat myuser.csr | base64 | tr -d "\n"  
```
* Закинем в поле request получившийся текст  
``` yaml
---
metadata:
  name: myuser
spec:
  request: base64 text
  signerName: kubernetes.io/kube-apiserver-client  
  usages:
  - client auth
--- 
```
kubectl create -f 
</detail>

# Проверить состояние запрос
kubectl get csr
# Разрешить получить сертификат
kubectl certificate approve myuser
# просмотреть существующие запросы

# Узнать. к каким группам был сделан запрос
kubectl get csr myuser -o yaml
# Удалить запрос

#### Развернуть nginx ingress controller

<details>

```bash
kubectl apply -f https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/course-content/cluster-setup/secure-ingress/nginx-ingress-controller.yaml  
kubectl get ns  
kubectl -n ingress-nginx get pod,svc  
# К этому моменту мне уже все надоело). Будем считатЬ, что вы что-то уже понимаете. По полученным результатам попытаться получить доступ по http and https
```

</details>

#### Создать два правила для разруливания трафика на два разных веб-сайта

<details>

```yaml
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-test
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - http:
      paths:
      - path: /path1
        pathType: Prefix
        backend:
          service:
            name: path1
            port:
              number: 80
      - path: /path2
        pathType: Prefix
        backend:
          service:
            name: path2
            port:
              number: 80
```
```bash
kubectl get ing  
```
</details>

#### Согласно созданного ингресс создать два под с веб-серверами nginx и httpd.  И разрулить трафик согласно указанных сервисов в ингресс(сервисы надо создать)

<details>

```bash
kubectl run nginx --image=nginx  
kubectl run httpd --image=httpd  
kubectl expose pod nginx --port=80 --name path1  
kubectl expose pod httpd --port=80 --name path2  
```

</details>

#### Накрутим секурности

* Секурность
* Сначала детально посмотрим, что происходит, когда подключение идет через https

<details>

```bash
curl https://cks-master.cyberhelp.pro:30667/path1 -kv  
curl https://cks-master.cyberhelp.pro:30667/path2 -kv

```

</details>
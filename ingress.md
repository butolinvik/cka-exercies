#### Развернуть nginx ingress controller

<details>

```bash
kubectl apply -f https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/course-content/cluster-setup/secure-ingress/nginx-ingress-controller.yaml  
kubectl get ns  
kubectl -n ingress-nginx get pod,svc  
# К этому моменту мне уже все надоело). Будем считатЬ, что вы что-то уже понимаете. По полученным результатам попытаться получить доступ по http and https
```

</details>
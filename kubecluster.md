#### kube-api server
<details>  
* Найти, где находится конфигурация  
```bash  
cat /etc/kubernetes/manifests/kube-apiserver.yaml  
cat /etc/systemd/system/kube-apiserver.service  
ps -aux | grep -i apiserver
```
</details>
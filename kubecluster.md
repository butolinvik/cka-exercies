#### kube-api server
* Найти, где находится конфигурация
<details>  
```bash
 cat /etc/kubernetes/manifests/kube-apiserver.yaml</br>
 cat /etc/systemd/system/kube-apiserver.service </br>
 ps -aux | grep -i apiserver 
```
</details>
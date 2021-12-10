#### kube-api server
<details>  

* Найти, где находится конфигурация  
```bash  
cat /etc/kubernetes/manifests/kube-apiserver.yaml  
cat /etc/systemd/system/kube-apiserver.service  
ps -aux | grep -i apiserver
```  
https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/  
https://kubernetes.io/docs/concepts/overview/components/  
https://kubernetes.io/docs/concepts/overview/kubernetes-api/  
https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/  
https://kubernetes.io/docs/tasks/administer-cluster/access-cluster-api/  

</details>
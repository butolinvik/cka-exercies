#### Создать 2 secrets и добавить к созданному поду
* Прибиндить как файловую систему
* Прибиндить как env var
<details>

```bash  
# sdfasdf
kubectl run nginx --image=nginx --dry-run=client -oyaml >nginx.yaml  
kubectl create secret generic sec --from-literal=user=admin  
kubectl create secret generic sec1 --from-literal=pass=PassWord  
```


```yaml
---
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    resources: {}
    volumeMounts:
    - name: sec
      mountPath: "/etc/secrets"
      readOnly: true
    env:
      - name: PASS
        valueFrom:
          secretKeyRef:
            name: sec1
            key: pass
  volumes:
  - name: sec
    secret:
      secretName: sec
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```  
```bash  
kubectl exec nginx -- cat /etc/secrets/user  
kubectl exec nginx -- env  
```
</details>
 
####  Вскрыть  secrets только что созданные

<details>

```bash
# Определяем сначала где у нас создался под  
kubectl get pods -owide  
critctl ps  
critctl inspect container_id  
#Разделы env and mounts  
crictl inspect 3bd5a79dd7829 | grep pid  
cat /proc/26707/root/etc/secrets/user  
```
</details>

#### Захакать ETCD

<details>

```bash  
cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep etcd  
ETCDCTL_API=3 etcdctl --cert /etc/kubernetes/pki/apiserver-etcd-client.crt --key /etc/kubernetes/pki/apiserver-etcd-client.key --cacert /etc/kubernetes/pki/etcd/ca.crt get /registry/secrets/default/sec1  
ETCDCTL_API=3 etcdctl --cert /etc/kubernetes/pki/apiserver-etcd-client.crt --key /etc/kubernetes/pki/apiserver-etcd-client.key --cacert /etc/kubernetes/pki/etcd/ca.crt get /registry/secrets/default/sec  
# Encription ETCD  
* https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/
cd /etc/kubernetes/  
mkdir etcd  
cd etcd/  
vim ec.yaml  
# Прежде чем заполнить файл, переведем наши данные секретные в base64. -n убирает символ перевода строки
echo -n PassWord | base64  
cd ../manifests/  
vim kube-apiserver.yaml  
# Add encription provider config
 - --encryption-provider-config=/etc/kubernetes/etcd/ec.yaml  
 # После добавляем в volumes директорию с нашим конфигом аналогично pki. Ну в раздел volumeMounts так же добавлям это volume  
 ```
 ```yaml
 ---
 volumes:
 - hostPath:
      path: /etc/kubernetes/etcd
      type: DirectoryOrCreate
    name: etcd
   volumeMounts:
   - mountPath: /etc/kubernetes/etcd
      name: etcd
      readOnly: true
   
 ```
```bash  
# Check  
ps aux | grep apiserver  
cd /var/log/pods/  
# Not working becouse short pass  
# Сначала настроить шифрование на всех нодах. После этого создать secret и его уже не будет видно в etcd  
kubectl create secret generic secure-secret --from-literal pass=1234  
ETCDCTL_API=3 etcdctl --cert /etc/kubernetes/pki/apiserver-etcd-client.crt --key /etc/kubernetes/pki/apiserver-etcd-client.key --cacert /etc/kubernetes/pki/etcd/ca.crt get /registry/secrets/default/secure-secret  
kubectl get secrets  very-secure -oyaml  
# Все-равно видно))
echo MTIzNA== | base64 -d  
# Чтобы зашифровать default secret. Удаляем его и он заново создастся  
# Encript all secrets  
kubectl get secrets -A -oyaml | kubectl replace -f -  
```
</details>
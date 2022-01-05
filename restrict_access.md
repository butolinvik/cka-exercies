#### Anonymous access
* Enable/Disable anonymous acces
<details>

```bash
cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep anonymous-auth  
# true or false  
```

</details>

#### Api Requests Internal and External

<details>

```bash
kubectl config view  
kubectl config view --raw  
echo client-sertificate-data | base64 -d > crt.crt
echo client-key-data  | base64 d >key.key  
curl https://localhost:6443  
curl https://haproxy.cyberhelp.pro:6443 --cacert /etc/kubernetes/pki/ca.crt --cert crt.crt --key key.key  
# In this case i have a loadbalancer  
# External Access  
We can change default service kubernetes to service type NodePort  
# Verify the Node Restriction works  


```


</details>
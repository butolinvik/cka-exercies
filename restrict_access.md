#### Anonymous access
* Enable/Disable anonymous acces
<details>

```bash
cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep anonymous-auth  
# true or false  
```

</details>
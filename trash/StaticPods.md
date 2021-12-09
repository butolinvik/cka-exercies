#Создать статик под
kubectl run static-busybox --image=busybox --dry-run=client --command sleep 1000 -oyaml >/etc/kubernetes/manifests/busybox.yaml
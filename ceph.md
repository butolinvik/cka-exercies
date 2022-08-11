# Delete ns rook-ceph ultimate
```bash
NAMESPACE=rook-ceph
kubectl proxy &
kubectl get namespace $NAMESPACE -o json |jq '.spec = {"finalizers":[]}' >temp.json
curl -k -H "Content-Type: application/json" -X PUT --data-binary @temp.json 127.0.0.1:8001/api/v1/namespaces/$NAMESPACE/finalize
https://programmersought.com/article/91847081545/

# Start
kubectl -n rook-ceph exec -it deploy/rook-ceph-tools -- bash
# Install
apt-get update && sudo apt-get install ceph ceph-mds
# Настройка монитора
https://docs.ceph.com/en/latest/install/manual-deployment/
sudo vim /etc/ceph/ceph.conf

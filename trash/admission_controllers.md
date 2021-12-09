# Посмотреть допустимые плагиры admission controller в kube-apiserver
kubectl exec -n kube-system kube-apiserver-cks-master -- kube-apiserver -h | grep enable-admission-plugins

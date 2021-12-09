# Какой внешний(внутренний) адрес у ноды?
kubectl get nodes -owide
# Какой сетевой интерфейс настроен для подключения к мастер-ноде
kubectl get nodes -owide
ip a
# Mac адрес нод
kubectl get nodes -owide
ip a
# Какой интерфейс использует Docker? Каков его статус?
ip link
# просмотреть маршрут по умолчанию наружу на нодах
ip route show default
# какой порт использует kube-sheduller
netstat -nplt
# Какие порты использует ETCD?
netstat -nplt | grep etcd
# Определить, какой network plugin использует kubelet
ps -aux | grep kubelet
# Путь для поддерживаемых плагинов CNI?
/opt/cni/bin
# Какой плагин CNI сконфигурирован для kubernetes кластера
cat /etc/cni/net.d/nameconfigfile.conflist
# Какой бинарный файл будет запущен после создания котейнера?
cat /etc/cni/net.d/nameconfigfile.conflist
# Настройте weave
https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#-installation
# В какой подсети находятся поды
kubectl -n kube-system logs weave-net-6cws8 weave
# Как развернуты поды в кластере. Как деплоймент или как даймонсет?
kubectl -n kube-system get deployments.apps
kubectl -n kube-system get daemonsets.apps
# Какие сервисы с каками IP развернуты в кластере
kubectl -n kube-system get service
# 
Создать шедулер
Удалить шедулер
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  -  image: nginx
     name: nginx
  nodeName: node01
  #Переделать шедулер, чтобы он запустился на мастер-ноде
  
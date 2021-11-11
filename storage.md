   # Understand storage classes, persistent volumes
# Какие типы volumes существуют на данный момент
# Exercies configMap
--- Создать под nginx, image nginx  который в качестве volumes использует configMap map-web с сключами nginx_base=nginx_base. Если нет данных в configMap, создать его ---
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
    volumeMounts:
      - name: mapvolume
        mountPath: /data
  volumes:
    - name: mapvolume
      configMap:
        name: map-web
        items:
          - key: nginx_base
            path: nginx_base
--- под не запустится, потому что нет объекта configMap. Создадим его ---
apiVersion: v1
kind: ConfigMap
metadata:
  creationTimestamp: null
  name: map-web
data:
  nginx_base: nginx_base
# Exercies emptyDir
---Создать pod prometheus c emtyDir. Данные внутри пода должны быть примонтированы по пути /data---
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: prom
  name: prom
spec:
  containers:
  - image: prom/prometheus
    name: prom
    volumeMounts:
    - mountPath: /data
      name: emptydir
  volumes:
  - name: emptydir
    emptyDir: {}
# Exercies hostPath
-- Желательно не использовать
-- Создать pod nginx c image nginx с volumes hostPath c сохранением в образе папки, файла после пересоздания пода. 2) Без сохранения
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
    volumeMounts:
    - mountPath: /data
      name: data
    - mountPath: /data/1.txt
      name: file
    - mountPath: /data/dd
      name: emptydir
  volumes:
  - name: data
    hostPath:
      path: /data
      type: DirectoryOrCreate
  - name: file
    hostPath:
      path: /data/1.txt
      type: FileOrCreate
  - name: emptydir
    emptyDir: {}
# Exercies local
-- Используется только с persistent volumes--
# Examples projected
-- может подключать разные типы волюмов ---

   # Understand volume mode, access modes and reclaim policies for volumes
   # Understand persistent volume claims primitive
   # Know how to configure applications with persistent storage
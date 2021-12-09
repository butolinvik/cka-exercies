   # Understand storage classes, persistent volumes
   # Volumes
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
   # Persistentvolumes
# Exercies persistent Volumes

-- Создать 4  PV со всеми возможноми уровнями Access Mode c типом nfc, local, hostPath(не в нашу смену). Чтоб затошнило
1 - name: pvcyberhelp, AccessModes: ReadWriteOnce, type local, capacity: 10Gi (Требуется node affinity для local). Лейблы предварительно добавить на необходимую ноду
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pvcyberhelp
spec:
  capacity:
    storage: 10Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Recycle
  storageClassName: slow
  local:
    path: /data
  nodeAffinity:
    required:
      nodeSelectorTerms:
      - matchExpressions:
        - key: pv
          operator: In
          values:
          - pvtest
2 - name: pvcyberhelp, AccessModes: ReadOnlyMany, type nfc, capacity: 10Gi

3 - name: pvcyberhelp, AccessModes: ReadWriteMany, type azureDisk, capacity: 10Gi
4 - name: pvcyberhelp, AccessModes: ReadWriteOncePod, type iscsi, capacity: 10Gi
# Exercies persistent volume claims
-- Создать pvc на основе созданных pv размером 5Gi --
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvccyberhelp
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 5Gi
  storageClassName: slow
  selector:
    matchLabels:
      release: "pvcyberhelp"
   # Storage classes

   # Understand volume mode, access modes and reclaim policies for volumes
   # Understand persistent volume claims primitive
   # Know how to configure applications with persistent storage
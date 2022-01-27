### Здесь будут задания, ориентированные на повышенную сложность. А-ля killes.sh
* Подготовка

### Подготовка
```bash
#Для экономии времени создаем алиасы kubectl и dry-run  
alias k=kubectl  
export do="--dry-run=client -oyaml"  
export now="--force --grace-period 0" # удалить под моментально  
#Редактируем ~/.vimrc, чтобф автоматический таб в vim вел себя нормально, а не делал адского размера пробелы  
set tabstop=2  
set expandtab  
set shiftwith=2  
# Заготовить контексты. Не обязательно рабочие  
kubectl config set-context test1
kubectl config set-context test2
kubectl config set-context test3
kubectl config set-context test4 #etc.
# Прочитать контексты и записать их имена в файл. Написать скрипт для чтения контекстов
k config get-contexts -o name
k config get-contexts -o name >context
echo kubectl config get-contexts -o name > getcontexts.sh | chmod +x getcontexts.sh  
# 2 вариант. Заменяем знаки пробела символом переноса строки
k config view -o=jsonpath='{.contexts[*].name}' | tr " " "\n"  
# прочитать контексты без использования kubectl

```
#### 2 задание
```bash
# Создать под c image nginx в дефолтном неймспейс. Под должен называться mypod. Внутри контейнер должен называться nginxmypod. Под должен запускаться на мастер-ноде. Дополнительные label не использовать 
k run mypod --image=nginx $do >mypod.yaml  
# подсмотрим labels и taints у мастер-нод разными способами. victoria - мастер-нода. victoria1 воркер-нод в данном случа
k describe nodes victoria | grep Taint
k get nodes --show-labels
```
```yaml
---
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: mypod
  name: mypod
spec:
  nodeSelector:
    node-role.kubernetes.io/master: ""
  containers:
  - image: nginx
    name: nginxmypod
    resources: {}
  tolerations:
  - key: "node-role.kubernetes.io/master"
    operator: "Exists"
    effect: "NoSchedule"
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

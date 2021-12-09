#### Создать  под nginx имеративным и декларативным методом с образом nginx:1.16
<details>  

* Imperative
``` bash
kubectl run nginx --image=nginx:1.16
```
* Declarative
``` bash
kubectl run nginx --image=nginx:1.16 --dry-run=client -oyaml >nginx.yaml
kubectl create -f nginx.yaml
```
</details>

#### Поменять образ пода на nginx:1.17
<details>
``` bash
# Один варинат
kubectl edit pod nginx
# Второй вариант
vim nginx.yaml
kubectl replace -f nginx.yaml --force
```
</details>
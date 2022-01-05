#### Подготовить namespace
* Сделать 20 ns. Можно по шаблону скриптом
<details>
```bash  


for (( i =0; i < 20; i++))  
do  
kubectl create ns ns${i}  
done  
* Удалить ns
for (( i =0; i < 20; i++))  
do  
kubectl create ns ns${i}  
done  
</details>

#### Создать роли, удалить роли
<details>

```bash  
#!/bin/bash
for (( i =0; i < 20; i++))
do

kubectl create role role${i} -n ns10 --resource=pod,configmaps --verb=create,delete

done
```

</details>
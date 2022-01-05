#### Подготовить namespace
* Сделать 20 ns. Можно по шаблону скриптом
<details>
```bash
#!/bin/bash  
for (( i =0; i < 20; i++))  
do  
kubectl create ns ns${i}  
done  


</details>
#### Вспоминаем, что такое etcd 
<details>
  * Отказоустойчивая система хранения данных ключ-значение
  Держать в нечетном количестве, ибо это кластер и ноды постоянно друг друга опрашивают.
  Рекомендуется минимум 5 нод
</details>

#### Проверка ETCD не в составе кластера kubernetes. Изучение компонентов. Установите отдельно, чтобы было проще изучать свойства
* Скачать etcd сервер
* Распковать. Запустить без установки. Проверить версию
* Посмотреть возможные параметры запуска
* Сделать пробный запуск
<details>
https://github.com/etcd-io/etcd/releases/tag/v3.5.1
``` bash
https://github.com/etcd-io/etcd/releases/download/v3.5.1/etcd-v3.5.1-linux-amd64.tar.gz

tar xvzf tar xvzf etcd-v3.5.1-linux-amd64.tar.gz

cd etcd-v3.5.1-linux-amd64/

./etcd --version

./etcd -h

#### Если кластер уже установлен, то порты будут заняты, поэтому в качестве эксперимента меняйте порт. Данное упражнение исключительно для поверхностного понимания работы ETCD. В кластере он будет работать в виде набора подов со значениями по-умолчанию. "Отсылка на статические поды"

./etcd --listen-client-urls=http://localhost:2379 --advertise-client-urls=http://localhost:2379

```
</details>

#### Выполнение операций с etcdctl 
* Выполнить бекап etcd
* Добавить пользователя
* Добавить роль
* Изменить пароль пользователя
* ....... далее по желанию
<details>
``` bash
param="--cert=/etc/kubernetes/pki/etcd/server.crt  --key=/etc/kubernetes/pki/etcd/server.key --cacert=/etc/kubernetes/pki/etcd/ca.crt"  

ETCDCTL_API=3 ./etcdctl $param snapshot save /data/backup  

# Можно добавить с какой ноды снимаем бекап  

ETCDCTL_API=3 ./etcdctl --endpoints https://192.168.145.28:2379 $param snapshot save /data/backup  

# Add role  

ETCDCTL_API=3 ./etcdctl $param role add testrole1  

ETCDCTL_API=3 ./etcdctl $param role list  

# Аналогично с пользователем  

## Если у вас уже настроен кластер, то можно не качать etcd и утилиту etcdctl, а запускать напрямую из пода  

kubectl exec etcd-master -n kube-system -- ETCDCTL_API=3 etcdctl -cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt  --key /etc/kubernetes/pki/etcd/server.key" 
```
</details>
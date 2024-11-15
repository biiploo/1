# Установка Linux Oracle на VirtualBox
Установить образ Linux из папки X:\К-ИСП-49\Давыдов АА\Учебные материалы Давыдов

Выделить 4 ядра

Выделить 4096 мб оперативной памяти


# Docker
1. Устанавливает утилиту wget на систему

   `sudo yum install wget`
   
![image](https://github.com/user-attachments/assets/fc492ace-6c3d-406c-a50e-8ce018278012)

2. Скачиваем файл репозитория

   `sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo`
   
![image](https://github.com/user-attachments/assets/3af59fe1-09c4-40a7-907d-4e47953b9e8c)

3. Устанавливаем docker

   `sudo yum install docker-ce docker-ce-cli containerd.io`
   
![image](https://github.com/user-attachments/assets/b25f06fe-ac96-4ef8-b063-e5b7cd97fb07)

4. Запускаем его и разрешаем автозапуск

   `sudo systemctl enable docker --now`
   
![image](https://github.com/user-attachments/assets/0fb933c5-c82f-4752-b6dd-84d0c17bfe2f)

# Сompose
5. Для начала нужно убедиться в наличии пакета curl

   `sudo yum install curl`

![image](https://github.com/user-attachments/assets/09c98cc0-b9af-4291-b86a-e685fd569037)

6. Объявление переменной COMVER, полученной в результате curl запроса, хранящей в себе номер последней версии Docker Compose

   ` COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)`

7. Скачиваем скрипт docker-compose последней версии, используя объявленную ранее переменную и помещаем его в каталог /usr/bin

     `sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose`

 ![image](https://github.com/user-attachments/assets/08c73132-ee66-40e8-b621-3b734f4fe415)

8. Предоставление прав на выполнение файла docker-compose

`sudo chmod +x /usr/bin/docker-compose`

9. Проверка установленной версии Docker Compose

`sudo docker-compose --version`

![image](https://github.com/user-attachments/assets/a921fa68-1937-4827-bc86-9381faa9a4e5)

# Grafana
10. Установка git

`sudo yum install git`

![image](https://github.com/user-attachments/assets/24e00bf7-7a74-4dfa-b774-68893aad4c5a)

11. Этот код скачивает содержимое репозитория skl256/grafana_stack_for_docker

`sudo git clone https://github.com/skl256/grafana_stack_for_docker.git`

![image](https://github.com/user-attachments/assets/1744da67-2d36-4d26-9146-ffbf360b98cb)

12. Заходит в папку - cd

`cd grafana_stack_for_docker`

cd .. - возвращает в папку выше

13. Cоздаем папки двумя разными способами

`sudo mkdir -p /mnt/common_volume/swarm/grafana/config`

`sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data,loki-data,promtail-data}`

14. Выдаем права

`sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}`

15. Создаем файл

`sudo touch /mnt/common_volume/grafana/grafana-config/grafana.ini`

16. Копирование файлов

`sudo cp config/* /mnt/common_volume/swarm/grafana/config/`

17. Переименовывание файла

`sudo mv grafana.yaml docker-compose.yaml`

![image](https://github.com/user-attachments/assets/874a08b1-aa05-4f74-bd12-e984376d569f)

18. Собрать докер (нужно запускать из папки где docker-compose.yaml)

`sudo docker compose up -d`

Опустить докер - sudo docker compose stop

![image](https://github.com/user-attachments/assets/a5692701-827b-47d4-8224-0d45101cd78b)

# Чистка файлов

Команда открывает файл docker-compose.yaml в текстовом редакторе vi с правами суперпользователя

`sudo vi docker-compose.yaml`

Что-бы что-то изменить в тесковом редакторе нужно нажать insert на клавиатуре

Затем в docker-compose нужно вставить node-exporter и удалить ненужные файлы (можно вставить готовый докер)

![image](https://github.com/user-attachments/assets/a7d84595-e64d-45f7-b701-f77dd300d58d)

выйти не сохраняясь из vim - esc :q!

выйти сохраняясь из vim - esc :wq!

Заходим в другую папку

`cd /mnt/common_volume/swarm/grafana/config/`

Открываем файл prometheus.yaml в текстовом редакторе vi с правами суперпользователя

`sudo vi prometheus.yaml`

 команда открывает файл prometheus.yaml в текстовом редакторе vi с правами суперпользователя.

/mnt/common_volume/swarm/grafana/config/prometheus.yaml - исправить targets: на exporter:9100

![image](https://github.com/user-attachments/assets/1d460484-6230-47a1-94ae-e75a506aaaf7)

# Grafana на сайте
- Заходим на сайт localhost:3000

- Регестрируемся на сайте
   - User и Password - admin, потом где он предлагает установить пароль, нажимаем - скип

- Открываем Dashboards - create dashboard - ждем кнопку +Add visualization - configure a new data source - prometheus
   - Connection: http://prometheus:9090
   - Authentication - basic authentication - admin admin
   - save and test

- Dashboards - import dashboard
   - Find and import...: 1860 - кнопка load
   - prometheus - select prometheus
   - import

![image](https://github.com/user-attachments/assets/cf9d318c-9649-4804-880b-6527959e7149)

# VictoriaMetrics
Для начала зайдем в нужную папку

`cd grafana_stack_for_docker`

Открываем docker-compose

`sudo vi docker-compose.yaml`

После prometheus вставляем vmagent (вставлен готовый докер)

![image](https://github.com/user-attachments/assets/872acf3c-7d39-4f56-9709-c58f5d1e5874)

Открываем grafana на сайте и также создаем dashboard, но в Connection пишем http://victoriametrics:8428

Заменяем имя из "Prometheus-2" в "Vika" нажимаем на dashboards add visualition выбираем "Vika" снизу меняем на "code"

В терминале пишем:

`echo -e "# TYPE OILCOINT_metric1 gauge\nOILCOINT_metric1 0" | curl --data-binary @- http://localhost:8428/api/v1/import/prometheus`

команда отправляет бинарные данные (например, метрики в формате Prometheus) на локальный сервер

Потом вводим команду которая делает запрос к API для получения данных по метрике OILCOINT_metric1

`curl -G 'http://localhost:8428/api/v1/query' --data-urlencode 'query=OILCOINT_metric1'`

Значение 0 меняем на любое другое

![image](https://github.com/user-attachments/assets/99a2b9ae-cdad-47c9-92be-01eb62c3f72c)

Заходим на сайт localhost:8428 и открываем vmui

Копируем переменную OILCOINT_metric1 и вставляем в query

![image](https://github.com/user-attachments/assets/0bebcea3-a1af-41d6-9b62-d42ae74e32e1)
![image](https://github.com/user-attachments/assets/dc83100b-25c6-4853-92d4-0d0532fe19be)

Копируем переменную OILCOINT_metric1 и вставляем в code

![image](https://github.com/user-attachments/assets/ef6643e3-5d38-4d33-a580-2b1cb66e4143)

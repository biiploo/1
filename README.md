# Установка Linux Oracle на VirtualBox
Установить образ Linux из папки X:\К-ИСП-49\Давыдов АА\Учебные материалы Давыдов

Выделить 4 ядра

Выделить 4096 мб оперативной памяти


# Docker
1. Устанавливает утилиту wget на систему

   `sudo yum install wget`
   
![image](https://github.com/user-attachments/assets/fc492ace-6c3d-406c-a50e-8ce018278012)

3. Скачиваем файл репозитория

   `sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo`
   
![image](https://github.com/user-attachments/assets/3af59fe1-09c4-40a7-907d-4e47953b9e8c)

5. Устанавливаем docker

   `sudo yum install docker-ce docker-ce-cli containerd.io`
   
![image](https://github.com/user-attachments/assets/b25f06fe-ac96-4ef8-b063-e5b7cd97fb07)

7. Запускаем его и разрешаем автозапуск

   `sudo systemctl enable docker --now`
   
![image](https://github.com/user-attachments/assets/0fb933c5-c82f-4752-b6dd-84d0c17bfe2f)

# Сompose
8. Для начала нужно убедиться в наличии пакета curl

   `sudo yum install curl`

![image](https://github.com/user-attachments/assets/09c98cc0-b9af-4291-b86a-e685fd569037)

9. Объявление переменной COMVER, полученной в результате curl запроса, хранящей в себе номер последней версии Docker Compose

   ` COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)`

 10. Скачиваем скрипт docker-compose последней версии, используя объявленную ранее переменную и помещаем его в каталог /usr/bin

     `sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose`

 ![image](https://github.com/user-attachments/assets/08c73132-ee66-40e8-b621-3b734f4fe415)

11. Предоставление прав на выполнение файла docker-compose

`sudo chmod +x /usr/bin/docker-compose`

12. Проверка установленной версии Docker Compose

`sudo docker-compose --version`

![image](https://github.com/user-attachments/assets/a921fa68-1937-4827-bc86-9381faa9a4e5)

# Grafana
13. Установка git

`sudo yum install git`

![image](https://github.com/user-attachments/assets/24e00bf7-7a74-4dfa-b774-68893aad4c5a)

14. Этот код скачивает содержимое репозитория skl256/grafana_stack_for_docker

`sudo git clone https://github.com/skl256/grafana_stack_for_docker.git`

![image](https://github.com/user-attachments/assets/1744da67-2d36-4d26-9146-ffbf360b98cb)

15. Заходит в папку - cd

`cd grafana_stack_for_docker`

cd .. - возвращает в папку выше

Cоздаем папки двумя разными способами

`sudo mkdir -p /mnt/common_volume/swarm/grafana/config`

`sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data,loki-data,promtail-data}`

Выдаем права

`sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}`

Создаем файл

`sudo touch /mnt/common_volume/grafana/grafana-config/grafana.ini`

Копирование файлов

`sudo cp config/* /mnt/common_volume/swarm/grafana/config/`

Переименовывание файла

`sudo mv grafana.yaml docker-compose.yaml`

![image](https://github.com/user-attachments/assets/874a08b1-aa05-4f74-bd12-e984376d569f)

16. Собрать докер (нужно запускать из папки где docker-compose.yaml)

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


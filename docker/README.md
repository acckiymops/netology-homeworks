## Задача 1

Сценарий выполнения задачи:
- Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
```bash
mvmeles1@ubuntu:~$ docker -v
Docker version 28.5.1, build e180ab8
```
- Если dockerhub недоступен создайте файл /etc/docker/daemon.json с содержимым: ```{"registry-mirrors": ["https://mirror.gcr.io", "https://daocloud.io", "https://c.163.com/", "https://registry.docker-cn.com"]}```
- Зарегистрируйтесь и создайте публичный репозиторий  с именем "custom-nginx" на https://hub.docker.com (ТОЛЬКО ЕСЛИ У ВАС ЕСТЬ ДОСТУП);
```
https://hub.docker.com/repository/docker/acckiymops/custom-nginx/general
```
- скачайте образ nginx:1.21.1;
```bash
mvmeles1@ubuntu:~$ sudo docker pull nginx:1.21.1
1.21.1: Pulling from library/nginx
a330b6cecb98: Pull complete 
5ef80e6f29b5: Pull complete 
f699b0db74e3: Pull complete 
0f701a34c55e: Pull complete 
3229dce7b89c: Pull complete 
ddb78cb2d047: Pull complete 
Digest: sha256:a05b0cdd4fc1be3b224ba9662ebdf98fe44c09c0c9215b45f84344c12867002e
Status: Downloaded newer image for nginx:1.21.1
docker.io/library/nginx:1.21.1
mvmeles1@ubuntu:~$ sudo docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
nginx        1.21.1    822b7ec2aaf2   4 years ago   133MB
```
- Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы(/usr/share/nginx/html/index.html), на файл index.html с содержимым:
```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
```
- Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0 (ТОЛЬКО ЕСЛИ ЕСТЬ ДОСТУП). 
- Предоставьте ответ в виде ссылки на https://hub.docker.com/<username_repo>/custom-nginx/general.
```
https://hub.docker.com/repository/docker/acckiymops/custom-nginx/general
```

## Задача 2
1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
- имя контейнера "ФИО-custom-nginx-t2"
- контейнер работает в фоне
- контейнер опубликован на порту хост системы 127.0.0.1:8080
2. Не удаляя, переименуйте контейнер в "custom-nginx-t2"
3. Выполните команду ```date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html```
4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

  <img width="1877" height="382" alt="image" src="https://github.com/user-attachments/assets/70ae13b9-c931-4dfd-8595-9e015ce52cdc" />


## Задача 3
1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
2. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
3. Выполните ```docker ps -a``` и объясните своими словами почему контейнер остановился.
  <img width="1194" height="325" alt="image" src="https://github.com/user-attachments/assets/4394cb36-2651-4724-864c-13970af887d8" />

  ***Контейнер остановился, т.к. мы подключились к его основному процессу и комбинацией Ctrl+C отправили сигнал процессу для завершения***

4. Перезапустите контейнер
5. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
   <img width="503" height="60" alt="image" src="https://github.com/user-attachments/assets/71db1f35-4f07-443a-98ca-69d321059ed7" />

6. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
   <img width="747" height="400" alt="image" src="https://github.com/user-attachments/assets/f204781c-ab77-41f1-b075-2df3ec326832" />

7. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
   <img width="534" height="61" alt="image" src="https://github.com/user-attachments/assets/311333c2-33e5-4b10-8cc6-e6670a2818c7" />

8. Запомните(!) и выполните команду ```nginx -s reload```, а затем внутри контейнера ```curl http://127.0.0.1:80 ; curl http://127.0.0.1:81```.
9. Выйдите из контейнера, набрав в консоли  ```exit``` или Ctrl-D.
    <img width="599" height="289" alt="image" src="https://github.com/user-attachments/assets/8df72825-d1b9-4493-9877-891250bb66cd" />

10. Проверьте вывод команд: ```ss -tlpn | grep 127.0.0.1:8080``` , ```docker port custom-nginx-t2```, ```curl http://127.0.0.1:8080```. Кратко объясните суть возникшей проблемы.
    <img width="796" height="140" alt="image" src="https://github.com/user-attachments/assets/8ac3223f-f080-47e4-9cb4-b6b270873462" />

    ***Nginx в контейнере после изменения конфигурации слушает 81 порт, а из контейнера пробрасывается 80 порт на 8080 локального хоста***

12. * Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно. [пример источника](https://www.baeldung.com/linux/assign-port-docker-container)
    <img width="921" height="151" alt="image" src="https://github.com/user-attachments/assets/eac6f6bd-4c32-4a54-a46c-08a0f914c5c6" />
    <img width="929" height="288" alt="image" src="https://github.com/user-attachments/assets/d8a87163-d18b-4f0f-8d0f-f9c88c9d80d9" />
    <img width="1043" height="24" alt="image" src="https://github.com/user-attachments/assets/6ecaf7bc-9ff0-4d6e-9b44-08511e91ed69" />
    <img width="1911" height="239" alt="image" src="https://github.com/user-attachments/assets/e7a5bf30-acfd-4ae8-a5fc-f507602c4a83" />
    <img width="1196" height="274" alt="image" src="https://github.com/user-attachments/assets/f9b169f8-08b2-45a6-bb31-1ecf4d038024" />

13. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)
    <img width="592" height="108" alt="image" src="https://github.com/user-attachments/assets/713b2212-9d86-4083-87ff-815e76c0357b" />

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

## Задача 4

- Запустите первый контейнер из образа ***centos*** c любым тегом в фоновом режиме, подключив папку  текущий рабочий каталог ```$(pwd)``` на хостовой машине в ```/data``` контейнера, используя ключ -v.
- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив текущий рабочий каталог ```$(pwd)``` в ```/data``` контейнера. 
- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```.
- Добавьте ещё один файл в текущий каталог ```$(pwd)``` на хостовой машине.
- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера.

 <img width="931" height="218" alt="image" src="https://github.com/user-attachments/assets/3862bd62-9429-425a-a451-12fc04856913" />
 <img width="928" height="426" alt="image" src="https://github.com/user-attachments/assets/74b582d4-29e8-4f2b-8449-7943aa7ce848" />




В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.


## Задача 5

1. Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него.
"compose.yaml" с содержимым:
```
version: "3"
services:
  portainer:
    network_mode: host
    image: portainer/portainer-ce:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```
"docker-compose.yaml" с содержимым:
```
version: "3"
services:
  registry:
    image: registry:2

    ports:
    - "5000:5000"
```

И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-application-model/#the-compose-file )
<img width="1250" height="674" alt="image" src="https://github.com/user-attachments/assets/c87e7d86-e474-480d-8344-baf1c2e1f31c" />
***Поддерживаются оба формата, но при наличии двух и более файлов docker compose предпочитает формат compose.yaml***

2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)
   <img width="1692" height="362" alt="image" src="https://github.com/user-attachments/assets/cbb8837c-812b-4ced-82b6-dea3a55ec585" />


4. Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/
   ***Команды выполнялись в Ubuntu c GUI***
   <img width="702" height="462" alt="image" src="https://github.com/user-attachments/assets/8d66102a-1598-436f-9bbe-fce3fce60e29" />

6. Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)
   <img width="704" height="486" alt="image" src="https://github.com/user-attachments/assets/21c36792-2a43-46cc-84ea-2d87e0b3fa1f" />

8. Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local  окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

```
version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"
```
<img width="852" height="498" alt="image" src="https://github.com/user-attachments/assets/3e8e0334-9752-4ffd-934d-f9f9904bc011" />


6. Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".
   <img width="661" height="861" alt="image" src="https://github.com/user-attachments/assets/14f56f63-69a7-4308-a92a-fc4711223abb" />


8. Удалите любой из манифестов компоуза(например compose.yaml).  Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.

---

### Правила приема

Домашнее задание выполните в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.


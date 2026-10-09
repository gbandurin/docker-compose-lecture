## Learning Docker

### Что происходит?

В этом проекте мы изучаем docker и docker compose на примере проекта с разными сервисами.
Также этот проект подойдет для мониторинга собственного маленького VPS или домашнего сервера.

### Что мы узнаем?

- Узнаем как работает docker;
- Узнаем как работает docker compose;
- Познакомимся с директивами dockerfile, docker-compose.yml;
- Посмотрим на нюансы настройки;
- Займемся дебагом того, что не заведется сразу.

### Два проекта:
* Prometheus + Grafana
* Zabbix

### Дополнительные файлы

``example_daemon.json`` - файл настройки Docker Daemon.
Должен распологаться в `/etc/docker/daemon.json`, при изменении нужно перезапустить docker командой: 
`systemctl restart docker`

### Текущий состав сервисов:

| Prometheus + Grafana                                                               | Zabbix + Qdrant                                                                |
| ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| - Prometheus<br>- Grafana<br>- PostgreSQL DB (for Grafana data)<br>- Node exporter | - PostgreSQL DB (for Zabbix data)<br>- Zabbix server<br>- Zabbix front (nginx) |


### Как запустить?
```
# Склонируем себе проект и перейдем в папку:
git clone https://github.com/gbandurin/docker-compose-lecture.git
cd docker-compose-lecture/<project>

# Создадим директории для данных (volume):
mkdir grafana_data postgres_data prometheus_data

# Изменим права на директории так, чтобы процессы внутри контейнера имели к ним доступ на чтение и запись:
chown -R 472:472 grafana_data
chown -R 999:root postgres_data
chown -R 99:99 prometheus_data
```

Далее нужно скопировать или переименовать файл переменных `.env.EXAMPLE` в `.env` и уже в нём задать следующие переменные:
 * *POSTGRES_PASSWORD* - пароль доступа Grafana к БД - сгенерируйте, например командой `pwgen -s 16`
 * *GF_SECURITY_ADMIN_PASSWORD* - пароль УЗ admin для входа в Grafana - сгенерируйте, например командой `pwgen -s 16`
 * *GF_SECURITY_SECRET_KEY* - шифрование данных в БД, сгенерируйте командой `openssl rand -base64 32`


```
docker compose up -d
docker compose ps # Посмотрим список запустившихся контейнеров и их статус
```

Если всё сделано правильно, Grafana будет доступна по адресу хоста на порту :3000.
Используйте логин admin и пароль из переменной GF_SECURITY_ADMIN_PASSWORD.

### Если на хосте не установлены pwgen и openssl
```
sudo apt update && sudo apt install pwgen openssl
```





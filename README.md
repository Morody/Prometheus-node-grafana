# Prometheus & Grafana & Node-exporter & docker-compose
## Элементы разворачиваемой инфаструктуры
+ Контейнер образа prom/prometheus
+ Контейнер образа prom/node-exporter
+ Контейнер образа grafana/grafana
+ Контейнер приложения
___
## Deploy
Для разворачивания проекта необходимо установить `docker-compose`

```
docker-compose up -d
```
Устанавливаются необходимые образы, зависимости для работы приложения.

`docker-compose` использует тип сети `bridge` и создает между всеми контейнерами одну сеть - `monitoring`
___
## Элементы мониторинга
### Prometheus
Для того чтобы открыть панель Prometheus нужно перейти `http://localhost:9090`

Для просмотра таргетов prometheus нужно перейти `http://localhost:9090/targets`

![](https://github.com/Morody/Prometheus-node-grafana/blob/master/img/6.png?raw=true)

Управление таргетами осуществляется через `prometheus.yml`

В файле проекта сбор целевых метрик осуществляется `node-exporter` по `https://app:9200/metrics` - приложение.
### Node exporter
В `prometheus.yml` в качестве целевого таргета определен `https://app:9200` - приложение, мониторинг которого осуществляется в стеки
### Grafana
Логин - admin.
Пароль - foobar
Используется для визуализации полученных метрик. В проекте привентивно определены `datasources` и `dashborads`
`grafana/privisioning/datasources` и `grafana/privisioning/dashboards`. Эти директории монтируется уже в сам контейнер.

В качестве источника данных определен адрес `https://prometheus:9090`
![](https://github.com/Morody/Prometheus-node-grafana/blob/master/img/4.png?raw=true)
В качестве панели мониторинга определен дашборд, который собирает метрику active_users - количество пользователей разных категорий - и суммирует её.
![](https://github.com/Morody/Prometheus-node-grafana/blob/master/img/5.png?raw=true)
![](https://github.com/Morody/Prometheus-node-grafana/blob/master/img/2.png?raw=true)
Ее нельзя удалить непосредственно из самой панели мониторинга. Поменять дашборд или удалить можно через папку `grafana/privisioning/dashboards`, файл `Monitoring-1571332751387.json`
![](https://github.com/Morody/Prometheus-node-grafana/blob/master/img/3.png?raw=true)
## App
Суть приложения в том, что оно генерирует метрику "активные пользователи" по каждой категории. Генерация метрик происходит по `https://localhost:9200/metrics`
![](https://github.com/Morody/Prometheus-node-grafana/blob/master/img/1.png?raw=true)

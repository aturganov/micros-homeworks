
# Домашнее задание к занятию "Микросервисы: принципы"

Вы работаете в крупной компанию, которая строит систему на основе микросервисной архитектуры.
Вам как DevOps специалисту необходимо выдвинуть предложение по организации инфраструктуры, для разработки и эксплуатации.

## Задача 1: API Gateway 

Предложите решение для обеспечения реализации API Gateway. Составьте сравнительную таблицу возможностей различных программных решений. На основе таблицы сделайте выбор решения.

Решение должно соответствовать следующим требованиям:
- Маршрутизация запросов к нужному сервису на основе конфигурации
- Возможность проверки аутентификационной информации в запросах
- Обеспечение терминации HTTPS

Обоснуйте свой выбор.

https://www.techtarget.com/searchapparchitecture/feature/A-feature-rundown-of-6-popular-API-gateway-tools

https://www.nginx.com/blog/building-microservices-using-an-api-gateway/

````
Выбор решения для России, видимо, за последний год приобрело свою специфику. 
Вижу, что стали чаще фокусироваться большше на Open Source решениях 
на арендованном/собственном железе, либо если смотреть на сервисы облачные 
российского происхождения, как Яндекс, Сбер, VK, иные площадки.

Если внутреннее решение, то выбирал был исходя из сложности требуемого решения. 
Базовое NGINX, а далее скорее технологически с точки зрения поддержки/доработки, 
в частности, Tyk написан на Go. А Konga на скриптом языке Lua. Довольно специфический продукт.

Если сервисы базируется на облаках, включая интеграцию, 
то выбор с учетом рисков санкций не столь велик и из самых проработанных, 
разве, что Яндекс. Также надо, исходить из специфики самих микросервисов, 
на базе каких облачных решений они строятся и, соотвественно, выбирать облачного провайдера.

Поскольку требования базовые, выбрал бы NGINX.
````
Решение | Маршрутизация | Аутентификация | Терминация HTTPS 
 --- | --- | --- | ---
NGINX|+|+|+
Konga Gateway (based on NGINX, Apache)|+|+|+
Tyk API Gateway|+|+|+
```
Облачные решения от основных провайдеров:
```
Решение | Маршрутизация | Аутентификация | Терминация HTTPS 
 --- | --- | --- | ---
Yandex API Gateway
SBER API Gateway|+|+|+
VK API Gateway (based on mail.ru)|+|+|+
```
Решения под вопросом, ввиду возможных санкциЙ:
```
Решение|Маршрутизация|Аутентификация|Терминация HTTPS
--- | --- | --- | ---
Azure API Gateway|+|+|+
Amazon API Gateway|+|+|+
Oracle API Gateway|+|+|+

## Задача 2: Брокер сообщений

Составьте таблицу возможностей различных брокеров сообщений. На основе таблицы сделайте обоснованный выбор решения.

Решение должно соответствовать следующим требованиям:
- Поддержка кластеризации для обеспечения надежности
- Хранение сообщений на диске в процессе доставки
- Высокая скорость работы
- Поддержка различных форматов сообщений
- Разделение прав доступа к различным потокам сообщений
- Протота эксплуатации

Обоснуйте свой выбор.
````
Перечислю список брокеров, которые в основном рассматриваются в России при построении решений, 
облачные решения как Amazon не рассматривал, ввиду рисков. 
По сути Kafka - это для производительных систем и потоков, RabbitMQ - производительная система, 
медленнее чем Kafka, и где требуется разветвленная маршрутизация, 
Редис - скорее для краткосрочных сообщений, не рассчитанных на длительное хранение. 
Также - это самое базовое решение.

Я бы выбрал Kafka, если требовалась поддержка различных систем и форматов, 
и как наиболее универсальная система, с хранением сообщений и логированием.
````
Решение|Кластеризация|Хранение|Скорость|Различные форматы|	Права|Простота
---|---|---|---|---|---|---
Kafka(Apache)|+|+|+-|+|+|-+
RabbitMQ|+|+-|+-|+|+|+-
Redis|+|-+|+|-+|-+|+|

## Задача 3: API Gateway * (необязательная)

### Есть три сервиса:

**minio**
- Хранит загруженные файлы в бакете images
- S3 протокол

**uploader**
- Принимает файл, если он картинка сжимает и загружает его в minio
- POST /v1/upload

**security**
- Регистрация пользователя POST /v1/user
- Получение информации о пользователе GET /v1/user
- Логин пользователя POST /v1/token
- Проверка токена GET /v1/token/validation

### Необходимо воспользоваться любым балансировщиком и сделать API Gateway:

**POST /v1/register**
- Анонимный доступ.
- Запрос направляется в сервис security POST /v1/user

**POST /v1/token**
- Анонимный доступ.
- Запрос направляется в сервис security POST /v1/token

**GET /v1/user**
- Проверка токена. Токен ожидается в заголовке Authorization. Токен проверяется через вызов сервиса security GET /v1/token/validation/
- Запрос направляется в сервис security GET /v1/user

**POST /v1/upload**
- Проверка токена. Токен ожидается в заголовке Authorization. Токен проверяется через вызов сервиса security GET /v1/token/validation/
- Запрос направляется в сервис uploader POST /v1/upload

**GET /v1/user/{image}**
- Проверка токена. Токен ожидается в заголовке Authorization. Токен проверяется через вызов сервиса security GET /v1/token/validation/
- Запрос направляется в сервис minio  GET /images/{image}

### Ожидаемый результат

Результатом выполнения задачи должен быть docker compose файл запустив который можно локально выполнить следующие команды с успешным результатом.
Предполагается что для реализации API Gateway будет написан конфиг для NGinx или другого балансировщика нагрузки который будет запущен как сервис через docker-compose и будет обеспечивать балансировку и проверку аутентификации входящих запросов.
Авторизаци
curl -X POST -H 'Content-Type: application/json' -d '{"login":"bob", "password":"qwe123"}' http://localhost/token

**Загрузка файла**

curl -X POST -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJib2IifQ.hiMVLmssoTsy1MqbmIoviDeFPvo-nCd92d4UFiN2O2I' -H 'Content-Type: octet/stream' --data-binary @yourfilename.jpg http://localhost/upload

**Получение файла**
curl -X GET http://localhost/images/4e6df220-295e-4231-82bc-45e4b1484430.jpg

---

#### [Дополнительные материалы: как запускать, как тестировать, как проверить](https://github.com/netology-code/devkub-homeworks/tree/main/11-microservices-02-principles)

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---

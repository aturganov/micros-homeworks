Дополнительный вопрос 2:
```
Артем, вы так и не поняли суть вопроса. Какие проблемы нужно решить в первую очередь при переходе с монолита на микросервисы?
```
Ответ:
```
Касательно технического аспекта перехода с монолита на микросервисную архитектуру.
1. Монолит - это обычно ситуация, когда код связан с разными частями. Связанность бизнес логики.
И первая проблема - разделить функции на самостоятельные. Выделить в микросервис.
Формируется план архитектуры деления на самостоятельные блоки, исходя из процессов,
далее отцениваются вопросы насколько возможно переиспользовать текущий код монолита
при формировании микросервисов (самостоятельных блоков). 
Необходимость рефакторинга кода. Принимаются решения о разделении, 
исходя из бизнес-логики и технического целесообразности, возможности. Проводится деления кода монолита. 
В некоторых случаях код настолько связан 
с другими частями, что при формировании микросервиса необходмои писать сервис заново, с нуля.
2. В сложных проектах, кроме как виделить в самостоятельные части проблема - это 
сложность самого репозитария. Может быть много проектов (веток -git), и взять и разделить репу на сервисы довольно сложно.
3. Еще одна задача - это выбор инфраструктурного решения, платформы для формирование микросервисов, копирование кода по микросервисам. 
Настройка. Условно, если выбрали Кубер, под него надо создать проекты по микросервисам, распределить код логику из монолита 
и связать на уровне самостоятельных частей сервисы (оперделить мощности, задать докер-композы, в общую подсеть связать, определить протоколы дата обмена, обвязать новый контур безопасности, разделить на несколько баз данных и так далеее). Задать модель интеграции и оркестрации. 
4. Организационная тема. Разделить команду по сервисам, по заказчикам 
и усилить devops.
```
Дополнительный вопрос:
```
Артем, спасибо за работу но в доработку
и какие проблемы необходимо будет решить в первую очередь.
```
Ответ:
```
Ну судя по инет магазину, вернее трех, где я занимаюсь аналитикой,
был болезненный период, когда элементарно компания уперлась в предел монолита, нельзя ввести более продвинутую функцию
ценнообразования ввести и так далее, нельзя масштабировать, нужно перетестировать весь монолит. Сложно добавить новые подсерсисы, например - логика рекомендаций.

Т.е. что закрывали:

Добавление новых процессов. Вшивать в монолит стало совсем трудоемко.

Ускорить по сути разработку и перенос на прод за счет концентрации на слабо зависимых блоков(микросервисов). Организационно команды могут дорабатывать независимо. Сократить трудоемкость тестирования юнитов.

Стабилизировать работу, ввиду флуктурующие нагрузки, особенно, в пики спроса.
```

# Домашнее задание к занятию "Введение в микросервисы"

## Задача 1: Интернет Магазин

Руководство крупного интернет магазина у которого постоянно растёт пользовательская база и количество заказов рассматривает возможность переделки своей внутренней ИТ системы на основе микросервисов. 

Вас пригласили в качестве консультанта для оценки целесообразности перехода на микросервисную архитектуру. 

Опишите какие выгоды может получить компания от перехода на микросервисную архитектуру и какие проблемы необходимо будет решить в первую очередь.
```
Для поддержания интернет торговли требуется организавовать ряд самостоятельных процессов:
- Закупочная деятельность
- Ценнообразование
- Управление каталогами товаров 
- Организация поисковика товаров
- Организация личном кабинетом, корзиной, оформление заказа
- Платежи
- Маркетинговая активность (акции, бонусные программы)
- Персонализация сайта для клиента под его интересы
- Хранилище данных и отчетность
- Отзывы
- Логистика доставки клиенту

С ростом продаж микросервисная архитектура позволяет масштабировать решение, а путем выделение процессов в микросервисы ускорить внедрение и развитие по сравнению с монолитным решением.
А тажке получить следующие преимущества:
- Развитие интеграции с партнерскими системами
- Ввод новых процессов, расширяющих бизнес
- Сбалансированное управление нагрузкой при ппиковых продажах
- Фокусировка на развитие работы процессов и подразделений, соотвествие структуре подразделения (закон Конвея)
- Более простой способ развертывания новых изменений по сравнению с монолитом
- Более высокая устойчивость к ошибкам, локализация проблемы на работу на уровне микросервиса
- Совместимость с API
---
```
### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---

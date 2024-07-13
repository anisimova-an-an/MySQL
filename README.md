# MySQL

В данном документе представлено описание работы с базой данных с помощью MySQL Workbench

## Оглавление
1. [Описание проекта](#описание-проекта)
2. [Создание и удаление таблиц](#создание-и-удаление-таблиц)
   - Создание таблицы с помощью команды ***Create***
   - Создание таблиц с помощью ***ERR диаграммы***
   - Удаление таблицы
4. [Команды манипуляции данными в таблице](#майнд-карта)
   - [Select]()
   - [Insert]()
   - [Update]()
   - [Delete]()
   - [Select]()
5. [Коллекция POSTMAN](#коллекция-postman)
6. [Автотесты](#автотесты)
   
## Описание проекта

DummyAPI (https://dummyapi.io/) является сервисом для тестирования API. Для выполнения каждого запроса необходим app-id, который можно получить при регистрации на сайте и передать через хедер.
В качестве тестирования взят объект **Post**

### Создание и удаление таблиц
______
#### Создание таблицы с помощью команды ***Create***
```sql
CREATE TABLE shop.product (
id_products INT NOT NULL AUTO_INCREMENT,
product VARCHAR(45) NULL,
price INT NOT NULL,
PRIMARY KEY (id_products));
```


### Команды манипуляции данными в таблице
_____
#### Select


#### Insert


#### Update


#### Delete

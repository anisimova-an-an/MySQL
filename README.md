# MySQL

В данном документе представлено описание работы с базой данных с помощью MySQL Workbench

## Оглавление
1. [Описание проекта](#описание-проекта)
2. [Создание и удаление таблиц](#создание-и-удаление-таблиц)
   - [Создание таблицы с помощью команды ***Create***](#создание-таблицы-с-помощью-команды-create)
   - [Создание таблиц с помощью ***ERR диаграммы***](#создание-таблиц-с-помощью-err-диаграммы)
   - [Удаление таблицы с помощью команды ***Drop***](#удаление-таблицы-с-помощью-команды-drop)
4. [Команды манипуляции данными в таблице](#команды-манипуляции-данными-в-таблице)
   - [Select](#select)
   - [Insert](#insert)
   - [Update](#update)
   - [Delete](#delete)
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
#### Создание таблиц с помощью ***ERR диаграммы***
В примере создана ERR диаграмма для нескольких связанным таблиц, в которой отражены все столбцы с типами данных, учитываются связи "Один ко многим" и "Многие ко многим".

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/ERR%20диаграмма.png "диаграмма")

Далее производится экспорт SQL скрипта, который в дальнейшем импортируется для автоматического создания таблиц в программе [Ссылка на SQL скрипт](https://github.com/anisimova-an-an/MySQL/blob/main/скрипт.sql)

#### Удаление таблицы с помощью команды ***Drop***

```sql
DROP TABLE shop.product;
```

### Команды манипуляции данными в таблице
_____
#### Select

***Показать все столбцы в таблице "product"***

```sql
use ml;
select * from product;
```
Результат:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/table%20product.jpg "product")

***Показать все товары в таблице и столбец назвать "Товар"***

```sql
select product as Товар from product;
```

Результат:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-06-26_11-49-31.png "товар")

***Показать продукт со стоимостью, где цена в промежутке между 1000 и 10000, отсортировать по возрастающей по цене***

```sql
select product, price from ml.product
where price between 1000 and 10000
order by price asc;
```

Результат:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-06-26_11-49-44.png "товар-с-ценой")

***Показать все стобцы у товаров, которые относятся к категории "Авиация" и подкатегории "Боевая", отсортировав по производителю в обратном алфавитном порядке***

```sql
select * from product
where category="авиация" and subcategory="боевая"
order by seller desc;
```

Результат:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-06-26_11-49-59.png "товар-с-категорией")

***Показать товары, кроме тех, чье id равно 5,7 и 9, отсортировав по цене***

```sql
select * from product
where product_id not in (5,7,9)
order by price;
```

Результат:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-06-26_11-50-07.png "товар")

***Показать все товары с id меньше 6 (включительно) или больше 8***

```sql
select *from product 
where product_id <=6 or product_id >8;
```

Результат:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-06-26_11-50-17.png "товар")

#### Insert


#### Update


#### Delete

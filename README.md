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
     - Операторы WHERE и ORDER BY с логическими операторами
     - Операторы GROUP BY и HAVING  с агрегатными функциями
     - Вложенные запросы
     - Связанные таблицы (JOIN)
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
_____
#### Операторы WHERE и ORDER BY с логическими операторами

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

#### Операторы GROUP BY и HAVING  с агрегатными функциями

***Показать количество наименований в каждой категории товаров***

```sql
SELECT category AS 'Категория товаров',
COUNT(product) AS 'Количество наименований'
FROM ml.product
GROUP BY category;
```

Результат:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-06-28_12-29-22.png "группировка")

***Показать товар с количеством, ценой и общей суммой более 50000***

```sql
SELECT product AS Товар,
quantity AS Количество,
price AS Стоимость,
SUM(quantity * price) AS Сумма
FROM ml.product
GROUP BY product
HAVING Сумма > 50000;
```

Результат:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-06-28_12-29-54.png "группировка")

***Показать категории товаров, а также максимальную, минимальную стоимость и среднюю цену, округленную до сотых***

```sql
SELECT category AS 'Категория товаров',
MAX(price) AS 'Максимальная цена',
MIN(price) AS 'Минимальная цена',
ROUND(AVG(price),2) AS 'Средняя цена'
FROM ml.product
GROUP BY category;
```

Результат:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-06-28_12-30-18.png "группировка")

#### Вложенные операторы

****Показать товары с максимальной ценой***

```sql
select* from ml.product
where price=(select max(price) from ml.product);
```

Результат:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-07-01_16-05-40.png "вложенный")

***Показать товары с максимальным и минимальным количеством***

```sql
SELECT * FROM ml.product
WHERE quantity = (SELECT MIN(quantity)FROM ml.product) or quantity = (SELECT MAX(quantity) FROM ml.product);
```

Результат:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-07-01_16-06-00.png "вложенный")

***Показать товары, которые принадлежат к категориям с 2 и более позициями товаров внутри***

```sql
select * from ml.product
where category in (select category from ml.product group by category having count(category)>=2);
```

Результат:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-07-01_16-06-28.png "вложенный")

***Показать товары, у которых общая сумма больше среднего значения***

```sql
select * from ml.product
where (quantity*price) > (select avg(quantity*price) from ml.product);
```

Результат:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-07-01_16-08-28.png "вложенный")

***Показать только те товары, у которых стоимость меньше самой большой средней стоимости товара каждой категории***

```sql
select * from ml.product
where cost < any(select avg(cost) from ml.product group by category);
```

Результат:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-07-01_21-34-19.png "вложенный")

#### Связанные таблицы

***Какие товары из Беларусии продаются и на какую сумму***

```sql
select products.product, 
sum(`order`.quantity*`order`.price) as Объем_продаж
from ml.products
inner join manufacturer
on products.manufacturer_id = manufacturer.manufacturer_id
inner join `order`
on products.product_id = `order`.product_id
where address_manufacturer = 'Беларусь'
group by product;
```

Результат:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/ph_t6PI2C3U.jpg "join")

***Показать наименование товара с артикулом,а также наименование производителя с телефоном и почтой
тех товаров, чей остаток на складе меньше 10. Убрать null из пустых ячеек.***

```sql
select products.product, products.article, name_manufacturer,
COALESCE(telephone_manufacturer,'') as telephone,
COALESCE(email_manufacturer,'') as email
from ml.products
left join manufacturer
on products.manufacturer_id = manufacturer.manufacturer_id
where quantity < 10;
```

Результат:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/gbofb3Yv2dQ111.jpg "join")

***Показать имя покупателя, почту и сумму всех его заказов.***

```sql
select name_buyer, email_buyer, 
sum(quantity*price) as 'Объем продаж'
from buyer
right join `order`
on buyer.buyer_id = `order`.buyer_id
group by name_buyer;
```

Результат:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/UbjVzl9q_BQ.jpg "join")

#### Insert


#### Update


#### Delete

# MySQL

<br>

В данном документе представлены различные возможности работы с базой данных с помощью MySQL Workbench.

<br>

## Оглавление
1. [Цель проекта](#1-цель-проекта)
2. [Создание и удаление таблиц](#2-создание-и-удаление-таблиц)
   
   2.1. [Создание таблицы с помощью команды CREATE](#21-создание-таблицы-с-помощью-команды-create)
   
   2.2. [Создание таблиц с помощью ERR диаграммы](#22-создание-таблиц-с-помощью-err-диаграммы)
   
   2.3. [Удаление таблицы с помощью команды DROP](#23-удаление-таблицы-с-помощью-команды-drop)
   
3. [Команды манипуляции данными в таблице](#3-команды-манипуляции-данными-в-таблице)
   
    3.1. [Select:](#31-select---извлекает-записи-из-одной-или-нескольких-таблиц)
   
     - 3.1.1. [Операторы WHERE и ORDER BY с логическими операторами](#311-операторы-where-и-order-by-с-логическими-операторами)
   
     - 3.1.2. [Операторы GROUP BY и HAVING  с агрегатными функциями](#312-операторы-group-by-и-having--с-агрегатными-функциями)
   
     - 3.1.3. [Вложенные запросы](#313-вложенные-запросы)
   
     - 3.1.4. [Связанные таблицы (JOIN)](#314-связанные-таблицы)
   
    3.2. [Insert:](#32-insert---создает-записи)
     - 3.2.1. [Добавление данных в таблицу](#321-добавление-данных-в-таблицу)
     - 3.2.2. [Добавление данных через связанные таблицы](#322-добавление-данных-через-связанные-таблицы)
       
    3.3. [Update:](#33-update---модифицирует-записи)
     - 3.3.1. [Обновление данных в таблице](#331-обновление-данных-в-таблице)
     - 3.3.2. [Обновление данных через связанные таблицы](#332-обновление-данных-через-связанные-таблицы)
   
    3.4. [Delete](#34-delete--удаляет-записи)
   
<br>

## 1. Цель проекта

<br>

MySql Workbench — это программное обеспечение для создания и проектирования баз данных с помощью схем и других визуальных средств. Для проекта данный продукт и его компоненты были установлены. 
Цель проекта - изучение и практическая реализация различных запросов к базе данных.

<br>

## 2. Создание и удаление таблиц

### 2.1 Создание таблицы с помощью команды CREATE

```sql
CREATE TABLE shop.product (
id_products INT NOT NULL AUTO_INCREMENT,
product VARCHAR(45) NULL,
price INT NOT NULL,
PRIMARY KEY (id_products));
```

<br>

### 2.2 Создание таблиц с помощью ERR диаграммы
В примере создана ERR диаграмма для нескольких связанным таблиц, в которой отражены все столбцы с типами данных, учитываются связи "Один ко многим" и "Многие ко многим".

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/ERR%20диаграмма.png "диаграмма")

Далее производится экспорт SQL скрипта, который в дальнейшем импортируется для автоматического создания таблиц в программе [Ссылка на SQL скрипт](https://github.com/anisimova-an-an/MySQL/blob/main/скрипт.sql)

<br>

### 2.3 Удаление таблицы с помощью команды DROP

```sql
DROP TABLE shop.product;
```

<br>

## 3. Команды манипуляции данными в таблице

<br>

### 3.1 Select - Извлекает записи из одной или нескольких таблиц

#### 3.1.1 Операторы WHERE и ORDER BY с логическими операторами
_____

- *Показать все столбцы в таблице "product"*

```sql
use ml;
select * from product;
```
*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/table%20product.jpg "product")
_____
- *Показать все товары в таблице и столбец назвать "Товар"*

```sql
select product as Товар from product;
```

Результат:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-06-26_11-49-31.png "товар")
_____
- *Показать продукт со стоимостью, где цена в промежутке между 1000 и 10000, отсортировать по возрастающей по цене*

```sql
select product, price from ml.product
where price between 1000 and 10000
order by price asc;
```

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-06-26_11-49-44.png "товар-с-ценой")
_____
- *Показать все стобцы у товаров, которые относятся к категории "Авиация" и подкатегории "Боевая", отсортировав по производителю в обратном алфавитном порядке*

```sql
select * from product
where category="авиация" and subcategory="боевая"
order by seller desc;
```

*Результат*:

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-06-26_11-49-59.png "товар-с-категорией")
_____
- *Показать товары, кроме тех, чье id равно 5,7 и 9, отсортировав по цене*

```sql
select * from product
where product_id not in (5,7,9)
order by price;
```

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-06-26_11-50-07.png "товар")
______
- *Показать все товары с id меньше 6 (включительно) или больше 8*

```sql
select *from product 
where product_id <=6 or product_id >8;
```

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-06-26_11-50-17.png "товар")
_____

<br>

#### 3.1.2 Операторы GROUP BY и HAVING  с агрегатными функциями
_____

- *Показать количество наименований в каждой категории товаров*

```sql
SELECT category AS 'Категория товаров',
COUNT(product) AS 'Количество наименований'
FROM ml.product
GROUP BY category;
```

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-06-28_12-29-22.png "группировка")
_____
- *Показать товар с количеством, ценой и общей суммой более 50000*

```sql
SELECT product AS Товар,
quantity AS Количество,
price AS Стоимость,
SUM(quantity * price) AS Сумма
FROM ml.product
GROUP BY product
HAVING Сумма > 50000;
```

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-06-28_12-29-54.png "группировка")
_____
- *Показать категории товаров, а также максимальную, минимальную стоимость и среднюю цену, округленную до сотых*

```sql
SELECT category AS 'Категория товаров',
MAX(price) AS 'Максимальная цена',
MIN(price) AS 'Минимальная цена',
ROUND(AVG(price),2) AS 'Средняя цена'
FROM ml.product
GROUP BY category;
```

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-06-28_12-30-18.png "группировка")
_____

<br>

#### 3.1.3 Вложенные запросы
_____

- *Показать товары с максимальной ценой*

```sql
select* from ml.product
where price=(select max(price) from ml.product);
```

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-07-01_16-05-40.png "вложенный")
_____
- *Показать товары с максимальным и минимальным количеством*

```sql
SELECT * FROM ml.product
WHERE quantity = (SELECT MIN(quantity)FROM ml.product) or quantity = (SELECT MAX(quantity) FROM ml.product);
```

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-07-01_16-06-00.png "вложенный")
_____
- *Показать товары, которые принадлежат к категориям с 2 и более позициями товаров внутри*

```sql
select * from ml.product
where category in (select category from ml.product group by category having count(category)>=2);
```

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-07-01_16-06-28.png "вложенный")
_____
- *Показать товары, у которых общая сумма больше среднего значения*

```sql
select * from ml.product
where (quantity*price) > (select avg(quantity*price) from ml.product);
```

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-07-01_16-08-28.png "вложенный")
_____
- *Показать только те товары, у которых стоимость меньше самой большой средней стоимости товара каждой категории*

```sql
select * from ml.product
where cost < any(select avg(cost) from ml.product group by category);
```

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-07-01_21-34-19.png "вложенный")
_____

<br>

#### 3.1.4 Связанные таблицы
_____

- *Какие товары из Беларусии продаются и на какую сумму?*

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

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/ph_t6PI2C3U.jpg "join")
_____
- *Показать наименование товара с артикулом,а также наименование производителя с телефоном и почтой
тех товаров, чей остаток на складе меньше 10. Убрать null из пустых ячеек*

```sql
select products.product, products.article, name_manufacturer,
COALESCE(telephone_manufacturer,'') as telephone,
COALESCE(email_manufacturer,'') as email
from ml.products
left join manufacturer
on products.manufacturer_id = manufacturer.manufacturer_id
where quantity < 10;
```

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/gbofb3Yv2dQ111.jpg "join")
_____
- *Показать имя покупателя, почту и сумму всех его заказов*

```sql
select name_buyer, email_buyer, 
sum(quantity*price) as 'Объем продаж'
from buyer
right join `order`
on buyer.buyer_id = `order`.buyer_id
group by name_buyer;
```

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/UbjVzl9q_BQ.jpg "join")
_____

<br>

### 3.2 Insert - Создает записи
#### 3.2.1 Добавление данных в таблицу 
_____
- *Добавить в таблицу Карандаш со стоимостью 3р*

```sql
INSERT INTO shop.products (product, price)
VALUES ("карандаш", 3);
```

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/YniDtZzWfa8.jpg "добавление")
_____

<br>

#### 3.2.2 Добавление данных через связанные таблицы
_____
- *Создать таблицу и добавить в нее пользователей, которые не совершали ни одной покупки*

```sql
create table ml.unactive_buyer (
unactive_buyer_id int primary key auto_increment,
unactive_buyer varchar(50));

insert into ml.unactive_buyer (unactive_buyer)
select name_buyer
from ml.buyer
left join ml.`order`
on buyer.buyer_id=`order`.buyer_id
where order_id is null;
```

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-07-05_11-43-29.png "join")
_____

<br>

### 3.3 Update - Модифицирует записи
#### 3.3.1 Обновление данных в таблице
_____
- *Изменить стоимость карандаша на 4р*

```sql
UPDATE shop.products
SET price=4
WHERE id_products=2;
```

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/4yeieHfjcIY.jpg "join")
_____

<br>

#### 3.3.2 Обновление данных через связанные таблицы
_____
- *Добавить названия категорий в названия товаров*

```sql
update ml.products
inner join ml.category
on products.category_id = category.category_id
set product = concat(category, "/ ", product);
```

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/2024-07-05_12-28-14.png "join")
_____

<br>

### 3.4 Delete -	Удаляет записи
_____
- *Удалить товар с ID=4*

```sql
DELETE FROM shop.products
WHERE id_products=4;
```

*Результат:*

![Alt-текст](https://github.com/anisimova-an-an/MySQL/blob/main/gdKkavoNmoE.jpg "удалить")
_____

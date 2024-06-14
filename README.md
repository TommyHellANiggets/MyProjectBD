# Разработка базы данных для интернет-магазина по продаже спортивного питания
## Типовые запросы
### Получение информации о пользователях с ролью "admin":
```sql
SELECT username, first_name, last_name
FROM Users
JOIN Role ON Users.role_id = Role.id
WHERE Role.name = 'admin';
```
### Получение списка всех заказов с информацией о пользователе и продукте:
```sql
SELECT Orders.id AS order_id, Orders.created, Users.username, Products.name AS product_name, Products.price
FROM Orders
JOIN Users ON Orders.user_id = Users.id
JOIN Products ON Orders.product_id = Products.id;
```
### Получение списка продуктов с активными скидками:
```sql
SELECT Products.name AS product_name, Products.price, Discounts.discount_percentage, Discounts.start_date, Discounts.end_date
FROM Products
JOIN Discounts ON Products.id = Discounts.product_id
WHERE Discounts.start_date <= CURDATE() AND Discounts.end_date >= CURDATE();
```
### Получение информации о доставке для конкретного заказа:
```sql
SELECT Orders.id AS order_id, Users.username, Shipping.delivery_method, Shipping.delivery_cost, Shipping.delivery_time
FROM Shipping
JOIN Orders ON Shipping.order_id = Orders.id
JOIN Users ON Orders.user_id = Users.id
WHERE Orders.id = 1;  
```
### Получение отзывов и рейтингов для определенного продукта:
```sql
SELECT Products.name AS product_name, Reviews.rating, Reviews.comment, Users.username
FROM Reviews
JOIN Products ON Reviews.product_id = Products.id
JOIN Users ON Reviews.customer_id = Users.id
WHERE Products.id = 1;
```
## Транзакция 
```sql
START TRANSACTION;

INSERT INTO Orders (created, user_id, product_id)
VALUES (NOW(), 1, 1); 

SET @order_id = LAST_INSERT_ID();

UPDATE Products
SET quantity = quantity - 1
WHERE id = 1;  

INSERT INTO Shipping (delivery_method, delivery_cost, delivery_time, order_id)
VALUES ('Курьерская доставка', 300.00, 2, @order_id);

COMMIT;
```
## Локальные переменные с заданным типом данных
```sql
SET @user_id = 1;  
SET @product_id = 2;  
SET @delivery_method = 'Курьерская доставка';

SELECT 
    Users.username, 
    Products.name AS product_name, 
    @delivery_method AS selected_delivery_method
FROM 
    Users
JOIN 
    Orders ON Users.id = Orders.user_id
JOIN 
    Products ON Orders.product_id = Products.id
WHERE 
    Users.id = @user_id AND Products.id = @product_id;
```
## Пример запроса с условием
```sql
SELECT Products.name, Products.price, Products.description
FROM Products
JOIN Category ON Products.category_id = Category.id
WHERE Category.name = 'Протеины' AND Products.price < 4000;
```
##
```sql

```
##
```sql

```
##
```sql

```
##
```sql

```

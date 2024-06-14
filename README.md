# Разработка базы данных для интернет-магазина по продаже спортивного питания
## Создание базы данных
```sql
CREATE TABLE Role(
    id INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    name ENUM('user', 'admin', 'superuser') NOT NULL
);

CREATE TABLE Users(
    id INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    username VARCHAR(55) NOT NULL,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    password_hash VARCHAR(255) NOT NULL,
    role_id INT NOT NULL,
    FOREIGN KEY (role_id) REFERENCES Role(id)
);

CREATE TABLE Category(
    id INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    name VARCHAR(70)
);

CREATE TABLE Products(
    id INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    name VARCHAR(100),
    quantity INT NOT NULL,
    price DECIMAL(10, 2),
    description TEXT,
    image VARCHAR(255),
    category_id INT,
    FOREIGN KEY (category_id) REFERENCES Category(id)
);

CREATE TABLE Orders(
    id INT AUTO_INCREMENT PRIMARY KEY NOT NULL,
    created TIMESTAMP NOT NULL,
    user_id INT,
    product_id INT,
    FOREIGN KEY (user_id) REFERENCES Users(id),
    FOREIGN KEY (product_id) REFERENCES Products(id)
);

CREATE TABLE Shipping(
    id INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    delivery_method VARCHAR(100),
    delivery_cost DECIMAL(10, 2),
    delivery_time INT NOT NULL,
    order_id INT NOT NULL,
    FOREIGN KEY(order_id) REFERENCES Orders(id)
);

CREATE TABLE BonusPoints (
    customer_id INT NOT NULL,
    points INT NOT NULL,
    expiration_date DATE NOT NULL,
    PRIMARY KEY (customer_id),
    FOREIGN KEY (customer_id) REFERENCES Users(id)
);

CREATE TABLE Reviews (
    review_id INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    product_id INT NOT NULL,
    customer_id INT NOT NULL,
    rating INT CHECK (rating >= 1 AND rating <= 5),
    comment TEXT,
    FOREIGN KEY (product_id) REFERENCES Products(id),
    FOREIGN KEY (customer_id) REFERENCES Users(id)
);

CREATE TABLE Discounts (
    discount_id INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
    product_id INT NOT NULL,
    discount_percentage DECIMAL(5, 2) CHECK (discount_percentage > 0 AND discount_percentage <= 100),
    start_date DATE NOT NULL,
    end_date DATE NOT NULL,
    FOREIGN KEY (product_id) REFERENCES Products(id)
);
```
## Добавление данных в таблицы
```sql
-- Вставка данных в таблицу Role
INSERT INTO Role (name) VALUES ('user'), ('admin'), ('superuser');

-- Вставка данных в таблицу Users
INSERT INTO Users (username, first_name, last_name, password_hash, role_id)
VALUES
    ('user1', 'Иван', 'Иванов', 'password_hash_1', 1),
    ('user2', 'Петр', 'Петров', 'password_hash_2', 1),
    ('admin1', 'Анна', 'Сидорова', 'password_hash_3', 2),
    ('admin2', 'Мария', 'Кузнецова', 'password_hash_4', 2),
    ('superuser1', 'Алексей', 'Петров', 'password_hash_5', 3);

-- Вставка данных в таблицу Category
INSERT INTO Category (name) VALUES ('Протеины'), ('Витамины и минералы'), ('Энергетики'), ('Аминокислоты'), ('Спортивные добавки');

-- Вставка данных в таблицу Products
INSERT INTO Products (name, quantity, price, description, image, category_id)
VALUES
    ('Протеин Optimum Nutrition Gold Standard 100% Whey', 20, 3500.00, 'Протеин с высоким содержанием белка и низким содержанием жиров и углеводов', 'whey_protein.jpg', 1),
    ('Витамины и минералы Opti-Men', 30, 2500.00, 'Мультитравмин для мужчин с оптимальным набором витаминов и минералов', 'vitamins.jpg', 2),
    ('Энергетический напиток Red Bull', 100, 120.00, 'Энергетический напиток с таурином и кофеином', 'redbull.jpg', 3),
    ('BCAA Scivation Xtend', 40, 1800.00, 'Комплекс аминокислот с улучшенной формулой для роста мышц и восстановления', 'bcaa.jpg', 4),
    ('Креатин Ultimate Nutrition Creatine Monohydrate', 50, 1500.00, 'Креатин моногидрат для увеличения силы и выносливости', 'creatine.jpg', 5);

-- Вставка данных в таблицу Orders
INSERT INTO Orders (created, user_id, product_id)
VALUES
    (NOW(), 1, 1),
    (NOW(), 2, 2),
    (NOW(), 3, 3),
    (NOW(), 4, 4),
    (NOW(), 5, 5);

-- Вставка данных в таблицу Shipping
INSERT INTO Shipping (delivery_method, delivery_cost, delivery_time, order_id)
VALUES
    ('Курьерская доставка', 300.00, 2, 1),
    ('Почта России', 150.00, 5, 2),
    ('Pickup', 0.00, 1, 3),
    ('Самовывоз', 0.00, 1, 4),
    ('Курьерская доставка', 250.00, 3, 5);

-- Вставка данных в таблицу BonusPoints
INSERT INTO BonusPoints (customer_id, points, expiration_date)
VALUES
    (1, 100, '2024-12-31'),
    (2, 50, '2024-10-31'),
    (3, 150, '2025-01-15'),
    (4, 75, '2024-11-30'),
    (5, 200, '2025-02-28');

-- Вставка данных в таблицу Reviews
INSERT INTO Reviews (product_id, customer_id, rating, comment)
VALUES
    (1, 1, 5, 'Отличный протеин! Пользуюсь уже месяц и результаты радуют.'),
    (2, 2, 4, 'Хорошие витамины, чувствую себя бодрее.'),
    (3, 3, 3, 'Red Bull как всегда вкусный, но может быть слишком сладковат.'),
    (4, 4, 5, 'BCAA Xtend отлично помогает восстанавливаться после тренировок.'),
    (5, 5, 4, 'Креатин Ultimate Nutrition действительно улучшает выносливость.');

-- Вставка данных в таблицу Discounts
INSERT INTO Discounts (product_id, discount_percentage, start_date, end_date)
VALUES
    (1, 10.00, '2024-06-01', '2024-06-30'),
    (2, 20.00, '2024-06-15', '2024-06-30'),
    (3, 15.00, '2024-06-01', '2024-06-30'),
    (4, 12.00, '2024-06-10', '2024-06-30'),
    (5, 18.00, '2024-06-05', '2024-06-30');
```
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
## Хранимая процедура для добавления нового продукта
```sql
DELIMITER //

CREATE PROCEDURE AddNewProduct(
    IN p_name VARCHAR(100),
    IN p_quantity INT,
    IN p_price DECIMAL(10, 2),
    IN p_description TEXT,
    IN p_image VARCHAR(255),
    IN p_category_id INT
)
BEGIN
    INSERT INTO Products (name, quantity, price, description, image, category_id)
    VALUES (p_name, p_quantity, p_price, p_description, p_image, p_category_id);
END //

DELIMITER ;
```
### Использование процедуры
```sql
CALL AddNewProduct(
    'Протеин Dymatize ISO100',
    15,
    4500.00,
    'Высококачественный изолят сывороточного белка с минимальным содержанием углеводов и жиров',
    'dymatize_iso100.jpg',
    1
);
```
## Представление для отображения заказов с информацией о пользователе и продукте
```sql
CREATE VIEW OrderDetails AS
SELECT 
    Orders.id AS order_id,
    Orders.created AS order_date,
    Users.username AS user_username,
    Users.first_name AS user_first_name,
    Users.last_name AS user_last_name,
    Products.name AS product_name,
    Products.price AS product_price,
    Products.description AS product_description
FROM 
    Orders
JOIN 
    Users ON Orders.user_id = Users.id
JOIN 
    Products ON Orders.product_id = Products.id;
```
## Использование представления
```sql
SELECT * FROM OrderDetails;
```
## Пример триггера
```sql
DELIMITER //

CREATE TRIGGER UpdateBonusPointsAfterOrder
AFTER INSERT ON Orders
FOR EACH ROW
BEGIN
    DECLARE new_points INT;
    
    SELECT FLOOR(Products.price / 100) INTO new_points
    FROM Products
    WHERE Products.id = NEW.product_id;
    
    INSERT INTO BonusPoints (customer_id, points, expiration_date)
    VALUES (NEW.user_id, new_points, DATE_ADD(NOW(), INTERVAL 1 YEAR))
    ON DUPLICATE KEY UPDATE points = points + new_points;
END //

DELIMITER ;
```
### Использование триггера
```sql
INSERT INTO Orders (created, user_id, product_id)
VALUES (NOW(), 1, 1);
```
## Пользовательская функция для расчета скидки на продукт
```sql
DELIMITER //

CREATE FUNCTION GetProductDiscount(product_id INT) 
RETURNS DECIMAL(10, 2)
DETERMINISTIC
BEGIN
    DECLARE discount DECIMAL(10, 2);
    DECLARE product_price DECIMAL(10, 2);
    
    SELECT price INTO product_price
    FROM Products
    WHERE id = product_id;
    
    SELECT COALESCE(MAX(discount_percentage), 0) INTO discount
    FROM Discounts
    WHERE product_id = product_id
      AND start_date <= CURDATE()
      AND end_date >= CURDATE();
    
    RETURN product_price * (discount / 100);
END //

DELIMITER ;
```
### Использование функции
```sql
SELECT GetProductDiscount(1) AS discount_amount;
```
## Хранимая процедура с обработчиком исключений
```sql
DELIMITER //

CREATE PROCEDURE AddOrder(
    IN p_user_id INT,
    IN p_product_id INT,
    OUT p_order_id INT,
    OUT p_error_message VARCHAR(255)
)
BEGIN
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        SET p_error_message = 'Error occurred while adding the order. Please check user ID and product ID.';
        ROLLBACK;
    END;

    DECLARE EXIT HANDLER FOR NOT FOUND
    BEGIN
        SET p_error_message = 'User ID or Product ID not found.';
        ROLLBACK;
    END;

    START TRANSACTION;

    IF NOT EXISTS (SELECT id FROM Users WHERE id = p_user_id) THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'User ID not found';
    END IF;

    IF NOT EXISTS (SELECT id FROM Products WHERE id = p_product_id) THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Product ID not found';
    END IF;

    INSERT INTO Orders (created, user_id, product_id) VALUES (NOW(), p_user_id, p_product_id);
    SET p_order_id = LAST_INSERT_ID();

    COMMIT;
    SET p_error_message = NULL;
END //

DELIMITER ;
```
### Использование процедуры
```sql
CALL AddOrder(1, 1, @order_id, @error_message);
SELECT @order_id, @error_message;
```
## Хранимая процедура с обработчиком исключений
```sql
DELIMITER //

CREATE PROCEDURE ДобавитьЗаказ(
    IN p_user_id INT,
    IN p_product_id INT,
    OUT p_order_id INT,
    OUT p_error_message VARCHAR(255)
)
BEGIN
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        SET p_error_message = 'Произошла ошибка при добавлении заказа. Пожалуйста, проверьте идентификатор пользователя и продукта.';
        ROLLBACK;
    END;

    DECLARE EXIT HANDLER FOR NOT FOUND
    BEGIN
        SET p_error_message = 'Идентификатор пользователя или продукта не найден.';
        ROLLBACK;
    END;

    START TRANSACTION;

    IF NOT EXISTS (SELECT id FROM Users WHERE id = p_user_id) THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Идентификатор пользователя не найден';
    END IF;

    IF NOT EXISTS (SELECT id FROM Products WHERE id = p_product_id) THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Идентификатор продукта не найден';
    END IF;

    INSERT INTO Orders (created, user_id, product_id) VALUES (NOW(), p_user_id, p_product_id);
    SET p_order_id = LAST_INSERT_ID();

    COMMIT;
    SET p_error_message = NULL;
END //

DELIMITER ;
```
## Использование процедуры
```sql
CALL ДобавитьЗаказ(1, 1, @order_id, @error_message);
SELECT @order_id, @error_message;
```

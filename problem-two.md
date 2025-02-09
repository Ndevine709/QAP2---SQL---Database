### Creating the Tables
##### Products
```
CREATE TABLE products (
	id SERIAL PRIMARY KEY,
	product_name TEXT,
	price DECIMAL(10,2),
	stock_quantity INTEGER
);
```
##### Customers
```
CREATE TABLE customers (
	id SERIAL PRIMARY KEY,
	first_name TEXT,
	last_name TEXT,
	email TEXT
);
```
##### Orders
```
CREATE TABLE orders (
	id SERIAL PRIMARY KEY,
	customer_id INTEGER,
	order_date DATE,
	FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```
##### Order Items
```
CREATE TABLE order_items (
	order_id INTEGER,
	product_id INTEGER,
	quantity INTEGER,
	PRIMARY KEY (order_id, product_id),
	FOREIGN KEY (order_id) REFERENCES orders(id),
	FOREIGN KEY (product_id) REFERENCES products(id)
);
```
### Inserting the data
##### Products
```
INSERT INTO products (product_name, price, stock_quantity)
VALUES
('Laptop', 999.99, 20),
('Desktop', 599.99, 40),
('IPhone', 799.99, 50),
('Tablet', 499.99, 20),
('Camera', 299.99, 10);
```
##### Customers
```
INSERT INTO customers (first_name, last_name, email)
VALUES
('John', 'Doe', 'doe@gmail.com'),
('Jack', 'Smith', 'smith@gmail.com'),
('Jimmy', 'Ryan', 'ryan@gmail.com'),
('Joe', 'Maher', 'maher@gmail.com');
```
##### Orders
```
INSERT INTO orders (customer_id, order_date)
VALUES
((SELECT id FROM customers WHERE first_name = 'John' AND last_name = 'Doe'), '2023-01-02'),
((SELECT id FROM customers WHERE first_name = 'Jack' AND last_name = 'Smith'), '2023-01-03'),
((SELECT id FROM customers WHERE first_name = 'Jimmy' AND last_name = 'Ryan'), '2023-01-05'),
((SELECT id FROM customers WHERE first_name = 'Joe' AND last_name = 'Maher'), '2023-01-06'),
((SELECT id FROM customers WHERE first_name = 'John' AND last_name = 'Doe'), '2023-01-07');
```
##### Order Items
```
INSERT INTO order_items (order_id, product_id, quantity)
VALUES
(1, 1, 1),
(1, 2, 2),
(2, 2, 1),
(2, 3, 2),
(3, 5, 1),
(3, 1, 2),
(4, 2, 1),
(4, 4, 2),
(5, 4, 1),
(5, 2, 2);
```
### Write SQL Queries
##### Query 1
```
SELECT products.product_name, stock_quantity
FROM products
```
##### Query 2
```
SELECT products.product_name, order_items.quantity
FROM order_items
JOIN products ON order_items.product_id = products.id
WHERE order_items.order_id = 1;
```
##### Query 3
```
SELECT orders.id, order_items.product_id, order_items.quantity
FROM orders
JOIN order_items ON orders.id = order_items.order_id
WHERE orders.customer_id = 1;
```
##### Update Data
- Order 1 has 1 laptop and 2 desktops. Below code will remove 1 laptop and 2 desktops from the stock
```
UPDATE products
SET stock_quantity = stock_quantity - 1
WHERE id = 1;
```
```
UPDATE products
SET stock_quantity = stock_quantity - 2
WHERE id = 2;
```
##### Delete Data
```
DELETE FROM order_items
WHERE order_id = 1;
```
```
DELETE FROM orders
WHERE id = 1;
```
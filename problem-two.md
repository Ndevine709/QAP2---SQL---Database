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
## About The Example
These examples are about the relationship between tables in [postgresql](https://www.postgresql.org/)
   1. One TO One
   2. One TO Many
   3. Many TO Many


### 1.One To One


>     CREATE TABLE people (
>         id SERIAL PRIMARY KEY,
>         first_name VARCHAR(32) NOT NULL,
>         last_name VARCHAR(32) NOT NULL
>     );
> </pre>


>     CREATE TABLE phone_numbers (
>         id SERIAL PRIMARY KEY,
>         person_id INTEGER UNIQUE NOT NULL REFERENCES people ON DELETE CASCADE ON UPDATE CASCADE,
>         phone_number CHAR(9) NOT NULL
>     )
> </pre>


>     INSERT INTO people (first_name, last_name)
>     VALUES
>        ('Mahdi', 'Asadzadeh'),
>        ('Ali', 'Azadi')
> 
> <pre>


>     INSERT INTO phone_numbers (person_id, phone_number)
>     VALUES
>        (1, '123456789'),
>        (2, '123456789')
> </pre>


>     SELECT 
>        people.id "ID",
>        people.first_name "First Name",
>        people.last_name "Last Name",
>        phone_numbers.phone_number "Phone Number"
>     FROM 
>        people,
>        phone_numbers
>     
>     WHERE
>        people.id = phone_numbers.person_id
> </pre>


#### Output
> <pre>
> ID | First Name | Last Name | Phone Number
> -: | :--------- | :-------- | :-----------
>  1 | Mahdi      | Asadzadeh | 123456789   
>  2 | Ali        | Azadi     | 123456789   
> </pre>


*db<>fiddle [here](https://dbfiddle.uk/?rdbms=postgres_12&fiddle=095182b012a6f0d74e5b141bdfb18346)*


----------------------------------


## 2.One To Many


>     CREATE TABLE people (
>         id SERIAL PRIMARY KEY,
>         first_name VARCHAR(32) NOT NULL,
>         last_name VARCHAR(32) NOT NULL
>     );
> </pre>


>     CREATE TABLE post (
>         id SERIAL PRIMARY KEY,
>         person_id INTEGER  NOT NULL REFERENCES people ON DELETE CASCADE ON UPDATE CASCADE,
>         body VARCHAR(32) NOT NULL
>     );
> </pre>


>     INSERT INTO people (first_name, last_name)
>     VALUES
>        ('Mahdi', 'Asadzadeh'),
>        ('Ali', 'Azadi')
> </pre>


>     INSERT INTO post (person_id, body)
>     VALUES
>        (1, 'post 1 - 1'),
>        (1, 'post 1 - 2'),
>        (2, 'post 2 - 1')
> </pre>


>     SELECT 
>        people.id "ID",
>        people.first_name "First Name",
>        people.last_name "Last Name",
>        post.body "Body"
>     FROM 
>        people,
>        post
>     
>     WHERE
>        people.id = post.person_id
       
#### Output
> <pre>
> ID | First Name | Last Name | Body      
> -: | :--------- | :-------- | :---------
>  1 | Mahdi      | Asadzadeh | post 1 - 1
>  1 | Mahdi      | Asadzadeh | post 1 - 2
>  2 | Ali        | Azadi     | post 2 - 1
> </pre>

*db<>fiddle [here](https://dbfiddle.uk/?rdbms=postgres_12&fiddle=cf70ea651278fafd9d38784d17714657)*

----------------------------------------------------------


## 3.Many To Many
>     CREATE TABLE product (
>        id SERIAL PRIMARY KEY,
>        name VARCHAR(32) NOT NULL
>     );


>     CREATE TABLE users (
>        id SERIAL PRIMARY KEY,
>        name VARCHAR(32) NOT NULL
>     );


>     CREATE TABLE order_user_product (
>        id SERIAL PRIMARY KEY,
>        user_id INTEGER REFERENCES users,
>        product_id INTEGER REFERENCES product,
>        create_time TIMESTAMP NOT NULL DEFAULT NOW()
>     );


>     INSERT INTO product (name)
>     VALUES
>       ('product 1'),
>       ('product 2');
>     
>     
>     INSERT INTO users (name) 
>     VALUES 
>        ('mahdi'),
>        ('ali'),
>        ('hasan');
>     
>     
>     INSERT INTO order_user_product (user_id, product_id)
>     VALUES
>        (1, 1),
>        (1, 2),
>        (2, 1),
>        (2, 2);


>     SELECT 
>        users.name "Name",
>        product.name "Product Name"
>     FROM 
>        users
>     
>     JOIN
>        order_user_product
>        ON users.id = order_user_product.user_id
>        
>        
>     JOIN 
>        product
>        ON order_user_product.product_id = product.id
>        

#### Output
> <pre>
> Name  | Product Name
> :---- | :-----------
> mahdi | product 1   
> mahdi | product 2   
> ali   | product 1   
> ali   | product 2   
> </pre>

*db<>fiddle [here](https://dbfiddle.uk/?rdbms=postgres_13&fiddle=8f393da3c1a68bf936da9f12ba62520a)*

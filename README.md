# GARDENING QUERIES

## 1.4.5 Multi-table queries (Internal composition)

Resolve all queries using SQL1 and SQL2 syntax. Queries with SQL2 syntax must be resolved with JOIN.

1. Obtain a list with the name of each client and the first and last name of their sales representative.

```sql
SELECT nombre_cliente, CONCAT(nombre, ' ', apellido1,  ' ',apellido2) AS nombre  
FROM cliente 
JOIN empleado ON cliente.codigo_empleado_rep_ventas = empleado.codigo_empleado;
```

2. Shows the name of customers who have made payments along with the name of their sales representatives.

```sql
SELECT DISTINCT c.nombre_cliente as Cliente_que_pagó, CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) AS Nombre_representante_de_ventas  
FROM pago p 
JOIN cliente c ON p.codigo_cliente = c.codigo_cliente 
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado;
```

3. Shows the name of customers who have not made payments along with the name of their sales representatives.

```sql
SELECT c.nombre_cliente AS Cliente_que_no_pago, CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) AS Nombre_representante_de_ventas 
FROM cliente c 
LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente 
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado 
WHERE p.codigo_cliente IS NULL;
```

4. Returns the name of customers who have made payments and the name of their representatives along with the city of the office to which the representative belongs.

```sql
SELECT DISTINCT c.nombre_cliente AS Cliente_que_pagó, CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) AS Nombre_representante_de_ventas, o.ciudad as Oficina 
FROM pago p 
JOIN cliente c ON p.codigo_cliente = c.codigo_cliente 
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado 
JOIN oficina o ON e.codigo_oficina = o.codigo_oficina;
```

5. Returns the name of customers who have not made payments and the name of their representatives along with the city of the office to which the representative belongs.

```sql
SELECT c.nombre_cliente AS Cliente_que_no_pago, CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) AS Nombre_representante_de_ventas, o.ciudad as Oficina 
FROM cliente c 
LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente 
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado 
JOIN oficina o ON e.codigo_oficina = o.codigo_oficina 
WHERE p.codigo_cliente IS NULL;
```

6. List the address of the offices that have clients in Fuenlabrada.

```sql
SELECT CONCAT(o.linea_direccion1, ' ', o.linea_direccion2) AS Direccion_oficina 
FROM oficina o 
JOIN empleado e ON o.codigo_oficina = e.codigo_oficina 
JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas 
WHERE c.ciudad = 'Fuenlabrada';
```

7. Returns the name of the clients and the name of their representatives along with the city of the office to which the representative belongs.

```sql
SELECT c.nombre_cliente, CONCAT(e.nombre, ' ', e.apellido1, ' ',e.apellido2) AS Nombre_de_representante, o.ciudad AS Ciudad_representante 
FROM cliente c 
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado 
JOIN oficina o ON e.codigo_oficina = o.codigo_oficina;
```

8. Returns a list with the names of the employees along with the names of their bosses.

```sql
SELECT e.nombre AS Nombre_empleado, j.nombre AS Nombre_jefe 
FROM empleado e 
LEFT JOIN empleado j ON e.codigo_jefe = j.codigo_empleado;
```

9. Returns a list showing the name of each employee, the name of their boss, and the name of their boss's boss.

```sql
SELECT e.nombre AS Nombre_empleado, j.nombre AS Nombre_jefe, jj.nombre AS Nombre_jefe_del_jefe  
FROM empleado e 
LEFT JOIN empleado j ON e.codigo_jefe = j.codigo_empleado 
LEFT JOIN empleado jj ON jj.codigo_empleado = j.codigo_jefe;
```

10. Returns the name of customers to whom an order has not been delivered on time.

```sql
SELECT p.fecha_esperada,p.fecha_entrega, c.nombre_cliente 
FROM cliente c 
JOIN pedido p ON c.codigo_cliente = p.codigo_cliente 
WHERE p.fecha_entrega > p.fecha_esperada OR p.fecha_entrega IS NULL;
```

11. Returns a list of the different product ranges that each customer has purchased.

```sql
SELECT DISTINCT c.nombre_cliente, g.gama 
FROM cliente c 
JOIN pedido p ON c.codigo_cliente = p.codigo_cliente 
JOIN detalle_pedido d ON p.codigo_pedido = d.codigo_pedido 
JOIN producto pr ON d.codigo_producto = pr.codigo_producto 
JOIN gama_producto g ON pr.gama = g.gama;
```


## 1.4.5 Multi-table queries (External Composition)

Resolve all queries using the LEFT JOIN, RIGHT JOIN, NATURAL LEFT JOIN, and NATURAL RIGHT JOIN clauses.

1. Returns a list that shows only the customers who have not made any payments.

```sql
SELECT DISTINCT c.nombre_cliente 
FROM cliente c 
LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente 
WHERE p.codigo_cliente IS NULL;
```

2. Returns a list showing only customers who have not placed any orders.

```sql
SELECT DISTINCT c.nombre_cliente 
FROM cliente c 
LEFT JOIN pedido p ON c.codigo_cliente = p.codigo_cliente 
WHERE p.codigo_cliente IS NULL;
```

3. Returns a list showing the customers who have not made any payments and those who have not placed any orders.

```sql
SELECT DISTINCT c.nombre_cliente 
FROM cliente c 
LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente 
LEFT JOIN pedido pe ON c.codigo_cliente = pe.codigo_cliente 
WHERE p.codigo_cliente is null AND pe.codigo_cliente IS NULL;
```

4. Returns a list showing only employees who do not have an associated office.

```sql
SELECT o.codigo_oficina, e.nombre 
FROM empleado e 
RIGHT JOIN oficina o ON e.codigo_oficina = o.codigo_oficina 
WHERE e.codigo_oficina IS NULL;
```

5. Returns a list that shows only employees who do not have a client associated with them.

```sql
SELECT DISTINCT nombre 
FROM empleado 
LEFT JOIN cliente ON empleado.codigo_empleado = cliente.codigo_empleado_rep_ventas 
WHERE cliente.codigo_empleado_rep_ventas IS NULL;
```

6. Returns a list that shows only the employees who do not have an associated client along with the data of the office where they work.

```sql
SELECT DISTINCT e.nombre, o.* 
FROM empleado e 
LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas 
LEFT JOIN oficina o ON e.codigo_oficina = o.codigo_oficina 
WHERE c.codigo_empleado_rep_ventas IS NULL;
```

7. Returns a list showing employees who do not have an associated office and those who do not have an associated client.

```sql
SELECT o.codigo_oficina, e.nombre 
FROM empleado e  
RIGHT JOIN oficina o ON e.codigo_oficina = o.codigo_oficina 
LEFT JOIN cliente ON e.codigo_empleado = cliente.codigo_empleado_rep_ventas  
WHERE e.codigo_oficina IS NULL OR cliente.codigo_empleado_rep_ventas IS NULL;
```

8. Returns a list of products that have never appeared in an order.

```sql
SELECT pr.nombre 
FROM producto pr 
LEFT JOIN detalle_pedido d ON pr.codigo_producto = d.codigo_producto  
WHERE d.codigo_producto IS NULL;
```

9. Returns a list of products that have never appeared in an order. The result should show the name, description and image of the product.

```sql
SELECT pr.nombre, pr.descripcion, g.imagen 
FROM producto pr 
LEFT JOIN detalle_pedido d ON pr.codigo_producto = d.codigo_producto 
LEFT JOIN gama_producto g ON pr.gama = g.gama  
WHERE d.codigo_producto IS NULL;
```

10. Returns the offices where none of the employees who have been the sales representatives of a client who has made the purchase of a product from the 'Frutales' range work.

```sql
SELECT DISTINCT o.codigo_oficina
FROM oficina o
LEFT JOIN empleado e ON o.codigo_oficina = e.codigo_oficina
LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas
LEFT JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
LEFT JOIN detalle_pedido d ON p.codigo_pedido = d.codigo_pedido
LEFT JOIN producto pr ON d.codigo_producto = pr.codigo_producto
WHERE pr.gama = 'Frutales' AND c.codigo_empleado_rep_ventas IS NOT NULL AND e.codigo_empleado IS NOT NULL AND c.codigo_cliente IS NOT NULL AND p.codigo_pedido IS NOT NULL AND d.codigo_pedido IS NOT NULL AND pr.codigo_producto IS NOT NULL AND o.codigo_oficina IS NOT NULL; 
```

11. Returns a list of customers who have placed an order but have not made any payment.

```sql
SELECT DISTINCT c.nombre_cliente
FROM cliente c
INNER JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
LEFT JOIN pago pa ON c.codigo_cliente = pa.codigo_cliente
WHERE pa.codigo_cliente IS NULL;
```

12. Returns a list with the data of employees who do not have associated clients and the name of their associated boss.

```sql
SELECT DISTINCT e.*, j.nombre AS Nombre_jefe 
FROM empleado e 
LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas 
LEFT JOIN empleado j ON e.codigo_jefe = j.codigo_empleado 
WHERE c.codigo_empleado_rep_ventas IS NULL;
```

## 1.4.5 Additional multi-table queries

1. Returns a list with office id and the city where there are offices

```sql
SELECT codigo_oficina, ciudad 
FROM oficina;
```

2. Returns a list with the city and phone number of the offices in Spain.

```sql
SELECT telefono , ciudad 
FROM oficina
WHERE pais='España';
```

3. Returns a list with the first name, last name and email of the employees whose boss has a boss id equal to 7.

```sql
SELECT DISTINCT CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) AS Nombre_empleado, e.email
FROM empleado e 
JOIN empleado j ON e.codigo_jefe = j.codigo_jefe 
WHERE j.codigo_jefe = 7;
```

4. Returns the position, name, last name and email of the boss of the company.

```sql
SELECT puesto, CONCAT(nombre,' ',apellido1,' ',apellido2) AS Nombre_jefe, email 
FROM empleado 
WHERE codigo_jefe IS NULL;
```

5. Returns a list with the name, last name and position of those employees who are not sales representatives.

```sql
SELECT CONCAT(nombre, ' ', apellido1, ' ', apellido2) AS Nombre_empleado, puesto 
FROM empleado 
WHERE puesto != 'Representante Ventas';
```

6. Returns a list with the name of all Spanish clients. 

```sql
SELECT nombre_cliente 
FROM cliente 
WHERE cliente.pais = 'Spain';
```

7. Returns a list with the different states through which an order can go.

```sql
SELECT DISTINCT estado 
FROM pedido;
```

8. Returns a list with the customer id of those customers who made a payment in 2008. Keep in mind that you must eliminate those customer codes that appear repeated. Solve the query:
• Using the YEAR function of MySOL.
• Using the MysQL DATE_FORMAT function.
• Without using any of the previous functions.

```sql
SELECT codigo_cliente 
FROM pago 
WHERE YEAR(fecha_pago) = 2008;
```

9. Returns a list with the order code, customer code, expected date and delivery date of the orders that have not been delivered on time.

```sql
SELECT codigo_pedido , codigo_cliente, fecha_esperada, fecha_entrega 
FROM pedido 
WHERE fecha_esperada < fecha_entrega;
```

10. Returns a list with the order code, customer code, expected date and delivery date of orders whose delivery date was at least two days before the expected date.
• Using the ADDATE function of MySOL.
• Using the MysQL DATEDIFF function.
• Would it be possible to solve this query using the addition + or subtraction operator?

```sql
SELECT codigo_pedido , codigo_cliente, fecha_esperada, fecha_entrega 
FROM pedido 
WHERE fecha_entrega <= ADDDATE(fecha_esperada, -2) ;
```

11. Returns a list of all orders that were rejected in 2009.

```sql
SELECT * 
FROM pedido 
WHERE estado='rechazado' AND YEAR(fecha_pedido)=2009;
```

12. Returns a list of all orders that have been delivered in the month of January of any year.

```sql
SELECT * 
FROM pedido 
WHERE MONTH(fecha_entrega)=1;
```

13. Returns a list with all the payments that were made in 2008 through Paypal. Order the result from highest to lowest.

```sql
SELECT * 
FROM pago  
WHERE YEAR(fecha_pago) = 2008 AND forma_pago = 'Paypal' 
ORDER BY fecha_pago DESC;
```

14. Returns a list with all the payment methods that appear in the payment table. Please note that no duplicate payment methods should appear.

```sql
SELECT DISTINCT forma_pago 
FROM pago;
```

15. Returns a list with all the products that belong to the 'Ornamentales' range and that have more than 100 units in stock. The list must be ordered by their sales price, showing the highest prices first.

```sql
SELECT codigo_producto, nombre, precio_venta 
FROM producto 
WHERE gama = 'Ornamentales' AND cantidad_en_stock > 100 
ORDER BY precio_venta DESC;
```

16. Returns a list with all the clients who are from the city of Madrid and whose sales representative has the employee id 11 or 30.

```sql
SELECT nombre_cliente 
FROM cliente 
WHERE ciudad = 'Madrid' AND codigo_empleado_rep_ventas = 11 OR codigo_empleado_rep_ventas = 30;
```

### 1.4.8 Subqueries

#### 1.4.8.1 With basic comparison operators

1. Returns the name of the customer with the highest credit limit.

```sql
SELECT nombre_cliente 
FROM cliente 
WHERE limite_credito = (SELECT MAX(limite_credito) FROM cliente);
```

2. Returns the name of the product with the most expensive selling price.

```sql
SELECT nombre 
FROM producto  
WHERE precio_venta = (SELECT MAX(precio_venta) FROM producto);
```

3. Returns the name of the product that has sold the most units. (Note that you will have to calculate the total number of units that have been sold of each product from the data in the `detalle_pedido` table)

```sql
SELECT nombre 
FROM producto 
WHERE codigo_producto = (SELECT codigo_producto FROM detalle_pedido GROUP BY codigo_producto ORDER BY SUM(cantidad) DESC LIMIT 1);
```

4. Customers whose credit limit is greater than the payments you have made. (Without using `INNER JOIN`).

```sql
SELECT * FROM cliente
WHERE limite_credito > (SELECT SUM(total) FROM pago WHERE codigo_cliente = cliente.codigo_cliente);
```

5. Returns the product that has the most units in stock.

```sql
SELECT nombre 
FROM producto 
WHERE cantidad_en_stock = (SELECT MAX(cantidad_en_stock) FROM producto);
```

6. Returns the product with the fewest units in stock

```sql
SELECT nombre 
FROM producto 
WHERE cantidad_en_stock = (SELECT MIN(cantidad_en_stock) FROM producto);
```

7. Returns the name, surname and email of the employees who are in charge of **Alberto Soria**.

```sql
SELECT CONCAT(nombre, ' ', apellido1, ' ', apellido2) AS Nombre_empleado, email 
FROM empleado 
WHERE codigo_jefe IN (SELECT distinct j.codigo_empleado FROM empleado e JOIN empleado j ON e.codigo_jefe = j.codigo_empleado WHERE j.nombre LIKE 'al%' );
```
#### 1.4.8.2 Subqueries with ALL and ANY

1. Returns the name of the customer with the highest credit limit.

```sql
SELECT nombre_cliente,limite_credito
FROM cliente
WHERE limite_credito >= ALL (SELECT limite_credito FROM cliente);
```

2. Returns the name of the product with the most expensive selling price.

```sql
SELECT nombre 
FROM producto 
WHERE precio_venta >= ALL (SELECT precio_venta FROM producto);
```

3. Returns the product with the fewest units in stock.

```sql
SELECT nombre 
FROM producto 
WHERE cantidad_en_stock <= ALL (SELECT cantidad_en_stock FROM producto);
```

#### 1.4.8.3 Subqueries with IN and NOT IN

1. Returns the first name, last name1, and position of employees who do not represent any clients.

```sql
SELECT nombre, apellido1, puesto 
FROM empleado 
WHERE codigo_empleado NOT IN (SELECT codigo_empleado_rep_ventas FROM cliente);
```

2. Returns a list showing only customers who have not made any payments.

```sql
SELECT *
FROM cliente 
WHERE codigo_cliente NOT IN (SELECT codigo_cliente FROM pago);
```

3. Returns a list that shows only the clients who have made a payment.

```sql
SELECT *
FROM cliente 
WHERE codigo_cliente IN (SELECT codigo_cliente FROM pago);
```

4. Returns a list of products that have never appeared in an order.

```sql
SELECT * 
FROM producto 
WHERE codigo_producto NOT IN (SELECT codigo_producto FROM detalle_pedido);
```

5. Returns the first name, last name, position and office telephone number of those employees who are not sales representatives of any client.

```sql
SELECT e.nombre, e.apellido1, e.apellido2, o.telefono 
FROM empleado e, oficina o 
WHERE codigo_empleado NOT IN (SELECT codigo_empleado_rep_ventas FROM cliente) AND e.codigo_oficina = o.codigo_oficina;
```

6. Returns the offices where none of the employees who have been the sales representatives of a client who has made the purchase of a product from the 'Frutales' range work.

```sql
SELECT *
FROM oficina
WHERE codigo_oficina NOT IN (SELECT codigo_oficina FROM empleado WHERE codigo_empleado IN (SELECT codigo_empleado_rep_ventas FROM cliente WHERE codigo_cliente IN (SELECT codigo_cliente FROM pedido WHERE codigo_pedido IN(SELECT codigo_pedido FROM detalle_pedido WHERE codigo_producto IN (SELECT codigo_producto FROM producto WHERE gama = 'Frutales')))));
```

7. Returns a list of customers who have placed an order but have not made any payment.

```sql
SELECT * 
FROM cliente 
WHERE codigo_cliente IN (SELECT codigo_cliente FROM pedido) AND codigo_cliente NOT IN(SELECT codigo_cliente FROM pago);
```

#### 1.4.8.4 Subqueries with EXISTS and NOT EXISTS

1.  Returns a list showing only customers who have not made any payments.

```sql
SELECT * 
FROM cliente 
WHERE NOT EXISTS(SELECT codigo_cliente FROM pago WHERE pago.codigo_cliente = cliente.codigo_cliente);
```

2. Returns a list that shows only the clients who have made a payment.

```sql
SELECT * 
FROM cliente 
WHERE EXISTS(SELECT codigo_cliente FROM pago WHERE pago.codigo_cliente = cliente.codigo_cliente);
```

3. Returns a list of products that have never appeared in an order.

```sql
SELECT * 
FROM producto 
WHERE NOT EXISTS(SELECT codigo_producto FROM detalle_pedido WHERE detalle_pedido.codigo_producto = producto.codigo_producto);
```

4. Returns a list of the products that have ever appeared in an order.

```sql
SELECT * 
FROM producto 
WHERE EXISTS(SELECT codigo_producto FROM detalle_pedido WHERE detalle_pedido.codigo_producto = producto.codigo_producto);
```
-------------------------------------------------------------------------------------------------------------------------------------------------------

# TIPS SQL VIDEOS


# TIPS GROUP BY VIDEO

1. Multiple grouping

- List how many units each range has in stock.

    ```sql
    SELECT g.gama, p.precio_venta, COUNT(*) cantidad_en_stock 
    FROM producto p
    INNER JOIN gama_producto g ON g.gama = p.gama 
    GROUP BY g.gama, p.precio_venta;
    ```

2. Filter by added features

- Show the range and the units in stock greater than 100 of each of them.

    ```sql
    SELECT g.gama, COUNT(*) cantidad_en_stock 
    FROM producto 
    INNER JOIN gama_producto g ON g.gama = producto.gama 
    GROUP BY g.gama 
    HAVING COUNT(*) > 100;
    ```

- Show the range and the quantity in stock of the range that begins with the letter 'F', also the minimum quantity in stock must be greater than 10.

    ```sql
    SELECT g.gama, min(cantidad_en_stock) cantidad_en_stock 
    FROM producto 
    INNER JOIN gama_producto g ON g.gama = producto.gama 
    WHERE SUBSTRING(g.gama, 1, 1) = 'F' 
    GROUP BY g.gama 
    HAVING MIN(cantidad_en_stock) > 10;
    ```

3. Grouped by scalar functions

- Show the first letters of all the ranges there are and the units in stock of each of them.

    ```sql
    SELECT SUBSTRING(g.gama, 1,1) AS letraGama, COUNT(*) cantidad_en_stock 
    FROM producto 
    INNER JOIN gama_producto g ON g.gama = producto.gama 
    GROUP BY SUBSTRING(g.gama, 1,1);
    ```


4. Grouped with UNION ALL

- List the ranges and sales prices, in addition to showing the total number of ranges in products and the amount of price that is repeated.

    ```sql
    SELECT todo, COUNT(*) AS Total 
    FROM (SELECT concat('Gama: ', g.gama) AS todo FROM producto INNER JOIN gama_producto g ON g.gama = producto.gama UNION ALL SELECT CONCAT('Precios de Ventas: ', CAST(producto.precio_venta as char)) AS todo FROM producto) AS Tabla 
    GROUP BY todo;
    ```


5. Concatenate clustering results

- List the types of ranges and the products that belong to it.

    ```sql
    SELECT g.gama, GROUP_CONCAT(producto.nombre ORDER BY producto.nombre DESC SEPARATOR '/ ') AS producto
    FROM producto
    INNER JOIN gama_producto g ON g.gama = producto.gama
    GROUP BY g.gama;
    ```

6. Extra: Totals by grouping

- List the range, sales price and total units of those fields.

    ```sql
    SELECT IFNULL(g.gama, 'TOTAL') AS gama, IFNULL(CAST(producto.precio_venta AS CHAR), 'TOTAL') AS precio, COUNT(*) AS total
    FROM producto
    INNER JOIN gama_producto g ON g.gama = producto.gama
    GROUP BY g.gama, producto.precio_venta WITH ROLLUP;
    ```

# TIPS WHERE VIDEO

1. NOT IN

- List products where their id isn't 'AR-001' and 'FR-1'.

    ```sql
    SELECT codigo_producto 
    FROM producto 
    WHERE codigo_producto NOT IN('AR-001', 'FR-1');
    ```

2. Subquery

- List the ranges that are in the product and that are greater than zero.

    ```sql
    SELECT * 
    FROM gama_producto g 
    WHERE (SELECT count(*) FROM producto  WHERE producto.gama = g.gama) > 0;
    ```

3. REGEX

- Product's name where they contain a 'y' or a 'z'.

    ```sql
    SELECT * 
    FROM producto 
    WHERE nombre REGEXP "[y-z]";
    ```
    
- Range's name where they start with H.

    ```sql
    SELECT * 
    FROM producto 
    WHERE gama REGEXP '^H';
    ```

- Product's name where they end with a.

    ```sql
    SELECT * 
    FROM producto 
    WHERE nombre REGEXP 'a$';
    ```

- Product's name where it contains a blank space.

    ```sql
    SELECT * 
    FROM producto 
    WHERE nombre REGEXP '([A-Za-z]+[\s][A-Za-z]+)';
    ```

4. IN and Subquery

- Product's name where its range starts with A.

    ```sql
    SELECT * 
    FROM producto 
    WHERE gama IN(SELECT gama FROM gama_producto WHERE gama REGEXP '^A');
    ```


5. Functions

- Product's name where its range begins with F.

    ```sql
    SELECT * 
    FROM producto 
    WHERE SUBSTRING(gama,1,1) = 'F';
    ```

# TIPS UPDATE VIDEO

1. Edit using already saved value

- Update the sale price by adding 10.00.

    ```sql
    UPDATE producto 
    SET precio_venta = precio_venta +10.00;
    ```

2. Multiple grouping

-  Update the sales price using a default value.

    ```sql
    UPDATE producto 
    SET precio_venta = DEFAULT;
    ```
Note: It doesn't have a default value, but if it did it would give a result.

3. Edit getting value by subquery

- Update the product's description with the name of the range and the name of the respective product.

    ```sql
    UPDATE producto 
    SET descripcion = (SELECT CONCAT(g.gama, ' ', producto.nombre) FROM gama_producto g WHERE g.gama = producto.gama);
    ```

4. Subqueries in WHERE

- Update the word 'pato' where the first letter of the selected field starts with H.

    ```sql
    UPDATE producto 
    SET prueba = 'pato' 
    WHERE producto.prueba LIKE 'H%';
    ```
Note: The following query can be done, but this database fails in a constraint.

    ```sql
    UPDATE producto 
    SET gama = 'pato' WHERE producto.gama IN(SELECT gama FROM gama_producto g WHERE g.gama LIKE 'H%');
    ```

5. UPDATE con JOIN

- Update to transcribe product ranges by 'gama_producto' ranges.

    ```sql
    UPDATE producto
    INNER JOIN gama_producto ON gama_producto.gama = producto.gama
    SET producto.gama = gama_producto.gama;
    ```

# TIPS SELECT VIDEO

1. Fixed values

- Add a column and assign a value to it if it isn't in the original table.

    ```sql
    SELECT codigo_producto, nombre, gama, 1 AS ghost 
    FROM producto;
    ```

2. Operations with columns

-  Know the total of two columns that are related, but are different tables.

    ```sql
    SELECT DISTINCT d.precio_unidad, producto.precio_venta, (d.precio_unidad + producto.precio_venta) AS Suma FROM producto 
    INNER JOIN detalle_pedido d ON d.codigo_producto = producto.codigo_producto;
    ```

-  Know the total of two columns that are related, but are from different tables and know the product's name with the quantity they have of it.

    ```sql
    SELECT DISTINCT d.precio_unidad, producto.precio_venta, (d.precio_unidad + producto.precio_venta) AS Suma, CONCAT(producto.nombre, ' - ', d.cantidad) AS Almacenado_producto_cantidad 
    FROM producto 
    INNER JOIN detalle_pedido d ON d.codigo_producto = producto.codigo_producto;
    ```

3. Conditions

- List products and list in 'Retornar' when they are equal to 'Membrillero' a 1 appears, but if it's 'Higuera' a 2 and if it's not in the previous condition it returns a 3.

    ```sql
    SELECT nombre, CASE WHEN nombre = 'Membrillero' THEN 1 WHEN nombre = 'Higuera' THEN 2 ELSE 3 END AS Retornar 
    FROM producto;
    ```

4. Subqueries

- List the ranges and the total ranges there are for each product.

    ```sql
    SELECT gama, (SELECT COUNT(producto.gama) FROM producto WHERE producto.gama = gama_producto.gama) AS Total_gamas_productos 
    FROM gama_producto;
    ```

5. Query on Subquery

- List the ranges and the total number of ranges for each product, but greater than 4.

    ```sql
    SELECT gama, Total_gama 
    FROM (SELECT g.gama, COUNT(g.gama) AS Total_gama FROM producto INNER JOIN gama_producto g ON g.gama = producto.gama GROUP BY g.gama) AS MiTabla 
    WHERE MiTabla.Total_gama > 4;
    ```

6. Extra: Autoincremental virtual column

- List an autoincremental id, the product code in relation to the autoincremental and list the product's name.

    ```sql
    SELECT ROW_NUMBER() OVER (ORDER BY (SELECT 1)) AS MiId, codigo_producto, nombre
    FROM producto ORDER BY nombre;
    ```
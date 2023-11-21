# Consultas jardineria

## 1.4.5 Consultas multitabla (Composición interna)

Resuelva todas las consultas utilizando la sintaxis de SQL1 y SQL2. Las consultas con sintaxis de SQL2 se deben resolver con JOIN.

1. Obtén un listado con el nombre de cada cliente y el nombre y apellido de su representante de ventas.

```
SELECT nombre_cliente, CONCAT(nombre, ' ', apellido1,  ' ',apellido2) AS nombre  
FROM cliente 
JOIN empleado ON cliente.codigo_empleado_rep_ventas = empleado.codigo_empleado;
```

2. Muestra el nombre de los clientes que hayan realizado pagos junto con el nombre de sus representantes de ventas.

```
SELECT DISTINCT c.nombre_cliente as Cliente_que_pagó, CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) AS Nombre_representante_de_ventas  
FROM pago p 
JOIN cliente c ON p.codigo_cliente = c.codigo_cliente 
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado;
```

3. Muestra el nombre de los clientes que no hayan realizado pagos junto con el nombre de sus representantes de ventas.

```
SELECT c.nombre_cliente AS Cliente_que_no_pago, CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) AS Nombre_representante_de_ventas 
FROM cliente c 
LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente 
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado 
WHERE p.codigo_cliente IS NULL;
```

4. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```
SELECT DISTINCT c.nombre_cliente AS Cliente_que_pagó, CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) AS Nombre_representante_de_ventas, o.ciudad as Oficina 
FROM pago p 
JOIN cliente c ON p.codigo_cliente = c.codigo_cliente 
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado 
JOIN oficina o ON e.codigo_oficina = o.codigo_oficina;
```

5. Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```
SELECT c.nombre_cliente AS Cliente_que_no_pago, CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) AS Nombre_representante_de_ventas, o.ciudad as Oficina 
FROM cliente c 
LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente 
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado 
JOIN oficina o ON e.codigo_oficina = o.codigo_oficina 
WHERE p.codigo_cliente IS NULL;
```

6. Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.

```
SELECT CONCAT(o.linea_direccion1, ' ', o.linea_direccion2) AS Direccion_oficina 
FROM oficina o 
JOIN empleado e ON o.codigo_oficina = e.codigo_oficina 
JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas 
WHERE c.ciudad = 'Fuenlabrada';
```

7. Devuelve el nombre de los clientes y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```
SELECT c.nombre_cliente, CONCAT(e.nombre, ' ', e.apellido1, ' ',e.apellido2) AS Nombre_de_representante, o.ciudad AS Ciudad_representante 
FROM cliente c 
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado 
JOIN oficina o ON e.codigo_oficina = o.codigo_oficina;
```

8. Devuelve un listado con el nombre de los empleados junto con el nombre de sus jefes.

```
SELECT e.nombre AS Nombre_empleado, j.nombre AS Nombre_jefe 
FROM empleado e 
LEFT JOIN empleado j ON e.codigo_jefe = j.codigo_empleado;
```

9. Devuelve un listado que muestre el nombre de cada empleados, el nombre de su jefe y el nombre del jefe de sus jefe.

```
SELECT e.nombre AS Nombre_empleado, j.nombre AS Nombre_jefe, jj.nombre AS Nombre_jefe_del_jefe  
FROM empleado e 
LEFT JOIN empleado j ON e.codigo_jefe = j.codigo_empleado 
LEFT JOIN empleado jj ON jj.codigo_empleado = j.codigo_jefe;
```

10. Devuelve el nombre de los clientes a los que no se les ha entregado a tiempo un pedido.

```
SELECT p.fecha_esperada,p.fecha_entrega, c.nombre_cliente 
FROM cliente c 
JOIN pedido p ON c.codigo_cliente = p.codigo_cliente 
WHERE p.fecha_entrega > p.fecha_esperada OR p.fecha_entrega IS NULL;
```

11. Devuelve un listado de las diferentes gamas de producto que ha comprado cada cliente.

```
SELECT DISTINCT c.nombre_cliente, g.gama 
FROM cliente c 
JOIN pedido p ON c.codigo_cliente = p.codigo_cliente 
JOIN detalle_pedido d ON p.codigo_pedido = d.codigo_pedido 
JOIN producto pr ON d.codigo_producto = pr.codigo_producto 
JOIN gama_producto g ON pr.gama = g.gama;
```


## 1.4.5 Consultas multitabla (Composición externa)

Resuelva todas las consultas utilizando las cláusulas LEFT JOIN, RIGHT JOIN, NATURAL LEFT JOIN Y NATURAL RIGHT JOIN.

1. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago

```
SELECT DISTINCT c.nombre_cliente 
FROM cliente c 
LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente 
WHERE p.codigo_cliente IS NULL;
```

2. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pedido.

```
SELECT DISTINCT c.nombre_cliente 
FROM cliente c 
LEFT JOIN pedido p ON c.codigo_cliente = p.codigo_cliente 
WHERE p.codigo_cliente IS NULL;
```

3. Devuelve un listado que muestre los clientes que no han realizado ningún pago y los que no han realizado ningún pedido.

```
SELECT DISTINCT c.nombre_cliente 
FROM cliente c 
LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente 
LEFT JOIN pedido pe ON c.codigo_cliente = pe.codigo_cliente 
WHERE p.codigo_cliente is null AND pe.codigo_cliente IS NULL;
```

4. Devuelve un listado que muestre solamente los empleados que no tienen una oficina asociada.

```
SELECT o.codigo_oficina, e.nombre 
FROM empleado e 
RIGHT JOIN oficina o ON e.codigo_oficina = o.codigo_oficina 
WHERE e.codigo_oficina IS NULL;
```

5. Devuelve un listado que muestre solamente los empleados que no tienen un cliente asociado.

```
SELECT DISTINCT nombre 
FROM empleado 
LEFT JOIN cliente ON empleado.codigo_empleado = cliente.codigo_empleado_rep_ventas 
WHERE cliente.codigo_empleado_rep_ventas IS NULL;
```

6. Devuelve un listado que muestre solamente los empleados que no tienen un cliente asociado junto con los datos de la
oficina donde trabajan.

```
SELECT DISTINCT e.nombre, o.* 
FROM empleado e 
LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas 
LEFT JOIN oficina o ON e.codigo_oficina = o.codigo_oficina 
WHERE c.codigo_empleado_rep_ventas IS NULL;
```

7. Devuelve un listado que muestre los empleados que no tienen una oficina asociada y los que no tienen un cliente asociado.

```
SELECT o.codigo_oficina, e.nombre 
FROM empleado e  
RIGHT JOIN oficina o ON e.codigo_oficina = o.codigo_oficina 
LEFT JOIN cliente ON e.codigo_empleado = cliente.codigo_empleado_rep_ventas  
WHERE e.codigo_oficina IS NULL OR cliente.codigo_empleado_rep_ventas IS NULL;
```

8. Devuelve un listado de los productos que nunca han aparecido en un pedido.

```
SELECT pr.nombre 
FROM producto pr 
LEFT JOIN detalle_pedido d ON pr.codigo_producto = d.codigo_producto  
WHERE d.codigo_producto IS NULL;
```

9. Devuelve un listado de los productos que nunca han aparecido en un pedido. El resultado debe mostrar el nombre, la descripción y la imagen del producto.

```
SELECT pr.nombre, pr.descripcion, g.imagen 
FROM producto pr 
LEFT JOIN detalle_pedido d ON pr.codigo_producto = d.codigo_producto 
LEFT JOIN gama_producto g ON pr.gama = g.gama  
WHERE d.codigo_producto IS NULL;
```

10. Devuelve las oficinas donde no trabajan ninguno de los empleados que hayan sido los representantes de ventas de algún cliente que haya realizado la compra de algún producto de la gama Frutales.

```
SELECT DISTINCT o.codigo_oficina
FROM oficina o
LEFT JOIN empleado e ON o.codigo_oficina = e.codigo_oficina
LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas
LEFT JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
LEFT JOIN detalle_pedido d ON p.codigo_pedido = d.codigo_pedido
LEFT JOIN producto pr ON d.codigo_producto = pr.codigo_producto
WHERE pr.gama = 'Frutales' AND c.codigo_empleado_rep_ventas IS NOT NULL AND e.codigo_empleado IS NOT NULL AND c.codigo_cliente IS NOT NULL AND p.codigo_pedido IS NOT NULL AND d.codigo_pedido IS NOT NULL AND pr.codigo_producto IS NOT NULL AND o.codigo_oficina IS NOT NULL; 
```

11. Devuelve un listado con los clientes que han realizado algún pedido pero no han realizado ningún pago.

```
SELECT DISTINCT c.nombre_cliente
FROM cliente c
INNER JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
LEFT JOIN pago pa ON c.codigo_cliente = pa.codigo_cliente
WHERE pa.codigo_cliente IS NULL;
```

12. Devuelve un listado con los datos de los empleados que no tienen clientes asociados y el nombre de su jefe asociado.

```
SELECT DISTINCT e.*, j.nombre AS Nombre_jefe 
FROM empleado e 
LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas 
LEFT JOIN empleado j ON e.codigo_jefe = j.codigo_empleado 
WHERE c.codigo_empleado_rep_ventas IS NULL;
```

## 1.4.5 Consultas multitabla adicionales

1. Devuelve un listado con el código de oficina y la ciudad donde hay oficinas.

```
SELECT codigo_oficina, ciudad 
FROM oficina;
```

2. Devuelve un listado con la ciudad y el teléfono de las oficinas de España.

```
SELECT telefono , ciudad 
FROM oficina
WHERE pais='España';
```

3. Devuelve un listado con el nombre, apellidos y email de los empleados cuyo jefe tiene un código de jefe igual a 7.

```
SELECT DISTINCT CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) AS Nombre_empleado, e.email
FROM empleado e 
JOIN empleado j ON e.codigo_jefe = j.codigo_jefe 
WHERE j.codigo_jefe = 7;
```

4. Devuelve el nombre del puesto, nombre, apellidos y email del jefe de la empresa.

```
SELECT puesto, CONCAT(nombre,' ',apellido1,' ',apellido2) AS Nombre_jefe, email 
FROM empleado 
WHERE codigo_jefe IS NULL;
```

5. Devuelve un listado con el nombre, apellidos y puesto de aquellos empleados que no sean representantes de ventas.

```
SELECT CONCAT(nombre, ' ', apellido1, ' ', apellido2) AS Nombre_empleado, puesto 
FROM empleado 
WHERE puesto != 'Representante Ventas';
```

6. Devuelve un listado con el nombre de los todos los clientes españoles.

```
SELECT nombre_cliente 
FROM cliente 
WHERE cliente.pais = 'Spain';
```

7. Devuelve un listado con los distintos estados por los que puede pasar un pedido.

```
SELECT DISTINCT estado 
FROM pedido;
```

8. Devuelve un listado con el código de cliente de aquellos clientes que realizaron algún pago en 2008. Tenga en cuenta que deberá eliminar aquellos códigos de cliente que aparezcan repetidos. Resuelva la consulta:
• Utilizando la función YEAR de MySOL.
• Utilizando la función DATE_FORMAT de MysQL.
• Sin utilizar ninguna de las funciones anteriores.

```
SELECT codigo_cliente 
FROM pago 
WHERE YEAR(fecha_pago) = 2008;
```

9. Devuelve un listado con el código de pedido, código de cliente, fecha esperada y fecha de entrega de los pedidos que no han sido entregados a tiempo.

```
SELECT codigo_pedido , codigo_cliente, fecha_esperada, fecha_entrega 
FROM pedido 
WHERE fecha_esperada < fecha_entrega;
```

10. Devuelve un listado con el código de pedido, código de cliente, fecha esperada y fecha de entrega de los pedidos cuya fecha de entrega ha sido al menos dos días antes de la fecha esperada.
• Utilizando la función ADDATE de MySOL.
• Utilizando la función DATEDIFF de MysQL.
• ¿Sería posible resolver esta consulta utilizando el operador de suma + o resta?

```
SELECT codigo_pedido , codigo_cliente, fecha_esperada, fecha_entrega 
FROM pedido 
WHERE fecha_entrega <= ADDDATE(fecha_esperada, -2) ;
```

11. Devuelve un listado de todos los pedidos que fueron rechazados en 2009.

```
SELECT * 
FROM pedido 
WHERE estado='rechazado' AND YEAR(fecha_pedido)=2009;
```

12. Devuelve un listado de todos los pedidos que han sido entregados en el mes de enero de cualquier año.

```
SELECT * 
FROM pedido 
WHERE MONTH(fecha_entrega)=1;
```

13. Devuelve un listado con todos los pagos que se realizaron en el año 2008 mediante Paypal. Ordene el resultado de mayor a menor.

```
SELECT * 
FROM pago  
WHERE YEAR(fecha_pago) = 2008 AND forma_pago = 'Paypal' 
ORDER BY fecha_pago DESC;
```

14. Devuelve un listado con todas las formas de pago que aparecen en la tabla pago. Tenga en cuenta que no deben aparecer formas de pago repetidas.

```
SELECT DISTINCT forma_pago 
FROM pago;
```

15. Devuelve un listado con todos los productos que pertenecen a la gama Ornamentales y que tienen más de 100 unidades en stock. El listado deberá estar ordenado por su precio de venta, mostrando en primer lugar los de mayor precio.

```
SELECT codigo_producto, nombre, precio_venta 
FROM producto 
WHERE gama = 'Ornamentales' AND cantidad_en_stock > 100 
ORDER BY precio_venta DESC;
```

16. Devuelve un listado con todos los clientes que sean de la ciudad de Madrid y cuyo representante de ventas tenga el
código de empleado 11 o 30.

```
SELECT nombre_cliente 
FROM cliente 
WHERE ciudad = 'Madrid' AND codigo_empleado_rep_ventas = 11 OR codigo_empleado_rep_ventas = 30;
```

### 1.4.8 Subconsultas

#### 1.4.8.1 Con operadores básicos de comparación

1. Devuelve el nombre del cliente con mayor límite de crédito.

```
SELECT nombre_cliente 
FROM cliente 
WHERE limite_credito = (SELECT MAX(limite_credito) FROM cliente);
```

2. Devuelve el nombre del producto que tenga el precio de venta más caro.

```
SELECT nombre 
FROM producto  
WHERE precio_venta = (SELECT MAX(precio_venta) FROM producto);
```

3. Devuelve el nombre del producto del que se han vendido más unidades. (Tenga en cuenta que tendrá que calcular cuál es el número total de unidades que se han vendido de cada producto a partir de los datos de la tabla `detalle_pedido`)

```
SELECT nombre 
FROM producto 
WHERE codigo_producto = (SELECT codigo_producto FROM detalle_pedido GROUP BY codigo_producto ORDER BY SUM(cantidad) DESC LIMIT 1);
```

4. Los clientes cuyo límite de crédito sea mayor que los pagos que haya realizado. (Sin utilizar `INNER JOIN`).

```
SELECT * FROM cliente
WHERE limite_credito > (SELECT SUM(total) FROM pago WHERE codigo_cliente = cliente.codigo_cliente);
```

5. Devuelve el producto que más unidades tiene en stock.

```
SELECT nombre 
FROM producto 
WHERE cantidad_en_stock = (SELECT MAX(cantidad_en_stock) FROM producto);
```

6. Devuelve el producto que menos unidades tiene en stock.

```
SELECT nombre 
FROM producto 
WHERE cantidad_en_stock = (SELECT MIN(cantidad_en_stock) FROM producto);
```

7. Devuelve el nombre, los apellidos y el email de los empleados que están a cargo de **Alberto Soria**.

```
SELECT CONCAT(nombre, ' ', apellido1, ' ', apellido2) AS Nombre_empleado, email 
FROM empleado 
WHERE codigo_jefe IN (SELECT distinct j.codigo_empleado FROM empleado e JOIN empleado j ON e.codigo_jefe = j.codigo_empleado WHERE j.nombre LIKE 'al%' );
```
#### 1.4.8.2 Subconsultas con ALL y ANY

1. Devuelve el nombre del cliente con mayor límite de crédito.

```
SELECT nombre_cliente,limite_credito
FROM cliente
WHERE limite_credito >= ALL
(SELECT limite_credito FROM cliente);
```

2. Devuelve el nombre del producto que tenga el precio de venta más caro.

```
SELECT nombre 
FROM producto 
WHERE precio_venta >= ALL
(SELECT precio_venta FROM producto);
```

3. Devuelve el producto que menos unidades tiene en stock.

```
SELECT nombre 
FROM producto 
WHERE cantidad_en_stock <= ALL
(SELECT cantidad_en_stock FROM producto);
```

#### 1.4.8.3 Subconsultas con IN y NOT IN

1. Devuelve el nombre, apellido1 y cargo de los empleados que no representen a ningún cliente.

```
SELECT nombre, apellido1, puesto 
FROM empleado 
WHERE codigo_empleado NOT IN (SELECT codigo_empleado_rep_ventas FROM cliente);
```

2. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.

```
SELECT *
FROM cliente 
WHERE codigo_cliente NOT IN (SELECT codigo_cliente FROM pago);
```

3. Devuelve un listado que muestre solamente los clientes que sí han realizado algún pago.

```
SELECT *
FROM cliente 
WHERE codigo_cliente IN (SELECT codigo_cliente FROM pago);
```

4. Devuelve un listado de los productos que nunca han aparecido en un pedido.

```
SELECT * 
FROM producto 
WHERE codigo_producto NOT IN (SELECT codigo_producto FROM detalle_pedido);
```

5. Devuelve el nombre, apellidos, puesto y teléfono de la oficina de aquellos empleados que no sean representante de ventas de ningún cliente.

```
SELECT e.nombre, e.apellido1, e.apellido2, o.telefono 
FROM empleado e, oficina o 
WHERE codigo_empleado NOT IN (SELECT codigo_empleado_rep_ventas FROM cliente) AND e.codigo_oficina = o.codigo_oficina;
```

6. Devuelve las oficinas donde **no trabajan** ninguno de los empleados que hayan sido los representantes de ventas de algún cliente que haya realizado la compra de algún producto de la gama `Frutales`.

```
SELECT *
FROM oficina
WHERE codigo_oficina NOT IN (SELECT codigo_oficina FROM empleado WHERE codigo_empleado IN (SELECT codigo_empleado_rep_ventas FROM cliente WHERE codigo_cliente IN (SELECT codigo_cliente FROM pedido WHERE codigo_pedido IN(SELECT codigo_pedido FROM detalle_pedido WHERE codigo_producto IN (SELECT codigo_producto FROM producto WHERE gama = 'Frutales')))));
```

7. Devuelve un listado con los clientes que han realizado algún pedido pero no han realizado ningún pago.

```
SELECT * 
FROM cliente 
WHERE codigo_cliente IN (SELECT codigo_cliente FROM pedido) AND codigo_cliente NOT IN(SELECT codigo_cliente FROM pago);
```

#### 1.4.8.4 Subconsultas con EXISTS y NOT EXISTS

1. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.

```
SELECT * 
FROM cliente 
WHERE NOT EXISTS(SELECT codigo_cliente FROM pago WHERE pago.codigo_cliente = cliente.codigo_cliente);
```

2. Devuelve un listado que muestre solamente los clientes que sí han realizado algún pago.

```
SELECT * 
FROM cliente 
WHERE EXISTS(SELECT codigo_cliente FROM pago WHERE pago.codigo_cliente = cliente.codigo_cliente);
```

3. Devuelve un listado de los productos que nunca han aparecido en un pedido.

```
SELECT * 
FROM producto 
WHERE NOT EXISTS(SELECT codigo_producto FROM detalle_pedido WHERE detalle_pedido.codigo_producto = producto.codigo_producto);
```

4. Devuelve un listado de los productos que han aparecido en un pedido alguna vez.

```
SELECT * 
FROM producto 
WHERE EXISTS(SELECT codigo_producto FROM detalle_pedido WHERE detalle_pedido.codigo_producto = producto.codigo_producto);
```
-------------------------------------------------------------------------------------------------------------------------------------------------------

# TIPS SQL VIDEOS


# TIPS GROUP BY

1. Multiple grouping

- List how many units each range has in stock.

    ```
    SELECT g.gama, p.precio_venta, COUNT(*) cantidad_en_stock 
    FROM producto p
    INNER JOIN gama_producto g ON g.gama = p.gama 
    GROUP BY g.gama, p.precio_venta;
    ```

2. Filter by added features

- Show the range and the units in stock greater than 100 of each of them.

    ```
    SELECT g.gama, COUNT(*) cantidad_en_stock 
    FROM producto 
    INNER JOIN gama_producto g ON g.gama = producto.gama 
    GROUP BY g.gama 
    HAVING COUNT(*) > 100;
    ```

- Show the range and the quantity in stock of the range that begins with the letter 'F', also the minimum quantity in stock must be greater than 10.

    ```
    SELECT g.gama, min(cantidad_en_stock) cantidad_en_stock 
    FROM producto 
    INNER JOIN gama_producto g ON g.gama = producto.gama 
    WHERE SUBSTRING(g.gama, 1, 1) = 'F' 
    GROUP BY g.gama 
    HAVING MIN(cantidad_en_stock) > 10;
    ```

3. Grouped by scalar functions

- Show the first letters of all the ranges there are and the units in stock of each of them.

    ```
    SELECT SUBSTRING(g.gama, 1,1) AS letraGama, COUNT(*) cantidad_en_stock 
    FROM producto 
    INNER JOIN gama_producto g ON g.gama = producto.gama 
    GROUP BY SUBSTRING(g.gama, 1,1);
    ```


4. Grouped with UNION ALL

- List the ranges and sales prices, in addition to showing the total number of ranges in products and the amount of price that is repeated.

    ```
    SELECT todo, COUNT(*) AS Total 
    FROM (SELECT concat('Gama: ', g.gama) AS todo FROM producto INNER JOIN gama_producto g ON g.gama = producto.gama UNION ALL SELECT CONCAT('Precios de Ventas: ', CAST(producto.precio_venta as char)) AS todo FROM producto) AS Tabla 
    GROUP BY todo;
    ```


5. Concatenate clustering results

- List the types of ranges and the products that belong to it.

    ```
    SELECT g.gama, GROUP_CONCAT(producto.nombre ORDER BY producto.nombre DESC SEPARATOR '/ ') AS producto
    FROM producto
    INNER JOIN gama_producto g ON g.gama = producto.gama
    GROUP BY g.gama;
    ```

6. Extra: Totals by grouping

- List the range, sales price and total units of those fields.

    ```
    SELECT IFNULL(g.gama, 'TOTAL') AS gama, IFNULL(CAST(producto.precio_venta AS CHAR), 'TOTAL') AS precio, COUNT(*) AS total
    FROM producto
    INNER JOIN gama_producto g ON g.gama = producto.gama
    GROUP BY g.gama, producto.precio_venta WITH ROLLUP;
    ```

--------------------------------------------------------------------------------------------------------------

# TIPS WHERE

1. NOT IN

- List products where their id isn't 'AR-001' and 'FR-1'.

    ```
    SELECT codigo_producto 
    FROM producto 
    WHERE codigo_producto NOT IN('AR-001', 'FR-1');
    ```

2. Subquery

- List the ranges that are in the product and that are greater than zero.

    ```
    SELECT * 
    FROM gama_producto g 
    WHERE (SELECT count(*) FROM producto  WHERE producto.gama = g.gama) > 0;
    ```

3. REGEX

- Product's name where they contain a 'y' or a 'z'.

    ```
    SELECT * 
    FROM producto 
    WHERE nombre REGEXP "[y-z]";
    ```
    
- Range's name where they start with H.

    ```
    SELECT * 
    FROM producto 
    WHERE gama REGEXP '^H';
    ```

- Product's name where they end with a.

    ```
    SELECT * 
    FROM producto 
    WHERE nombre REGEXP 'a$';
    ```

- Product's name where it contains a blank space.

    ```
    SELECT * 
    FROM producto 
    WHERE nombre REGEXP '([A-Za-z]+[\s][A-Za-z]+)';
    ```

4. IN and Subquery

- Product's name where its range starts with A.

    ```
    SELECT * 
    FROM producto 
    WHERE gama IN(SELECT gama FROM gama_producto WHERE gama REGEXP '^A');
    ```


5. Functions

- Product's name where its range begins with F.

    ```
    SELECT * 
    FROM producto 
    WHERE SUBSTRING(gama,1,1) = 'F';
    ```
---------------------------------------------------------------------------------------------------------------

# TIPS UPDATE

1. Edit using already saved value

- Update the sale price by adding 10.00.

    ```
    UPDATE producto 
    SET precio_venta = precio_venta +10.00;
    ```

2. Multiple grouping

-  Update the sales price using a default value.

    ```
    UPDATE producto 
    SET precio_venta = DEFAULT;
    ```
Note: It doesn't have a default value, but if it did it would give a result.

3. Edit getting value by subquery

- Update the product's description with the name of the range and the name of the respective product.

    ```
    UPDATE producto 
    SET descripcion = (SELECT CONCAT(g.gama, ' ', producto.nombre) FROM gama_producto g WHERE g.gama = producto.gama);
    ```

4. Subqueries in WHERE

- Update the word 'pato' where the first letter of the selected field starts with H.

    ```
    UPDATE producto 
    SET prueba = 'pato' 
    WHERE producto.prueba LIKE 'H%';
    ```
Note: The following query can be done, but this database fails in a constraint.
    ```
    UPDATE producto 
    SET gama = 'pato' WHERE producto.gama IN(SELECT gama FROM gama_producto g WHERE g.gama LIKE 'H%');
    ```

5. UPDATE con JOIN

- Update to transcribe product ranges by 'gama_producto' ranges.

    ```
    UPDATE producto
    INNER JOIN gama_producto ON gama_producto.gama = producto.gama
    SET producto.gama = gama_producto.gama;
    ```

---------------------------------------------------------------------------------------------------------------

# TIPS SELECT

1. Fixed values

- Add a column and assign a value to it if it isn't in the original table.

    ```
    SELECT codigo_producto, nombre, gama, 1 AS ghost 
    FROM producto;
    ```

2. Operations with columns

-  Know the total of two columns that are related, but are different tables.

    ```
    SELECT DISTINCT d.precio_unidad, producto.precio_venta, (d.precio_unidad + producto.precio_venta) AS Suma FROM producto 
    INNER JOIN detalle_pedido d ON d.codigo_producto = producto.codigo_producto;
    ```

-  Know the total of two columns that are related, but are from different tables and know the product's name with the quantity they have of it.

    ```
    SELECT DISTINCT d.precio_unidad, producto.precio_venta, (d.precio_unidad + producto.precio_venta) AS Suma, CONCAT(producto.nombre, ' - ', d.cantidad) AS Almacenado_producto_cantidad 
    FROM producto 
    INNER JOIN detalle_pedido d ON d.codigo_producto = producto.codigo_producto;
    ```

3. Conditions

- List products and list in 'Retornar' when they are equal to 'Membrillero' a 1 appears, but if it's 'Higuera' a 2 and if it's not in the previous condition it returns a 3.

    ```
    SELECT nombre, CASE WHEN nombre = 'Membrillero' THEN 1 WHEN nombre = 'Higuera' THEN 2 ELSE 3 END AS Retornar 
    FROM producto;
    ```

4. Subqueries

- List the ranges and the total ranges there are for each product.

    ```
    SELECT gama, (SELECT COUNT(producto.gama) FROM producto WHERE producto.gama = gama_producto.gama) AS Total_gamas_productos 
    FROM gama_producto;
    ```

5. Query on Subquery

- List the ranges and the total number of ranges for each product, but greater than 4.

    ```
    SELECT gama, Total_gama 
    FROM (SELECT g.gama, COUNT(g.gama) AS Total_gama FROM producto INNER JOIN gama_producto g ON g.gama = producto.gama GROUP BY g.gama) AS MiTabla 
    WHERE MiTabla.Total_gama > 4;
    ```

6. Extra: Autoincremental virtual column

- List an autoincremental id, the product code in relation to the autoincremental and list the product's name.

    ```
    SELECT ROW_NUMBER() OVER (ORDER BY (SELECT 1)) AS MiId, codigo_producto, nombre
    FROM producto ORDER BY nombre;
    ```
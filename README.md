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
LEFT JOIN empleado e on o.codigo_oficina = e.codigo_oficina
LEFT JOIN cliente c on e.codigo_empleado = c.codigo_empleado_rep_ventas
LEFT JOIN pedido p on c.codigo_cliente = p.codigo_cliente
LEFT JOIN detalle_pedido d on p.codigo_pedido = d.codigo_pedido
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
FROM pedido WHERE fecha_esperada < fecha_entrega;
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
FRM pago;
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


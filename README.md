# Consultas jardineria

## 1.4.5 Consultas multitabla (Composición interna)

Resuelva todas las consultas utilizando la sintaxis de SQL1 y SQL2. Las consultas con sintaxis de SQL2 se deben resolver con JOIN.

1. Obtén un listado con el nombre de cada cliente y el nombre y apellido de su representante de ventas.

```
SELECT nombre_cliente, CONCAT(nombre, ' ', apellido1,  ' ',apellido2) as nombre  
FROM cliente 
JOIN empleado ON cliente.codigo_empleado_rep_ventas = empleado.codigo_empleado;
```

2. Muestra el nombre de los clientes que hayan realizado pagos junto con el nombre de sus representantes de ventas.

```
SELECT DISTINCT c.nombre_cliente as Cliente_que_pagó, CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) as Nombre_representante_de_ventas  
FROM pago p 
JOIN cliente c ON p.codigo_cliente = c.codigo_cliente 
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado;
```

3. Muestra el nombre de los clientes que no hayan realizado pagos junto con el nombre de sus representantes de ventas.

```
SELECT c.nombre_cliente as Cliente_que_no_pago, CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) as Nombre_representante_de_ventas 
FROM cliente c 
LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente 
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado 
WHERE p.codigo_cliente is null;
```

4. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```
SELECT DISTINCT c.nombre_cliente as Cliente_que_pagó, CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) as Nombre_representante_de_ventas, o.ciudad as Oficina 
FROM pago p 
JOIN cliente c ON p.codigo_cliente = c.codigo_cliente 
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado 
JOIN oficina o ON e.codigo_oficina = o.codigo_oficina;
```

5. Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```
SELECT c.nombre_cliente as Cliente_que_no_pago, CONCAT(e.nombre, ' ', e.apellido1, ' ', e.apellido2) as Nombre_representante_de_ventas, o.ciudad as Oficina 
FROM cliente c 
LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente 
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado 
JOIN oficina o ON e.codigo_oficina = o.codigo_oficina 
WHERE p.codigo_cliente IS NULL;
```

6. Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.

```
SELECT CONCAT(o.linea_direccion1, ' ', o.linea_direccion2)  as Direccion_oficina 
FROM oficina o 
JOIN empleado e ON o.codigo_oficina = e.codigo_oficina 
JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas 
WHERE c.ciudad = 'Fuenlabrada';
```

7. Devuelve el nombre de los clientes y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```
SELECT c.nombre_cliente, CONCAT(e.nombre, ' ', e.apellido1, ' ',e.apellido2) as Nombre_de_representante, o.ciudad as Ciudad_representante 
FROM cliente c 
JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado 
JOIN oficina o ON e.codigo_oficina = o.codigo_oficina;
```

8. Devuelve un listado con el nombre de los empleados junto con el nombre de sus jefes.

```
SELECT e.nombre as Nombre_empleado, j.nombre as Nombre_jefe 
FROM empleado e 
LEFT JOIN empleado j ON e.codigo_jefe = j.codigo_empleado;
```

9. Devuelve un listado que muestre el nombre de cada empleados, el nombre de su jefe y el nombre del jefe de sus jefe.

```
SELECT e.nombre as Nombre_empleado, j.nombre as Nombre_jefe, jj.nombre as Nombre_jefe_del_jefe  
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
SELECT DISTINCT e.*, j.nombre as Nombre_jefe 
FROM empleado e 
LEFT JOIN cliente c ON e.codigo_empleado = c.codigo_empleado_rep_ventas 
LEFT JOIN empleado j ON e.codigo_jefe = j.codigo_empleado 
WHERE c.codigo_empleado_rep_ventas IS NULL;
```
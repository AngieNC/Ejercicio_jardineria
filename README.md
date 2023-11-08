# Consultas jardineria

## 1.4.5 Consultas multitabla (Composición interna)

Resuelva todas las consultas utilizando la sintaxis de SQL1 y SQL2. Las consultas con sintaxis de SQL2 se deben resolver con JOIN.

1. Obtén un listado con el nombre de cada cliente y el nombre y apellido de su representante de ventas.

```
select nombre_cliente, codigo_empleado_rep_ventas, codigo_empleado, concat(nombre, ' ', apellido1,  ' ',apellido2) as nombre  from cliente join empleado on cliente.codigo_empleado_rep_ventas = empleado.codigo_empleado;
```

2. Muestra el nombre de los clientes que hayan realizado pagos junto con el nombre de sus representantes de ventas.

```
select distinct p.codigo_cliente, c.nombre_cliente as Cliente_que_pagó, concat(e.nombre, ' ', e.apellido1, ' ', e.apellido2) as Nombre_representante_de_ventas  from pago p join cliente c on p.codigo_cliente = c.codigo_cliente join empleado e on c.codigo_empleado_rep_ventas = e.codigo_empleado;
```

3. Muestra el nombre de los clientes que no hayan realizado pagos junto con el nombre de sus representantes de ventas.

```
select c.codigo_cliente, c.nombre_cliente as Cliente_que_no_pago, concat(e.nombre, ' ', e.apellido1, ' ', e.apellido2) as Nombre_representante_de_ventas from cliente c left join pago p ON c.codigo_cliente = p.codigo_cliente join empleado e on c.codigo_empleado_rep_ventas = e.codigo_empleado where p.codigo_cliente is null;
```

4. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```
select distinct c.codigo_cliente, c.nombre_cliente as Cliente_que_pagó, concat(e.nombre, ' ', e.apellido1, ' ', e.apellido2) as Nombre_representante_de_ventas, o.ciudad as Oficina from pago p join cliente c on p.codigo_cliente = c.codigo_cliente join empleado e on c.codigo_empleado_rep_ventas = e.codigo_empleado join  oficina o on e.codigo_oficina = o.codigo_oficina;
```

5. Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```
select c.codigo_cliente, c.nombre_cliente as Cliente_que_no_pago, concat(e.nombre, ' ', e.apellido1, ' ', e.apellido2) as Nombre_representante_de_ventas, o.ciudad as Oficina from cliente c left join pago p on c.codigo_cliente = p.codigo_cliente join empleado e on c.codigo_empleado_rep_ventas = e.codigo_empleado join oficina o on e.codigo_oficina = o.codigo_oficina where p.codigo_cliente is null;
```

6. Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.

```
select concat(o.linea_direccion1, ' ', o.linea_direccion2)  as Direccion_oficina from  oficina o join empleado e on  o.codigo_oficina = e.codigo_oficina join  cliente c on  e.codigo_empleado = c.codigo_empleado_rep_ventas where c.ciudad = 'Fuenlabrada';
```

7. Devuelve el nombre de los clientes y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```
select c.nombre_cliente, concat(e.nombre, ' ', e.apellido1, ' ',e.apellido2) as Nombre_de_representante, o.ciudad as Ciudad_representante from cliente c join empleado e on c.codigo_empleado_rep_ventas = e.codigo_empleado join oficina o on e.codigo_oficina = o.codigo_oficina;
```

8. Devuelve un listado con el nombre de los empleados junto con el nombre de sus jefes.

```
select e.nombre as Nombre_empleado, j.nombre as Nombre_jefe from empleado e left join empleado j on e.codigo_jefe = j.codigo_empleado;
```

9. Devuelve un listado que muestre el nombre de cada empleados, el nombre de su jefe y el nombre del jefe de sus jefe.

```
select e.nombre as Nombre_empleado, j.nombre as Nombre_jefe, jj.nombre as Nombre_jefe_del_jefe  from empleado e left join empleado j on e.codigo_jefe = j.codigo_empleado left join empleado jj on jj.codigo_empleado = j.codigo_jefe;
```

10. Devuelve el nombre de los clientes a los que no se les ha entregado a tiempo un pedido.

```

```

11. Devuelve un listado de las diferentes gamas de producto que ha comprado cada cliente.

```
select c.nombre_cliente, g.gama from cliente c join pedido p on c.codigo_cliente = p.codigo_cliente join detalle_pedido d on p.codigo_pedido = d.codigo_pedido join producto pr on d.codigo_producto = pr.codigo_producto join gama_producto g on pr.gama = g.gama;
```
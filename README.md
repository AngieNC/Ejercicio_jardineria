# Consultas jardineria

## 1.4.5 Consultas multitabla (Composición interna)

Resuelva todas las consultas utilizando la sintaxis de SQL1 y SQL2. Las consultas con sintaxis de SQL2 se deben resolver con JOIN.

1. Obtén un listado con el nombre de cada cliente y el nombre y apellido de su representante de ventas.

```
select nombre_cliente, codigo_empleado_rep_ventas, codigo_empleado, concat(nombre, ' ', apellido1,  ' ',apellido2) as nombre  from cliente join empleado on cliente.codigo_empleado_rep_ventas = empleado.codigo_empleado;
```

2. Muestra el nombre de los clientes que hayan realizado pagos junto con el nombre de sus representantes de ventas.

```
select distinct p.codigo_cliente, c.nombre_cliente as Cliente_hizo_pago, concat(e.nombre, ' ', e.apellido1, ' ', e.apellido2) as Nombre_representante_ventas  from pago p join cliente c on p.codigo_cliente = c.codigo_cliente join empleado e on c.codigo_empleado_rep_ventas = e.codigo_empleado;
```

3. Muestra el nombre de los clientes que no hayan realizado pagos junto con el nombre de sus representantes de ventas.

```
select c.codigo_cliente, c.nombre_cliente as Cliente_sin_pago, concat(e.nombre, ' ', e.apellido1, ' ', e.apellido2) as Nombre_representante_ventas from cliente c left join pago p ON c.codigo_cliente = p.codigo_cliente join empleado e on c.codigo_empleado_rep_ventas = e.codigo_empleado where p.codigo_cliente IS NULL;
```

4. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

5. Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

6. Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.

7. Devuelve el nombre de los clientes y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

8. Devuelve un listado con el nombre de los empleados junto con el nombre de sus jefes.

9. Devuelve un listado que muestre el nombre de cada empleados, el nombre de su jefe y el nombre del jefe de sus jefe.

10. Devuelve el nombre de los clientes a los que no se les ha entregado a tiempo un pedido.

11. Devuelve un listado de las diferentes gamas de producto que ha comprado cada cliente.
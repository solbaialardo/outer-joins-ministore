-- 1. ¿Por qué usaste LEFT JOIN para la Consulta 1 y no INNER JOIN? ¿Qué se perdería si usaras INNER JOIN?
Se utilizó `LEFT JOIN` porque el objetivo era retener todos los registros de la tabla de la izquierda (`productos`),
independientemente de si tenían una coincidencia en la tabla de la derecha (`ventas`).
Si se hubiera utilizado un `INNER JOIN`, los productos que nunca se vendieron (como el "Hub USB-C 7p" y el "Parlante Bluetooth") 
habrían desaparecido del resultado final, ya que el `INNER JOIN` requiere obligatoriamente que exista información en ambas tablas 
para mostrar la fila.

-- 2. ¿Por qué usaste RIGHT JOIN para la Consulta 2? ¿Qué tabla está a la izquierda y cuál a la derecha en tu consulta?
Se utilizó `RIGHT JOIN` para forzar a la base de datos a traer todas las filas de la tabla de ventas, incluso si el `producto_id` 
registrado no existe en el catálogo. En la consulta estructurada como `FROM productos p RIGHT JOIN ventas v`, la tabla a la izquierda 
es `productos` y la tabla a la derecha es `ventas`. 
Esto nos permite detectar la venta número 10, cuyo producto (999) es un registro huérfano.

-- 3. ¿Qué representan los valores NULL en cada resultado?
Los valores `NULL` indican la ausencia de una coincidencia entre las dos tablas al momento de cruzarlas:
En la Consulta 1: Un `NULL` en `venta_id` significa que el producto existe físicamente en nuestro catálogo de `productos`, 
pero al buscar en el historial de `ventas`, no se encontró ninguna transacción asociada a ese `producto_id`.
En la Consulta 2: Un `NULL` en el `nombre` o `producto_id` de la tabla productos significa que el sistema registró una transacción 
en la tabla `ventas` (venta_id = 10), pero el código de producto ingresado (999) no figura en la base de datos maestra del catálogo.

-- 4. ¿Cuándo usarías FULL OUTER JOIN en un caso real de negocio?
El `FULL OUTER JOIN` es para procesos de conciliación de datos o auditorías integrales. 
Un caso real de negocio sería cruzar la base de datos de facturación (sistema contable) con la base de datos de despachos (sistema logístico). 
Un `FULL OUTER JOIN` permitiría ver en una sola pantalla los pedidos que se cobraron pero no se enviaron, los que se enviaron pero olvidaron facturarse,
y los que fluyeron correctamente por ambos sistemas.

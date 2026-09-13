# RetailChain — UNION y UNION ALL

Ejercicio práctico de SQL para consolidar el inventario de dos sucursales y comprender la diferencia entre UNION y UNION ALL.

## Consultas realizadas

### 1. ¿Cuántas filas devuelve cada consulta y por qué son distintas?

La Consulta 1 con `UNION` devuelve **11 filas**, mientras que la Consulta 2 con `UNION ALL` devuelve **14 filas**.

La diferencia se debe a que `UNION` elimina las filas completamente duplicadas, mientras que `UNION ALL` conserva todos los registros.

En los datos del ejercicio, los productos `103 - Monitor 4K 27"`, `104 - Teclado Mecánico` y `106 - SSD Externo 1TB` aparecen en ambas sucursales con el mismo `id_producto`, `nombre_producto` y `categoria`. Como en la Consulta 1 no se incluye la columna `stock`, estas filas son idénticas y `UNION` elimina una aparición de cada una.

Por otro lado, la Webcam `HD 1080p` aparece con diferentes identificadores: `107` en la Sucursal Norte y `111` en la Sucursal Sur. Por lo tanto, `UNION` no las considera filas duplicadas y mantiene ambas.

### 2. ¿Por qué UNION ALL es más eficiente que UNION?

`UNION ALL` es más eficiente porque combina los resultados sin realizar una operación adicional para eliminar duplicados.

`UNION`, en cambio, necesita comparar las filas de los resultados para identificar y eliminar las que sean completamente duplicadas. Esta operación adicional consume recursos y puede requerir más procesamiento, especialmente cuando se trabaja con grandes cantidades de datos.

Por esta razón, cuando sabemos que necesitamos conservar todos los registros y no necesitamos eliminar duplicados, `UNION ALL` suele ser la opción más eficiente.

### 3. ¿En qué casos de negocio usarías cada uno?

Usaría `UNION` cuando necesitara obtener una lista única de elementos provenientes de diferentes fuentes. Por ejemplo, podría utilizarlo para consolidar una lista de proveedores registrados en diferentes sistemas y eliminar registros completamente duplicados.

También podría utilizar `UNION` para construir una lista única de clientes provenientes de diferentes campañas comerciales cuando las filas sean completamente iguales.

Usaría `UNION ALL` cuando necesitara conservar todos los registros para realizar un análisis de volumen. Por ejemplo, podría utilizarlo para consolidar los registros de pedidos realizados en diferentes canales de venta sin eliminar ninguno.

También podría utilizar `UNION ALL` para consolidar registros de producción de diferentes plantas cuando cada registro represente una operación física que debe mantenerse para realizar conteos o auditorías.

### 4. ¿Qué pasa si las columnas de ambas consultas no coinciden en número o tipo?

Las dos partes de un `UNION` o `UNION ALL` deben tener el mismo número de columnas y las columnas deben estar en posiciones compatibles.

Si una consulta tiene un número diferente de columnas, SQL genera un error porque no puede combinar los resultados.

Por ejemplo, si la primera consulta seleccionara cuatro columnas y la segunda solo tres, se produciría un error indicando que las consultas combinadas deben tener el mismo número de expresiones en sus listas de destino.

Además, los tipos de datos de las columnas correspondientes deben ser compatibles. Si se intentan combinar tipos que no pueden convertirse entre sí, SQL puede generar un error de incompatibilidad de tipos.

En este ejercicio, las dos partes de cada operación tienen el mismo número y orden de columnas, por lo que pueden combinarse correctamente.

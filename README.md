# RetailChain — UNION y UNION ALL

Ejercicio práctico de SQL para consolidar el inventario de dos sucursales y analizar la diferencia entre UNION y UNION ALL.

## Consultas realizadas

### 1. ¿Cuántas filas devuelve cada consulta y por qué son distintas?

La Consulta 1 con `UNION` devuelve **11 filas**, mientras que la Consulta 2 con `UNION ALL` devuelve **14 filas**.

La Sucursal Norte tiene 7 registros y la Sucursal Sur tiene otros 7 registros. Por lo tanto, `UNION ALL` conserva los 14 registros:

7 + 7 = 14 filas.

En cambio, `UNION` elimina las filas completamente duplicadas. En este ejercicio, los productos `103 - Monitor 4K 27"`, `104 - Teclado Mecánico` y `106 - SSD Externo 1TB` aparecen en ambas sucursales con el mismo `id_producto`, `nombre_producto` y `categoria`.

Como la Consulta 1 selecciona esas tres columnas, cada uno de esos productos aparece como una fila duplicada y `UNION` elimina una de las apariciones.

Por eso:

14 registros originales - 3 duplicados = 11 filas.

Es importante aclarar que `UNION` elimina filas completamente idénticas, no productos que simplemente tengan el mismo nombre.

Por ejemplo, la `Webcam HD 1080p` aparece en Norte con `id_producto = 107` y en Sur con `id_producto = 111`. Aunque tienen el mismo nombre y categoría, sus identificadores son diferentes, por lo que `UNION` conserva ambas filas.

### 2. ¿Por qué UNION ALL es más eficiente que UNION?

`UNION ALL` suele ser más eficiente porque simplemente combina o concatena los resultados de las dos consultas y conserva todas las filas. No necesita realizar una operación adicional para identificar y eliminar duplicados.

`UNION`, en cambio, debe comparar los registros del resultado para determinar cuáles son duplicados. Dependiendo del motor de base de datos y del plan de ejecución, esta eliminación de duplicados puede realizarse mediante operaciones como **ordenamiento (sorting) o hashing**.

Estas operaciones adicionales consumen recursos de CPU y memoria y pueden aumentar el tiempo de ejecución cuando se trabaja con grandes volúmenes de datos.

Por ejemplo, si las sucursales tuvieran millones de registros, `UNION ALL` podría ser más eficiente cuando necesitamos conservar todos los registros y no necesitamos eliminar duplicados.

Por eso, la elección depende del objetivo del análisis: si necesitamos eliminar duplicados usamos `UNION`; si necesitamos conservar todos los registros usamos `UNION ALL`.

### 3. ¿En qué casos de negocio usarías cada uno?

Usaría `UNION` cuando necesite construir una lista única a partir de diferentes fuentes y quiera eliminar registros completamente duplicados.

Por ejemplo, una empresa podría consolidar los registros de proveedores provenientes de dos sistemas administrativos y utilizar `UNION` para obtener una lista única cuando los registros duplicados sean exactamente iguales.

Otro caso sería consolidar listas de productos disponibles provenientes de diferentes catálogos cuando se necesite obtener un catálogo único sin filas repetidas.

Usaría `UNION ALL` cuando necesite conservar todos los registros porque cada fila representa un evento, transacción o registro que debe contabilizarse.

Por ejemplo, una empresa podría combinar las ventas registradas en dos canales comerciales para calcular el número total de transacciones, sin eliminar registros repetidos.

Otro caso sería consolidar los registros de producción de diferentes plantas para analizar el volumen total producido, manteniendo cada registro individual para realizar conteos y auditorías.

### 4. ¿Qué pasa si las columnas de ambas consultas no coinciden en número o tipo?

Para utilizar `UNION` o `UNION ALL`, ambas consultas deben devolver el mismo número de columnas y las columnas correspondientes deben ocupar la misma posición.

Si el número de columnas no coincide, SQL genera un error porque no puede combinar resultados con estructuras diferentes.

Por ejemplo, esta combinación sería incorrecta:

```sql
SELECT id_producto, nombre_producto, categoria
FROM inventario_sucursal_norte

UNION

SELECT id_producto, nombre_producto
FROM inventario_sucursal_sur;

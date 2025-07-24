# 📘 30 funciones DAX sencillas y de uso diario

### 1. Total de ventas
```dax
TotalVentas = SUM(FactVentas[Importe])
```
Suma todos los valores de la columna `Importe` en la tabla de hechos `FactVentas`.

**Ejemplo de datos:**
| Importe |
|---------|
| 100     |
| 200     |
| 50      |

**Resultado esperado:** `TotalVentas = 350`

✅ Asegúrate de que la columna contiene solo números (sin texto).

---

### 2. Conteo de productos distintos
```dax
ProductosDistintos = DISTINCTCOUNT(DimProducto[ID])
```
Cuenta cuántos valores únicos hay en la columna `ID` de la tabla `DimProducto`.

**Ejemplo de datos:**
| ID |
|----|
| 1  |
| 2  |
| 2  |
| 3  |

**Resultado esperado:** `ProductosDistintos = 3`

✅ Útil para eliminar duplicados al contar.

---

### 3. Valor promedio de ventas
```dax
PromedioVentas = AVERAGE(FactVentas[Importe])
```
Calcula el promedio (media) de los importes en la tabla de ventas.

**Ejemplo de datos:**
| Importe |
|---------|
| 100     |
| 200     |
| 300     |

**Resultado esperado:** `PromedioVentas = 200`

✅ Ignora automáticamente los valores en blanco.

---

### 4. Valor mínimo
```dax
VentaMinima = MIN(FactVentas[Importe])
```
Devuelve el valor más bajo de la columna `Importe`.

**Ejemplo de datos:**
| Importe |
|---------|
| 100     |
| 80      |
| 120     |

**Resultado esperado:** `VentaMinima = 80`

✅ Asegúrate de que no hay errores o textos en los datos.

---

### 5. Valor máximo
```dax
VentaMaxima = MAX(FactVentas[Importe])
```
Devuelve el valor más alto de la columna `Importe`.

**Ejemplo de datos:**
| Importe |
|---------|
| 100     |
| 80      |
| 120     |

**Resultado esperado:** `VentaMaxima = 120`

✅ Se ignoran valores vacíos automáticamente.

---### 6. Concatenar texto dinámico
```dax
ResumenVentas = "Total: " & FORMAT([TotalVentas], "#,##0.00 €")
```
Concatena texto con el total de ventas, aplicando formato numérico.

**Ejemplo:**
Si `[TotalVentas] = 1234.5`, entonces `ResumenVentas = "Total: 1.234,50 €"`

✅ Ideal para tarjetas o visuales de resumen.

---

### 7. Ventas del mes actual
```dax
VentasMesActual = 
CALCULATE(
    [TotalVentas],
    MONTH(DimTiempo[Fecha]) = MONTH(TODAY()) &&
    YEAR(DimTiempo[Fecha]) = YEAR(TODAY())
)
```
Calcula las ventas del mes actual según la fecha de hoy.

**Ejemplo:** Si hoy es 2025-07-24, se filtran solo registros de julio 2025.

✅ Requiere que `DimTiempo[Fecha]` esté en formato de fecha.

---

### 8. Ventas del mes pasado
```dax
VentasMesPasado = 
CALCULATE(
    [TotalVentas],
    PREVIOUSMONTH(DimTiempo[Fecha])
)
```
Suma las ventas del mes anterior al mes actual.

**Ejemplo:** Si hoy es julio 2025, traerá datos de junio 2025.

✅ Requiere tabla de fechas marcada como calendario en el modelo.

---

### 9. Acumulado YTD
```dax
VentasYTD = 
TOTALYTD(
    [TotalVentas],
    DimTiempo[Fecha]
)
```
Suma acumulada de ventas desde el 1 de enero hasta la fecha actual.

**Ejemplo:** Si hoy es 24/07/2025, suma desde el 01/01/2025 al 24/07/2025.

✅ Requiere que `DimTiempo[Fecha]` esté en una tabla de fechas válida.

---

### 10. Ranking por ventas
```dax
RankingVentas = 
RANKX(
    ALL(DimProducto),
    [TotalVentas],
    ,
    DESC
)
```
Asigna un número de orden a cada producto según sus ventas (mayor venta = 1).

**Ejemplo:**
| Producto | TotalVentas | RankingVentas |
|----------|-------------|----------------|
| A        | 500         | 2              |
| B        | 1000        | 1              |

✅ Usa `ALL()` para ignorar el filtro de producto y comparar todos.

---### 11. Diferencia de ventas entre años
```dax
DiferenciaYoY = [TotalVentas] - [VentasAñoAnterior]
```
Calcula la diferencia de ventas entre este año y el anterior.

**Ejemplo:** `TotalVentas = 1200`, `VentasAñoAnterior = 1000`  
**Resultado esperado:** `DiferenciaYoY = 200`

✅ Necesita tener definida previamente la medida `VentasAñoAnterior`.

---

### 12. Porcentaje de crecimiento anual
```dax
CrecimientoYoY = DIVIDE([DiferenciaYoY], [VentasAñoAnterior])
```
Calcula el porcentaje de crecimiento comparado con el año anterior.

**Ejemplo:** `DiferenciaYoY = 200`, `VentasAñoAnterior = 1000`  
**Resultado esperado:** `CrecimientoYoY = 0.2` (20%)

✅ Usa `DIVIDE()` para evitar errores por división por cero.

---

### 13. Ventas por cliente
```dax
VentasPorCliente = 
CALCULATE(
    [TotalVentas],
    ALLEXCEPT(DimCliente, DimCliente[ID])
)
```
Muestra el total de ventas por cliente, manteniendo solo el filtro de cliente.

✅ Útil en tablas con múltiples columnas filtradas.

---

### 14. Identificar si un cliente es nuevo
```dax
ClienteNuevo = 
IF(
    MIN(DimTiempo[Año]) = YEAR(TODAY()),
    "Sí",
    "No"
)
```
Identifica si el primer año de compra del cliente es el año actual.

✅ Usa correctamente la relación entre fechas y clientes.

---

### 15. Conteo condicional
```dax
PedidosGrandes = 
CALCULATE(
    COUNTROWS(FactPedidos),
    FactPedidos[Importe] > 1000
)
```
Cuenta cuántos pedidos tienen importe mayor a 1000.

✅ Se debe aplicar correctamente el filtro dentro de `CALCULATE`.

---

### 16. Promedio por categoría
```dax
PromedioPorCategoria = 
CALCULATE(
    AVERAGE(FactVentas[Importe]),
    ALLEXCEPT(DimProducto, DimProducto[Categoria])
)
```
Calcula el promedio de ventas por categoría de producto.

✅ Muy útil en visuales agrupados.

---

### 17. Conteo de días activos
```dax
DiasConVentas = 
CALCULATE(
    DISTINCTCOUNT(DimTiempo[Fecha]),
    FactVentas[Importe] > 0
)
```
Cuenta cuántos días se registraron ventas mayores a cero.

✅ Buena métrica de actividad comercial.

---

### 18. Porcentaje del total
```dax
PorcentajeVentas = 
DIVIDE(
    [TotalVentas],
    CALCULATE(SUM(FactVentas[Importe]), ALL(DimProducto))
)
```
Muestra qué porcentaje representa cada valor del total global.

✅ Muy útil en gráficos de torta o KPIs.

---

### 19. Valor anterior (mes)
```dax
VentasAnterior = 
CALCULATE(
    [TotalVentas],
    PREVIOUSMONTH(DimTiempo[Fecha])
)
```
Obtiene el total de ventas del mes anterior.

✅ Asegúrate de tener una tabla de fechas activa.

---

### 20. Variación mensual
```dax
VariacionMensual = [TotalVentas] - [VentasAnterior]
```
Calcula la diferencia entre ventas del mes actual y el mes anterior.

✅ Muy útil para indicadores de tendencia.

---

### 21. Suma condicional (por país)
```dax
VentasEspaña = 
CALCULATE(
    [TotalVentas],
    DimCliente[País] = "España"
)
```
Suma las ventas solo de clientes cuyo país sea España.

✅ Ideal para análisis geográficos.

---

### 22. Identificar valores nulos
```dax
EsNulo = 
IF(ISBLANK(FactVentas[Importe]), "Sin valor", "OK")
```
Evalúa si hay valores en blanco y devuelve un texto personalizado.

✅ Ideal para revisar calidad de datos.

---

### 23. Duración entre fechas (en días)
```dax
DuracionDias = 
DATEDIFF(FactPedidos[FechaPedido], FactPedidos[FechaEntrega], DAY)
```
Calcula cuántos días pasan entre pedido y entrega.

✅ `DATEDIFF` permite cambiar a semanas, meses o años.

---

### 24. Texto dinámico por selección
```dax
TextoResumen = 
"Categoría seleccionada: " & SELECTEDVALUE(DimProducto[Categoria], "Todas")
```
Devuelve un texto con la categoría seleccionada o "Todas" si hay más de una.

✅ Útil en tarjetas o encabezados de página.

---

### 25. Año-Mes formateado
```dax
AñoMes = 
FORMAT(DimTiempo[Fecha], "YYYY-MM")
```
Convierte la fecha en un texto con formato año-mes (útil para ejes).

✅ No es numérico, solo visual.

---

### 26. Semanas transcurridas desde una fecha
```dax
SemanasTranscurridas = 
DATEDIFF(DimTiempo[FechaInicio], TODAY(), WEEK)
```
Calcula cuántas semanas han pasado desde una fecha hasta hoy.

✅ Puede adaptarse fácilmente a días, meses o años.

---

### 27. Total ventas solo productos activos
```dax
VentasActivas = 
CALCULATE(
    [TotalVentas],
    DimProducto[Activo] = TRUE()
)
```
Suma las ventas únicamente de productos marcados como activos.

✅ Requiere columna booleana o tipo TRUE/FALSE.

---

### 28. Primera fecha de venta
```dax
PrimeraVenta = MIN(FactVentas[Fecha])
```
Muestra la fecha más antigua de venta registrada.

✅ Asegúrate de que la columna esté en formato fecha.

---

### 29. Última fecha de venta
```dax
UltimaVenta = MAX(FactVentas[Fecha])
```
Muestra la fecha más reciente de venta registrada.

✅ Muy útil para validar si los datos están actualizados.

---

### 30. Total promedio por día
```dax
PromedioDiario = 
DIVIDE([TotalVentas], DISTINCTCOUNT(FactVentas[Fecha]))
```
Calcula el promedio diario de ventas.

✅ Ignora días sin ventas si no existen registros.

---

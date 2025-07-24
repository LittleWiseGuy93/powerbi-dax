# 📊 Power BI · DAX Avanzado para Casos Reales

Este repositorio contiene medidas y fórmulas DAX avanzadas basadas en **casos reales de negocio**, organizadas por área temática. Estas fórmulas están pensadas para ser reutilizadas en proyectos de visualización y análisis profesional con Power BI.

---

## 🧱 Estructura del repositorio

```
powerbi-dax-advanced/
├── finanzas/           # Análisis de ventas, previsiones, KPIs financieros
├── rrhh/               # Headcount, nómina, empleados activos, rotación
├── operaciones/        # Logística, inventario, eficiencia operativa
```

---

## 🧠 Fórmulas incluidas

### Finanzas

- **forecast_lineal_simple.dax**  
  Proyección lineal de ventas basada en promedio histórico.

- **top_n_con_otros.dax**  
  Ranking dinámico de top N clientes con agrupación en "Otros".

---

### RRHH

- **empleados_activos_por_dia.dax**  
  Conteo de empleados válidos en un día, considerando fecha de entrada y salida.

---

### Operaciones

- **stock_proyectado.dax**  
  Cálculo de inventario futuro neto (entradas - salidas acumuladas hacia adelante).

- **duracion_promedio_dias.dax**  
  Promedio de duración de procesos entre fecha inicio y fin.

---

## 🧩 Requisitos

- Relación entre las tablas de hechos y la tabla de fechas (`DimTiempo`).
- Tablas bien normalizadas (por ejemplo: `FactVentas`, `FactSalidas`, `DimCliente`).
- Power BI Desktop con medidas creadas en el modelo de datos o mediante `DAX Studio`.

---

## ✍️ Autor

Desarrollado por **Juan Rodríguez** ([@LittleWiseGuy93](https://github.com/LittleWiseGuy93)) como parte de mi repositorio personal de prácticas avanzadas con Power BI.

---

---

## Recursos

  - https://learn.microsoft.com/es-es/dax/dax-function-reference
  - https://learn.microsoft.com/es-es/power-bi/transform-model/desktop-quickstart-learn-dax-basics
  - https://community.fabric.microsoft.com/t5/Power-BI-forums/ct-p/powerbi
  - https://stackoverflow.com/questions/tagged/dax
  - https://dax.guide/

---

## 📄 Licencia

MIT — libre de usar y adaptar en cualquier entorno profesional.

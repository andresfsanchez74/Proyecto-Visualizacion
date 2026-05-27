# Recomendaciones básicas para organizar posición y tamaño de gráficos en Power BI

## Maestría en Analítica Aplicada  
**Asignatura: Herramientas de Visualización para la Inteligencia de Negocios**  
**Docente: Luis Alberto Jaimes C.**

Este documento presenta recomendaciones prácticas para organizar la posición y el tamaño de los gráficos dentro de cada página de un informe en Power BI. El objetivo es mejorar la lectura, la jerarquía visual, la comparación entre indicadores y la experiencia del usuario final.

---

## 1. Principio general de organización

Antes de ubicar los gráficos, se define qué debe leer primero el usuario.

Una página de Power BI no debe organizarse únicamente por estética, sino por prioridad analítica. La estructura recomendada es:

1. **Título de la página**
2. **Filtros o segmentadores principales**
3. **Indicadores clave o KPI**
4. **Gráfico principal**
5. **Gráficos complementarios**
6. **Tablas o detalles**

Esta secuencia ayuda a que el usuario pase de una lectura general a una lectura más específica.

---

## 2. Tamaño de página recomendado

Para dashboards académicos, ejecutivos o de clase, se recomienda trabajar con formato panorámico:

```text
16:9
```

En Power BI:

1. Se hace clic en un espacio vacío de la página.
2. Se accede al panel **Formato de página**.
3. Se abre **Configuración del lienzo** o **Tamaño del lienzo**.
4. Se selecciona **16:9**.

Este formato se adapta bien a pantallas, proyectores y presentaciones.

---

## 3. Estructura base de una página

Una página clara puede organizarse en zonas:

```text
┌──────────────────────────────────────────────┐
│ Título de la página                          │
├──────────────┬───────────────────────────────┤
│ Filtros      │ KPIs principales              │
├──────────────┼───────────────────────────────┤
│ Filtros      │ Gráfico principal             │
│              │                               │
├──────────────┼───────────────┬───────────────┤
│              │ Gráfico 2     │ Gráfico 3     │
└──────────────┴───────────────┴───────────────┘
```

Otra estructura posible es:

```text
┌──────────────────────────────────────────────┐
│ Título + subtítulo                           │
├──────────────────────────────────────────────┤
│ Filtros horizontales                         │
├──────────┬──────────┬──────────┬─────────────┤
│ KPI 1    │ KPI 2    │ KPI 3    │ KPI 4       │
├──────────────────────────────┬───────────────┤
│ Gráfico principal            │ Gráfico apoyo │
├──────────────────────────────┼───────────────┤
│ Gráfico secundario           │ Tabla detalle │
└──────────────────────────────┴───────────────┘
```

---

## 4. Ubicación recomendada de los elementos

## 4.1. Título de página

El título debe ubicarse en la parte superior izquierda o superior central.

| Elemento | Recomendación |
|---|---|
| Posición | Parte superior |
| Altura aproximada | 40 a 60 px |
| Alineación | Izquierda o centro |
| Tamaño de fuente | 20 a 28 px |
| Color | Gris oscuro o color principal del informe |

Ejemplo de título adecuado:

```text
Crecimiento y rentabilidad comercial
```

Se evitan títulos demasiado generales como:

```text
Página 1
Ventas
Gráficos
```

---

## 4.2. Segmentadores o filtros

Los filtros deben ubicarse de forma consistente en todas las páginas.

### Opción A: filtros en columna lateral izquierda

Útil cuando hay varios filtros.

```text
┌──────────────┬───────────────────────────────┐
│ Filtros      │ Visualizaciones               │
│ Año          │                               │
│ Región       │                               │
│ Canal        │                               │
│ Categoría    │                               │
└──────────────┴───────────────────────────────┘
```

| Elemento | Recomendación |
|---|---|
| Ancho | 180 a 240 px |
| Ubicación | Lado izquierdo |
| Uso | Cuando hay más de 3 filtros |
| Ventaja | Mantiene limpia la zona central |

### Opción B: filtros en fila superior

Útil cuando hay pocos filtros.

```text
┌──────────────────────────────────────────────┐
│ Título                                       │
├──────────┬──────────┬──────────┬─────────────┤
│ Año      │ Región   │ Canal    │ Categoría   │
└──────────┴──────────┴──────────┴─────────────┘
```

| Elemento | Recomendación |
|---|---|
| Altura | 50 a 80 px |
| Ubicación | Debajo del título |
| Uso | Cuando hay 2 a 4 filtros |
| Ventaja | Deja más espacio horizontal para los gráficos |

---

## 4.3. Tarjetas KPI

Las tarjetas KPI deben ubicarse cerca de la parte superior, después del título y los filtros.

```text
┌────────────┬────────────┬────────────┬────────────┐
│ Ventas     │ Margen     │ Clientes   │ Descuento  │
│ $1.2M      │ 32.4%      │ 250        │ 8.5%       │
└────────────┴────────────┴────────────┴────────────┘
```

| Elemento | Recomendación |
|---|---|
| Cantidad de KPI | 3 a 5 por página |
| Altura | 80 a 120 px |
| Ancho | Igual para todos |
| Alineación | Horizontal |
| Espacio entre tarjetas | Constante |
| Formato | Valor grande y etiqueta pequeña |

No es recomendable poner demasiadas tarjetas KPI, porque el usuario pierde claridad sobre qué indicadores son realmente importantes.

---

## 4.4. Gráfico principal

Cada página debe tener un gráfico principal. Este gráfico debe ser el más grande y ocupar la zona central.

| Elemento | Recomendación |
|---|---|
| Tamaño | Entre 40% y 60% del área útil de la página |
| Ubicación | Centro o centro-izquierda |
| Función | Responder la pregunta principal de la página |
| Prioridad visual | Alta |

Ejemplos:

| Página | Gráfico principal sugerido |
|---|---|
| Crecimiento y rentabilidad | Línea y columnas por mes |
| Portafolio y canales | Matriz o heatmap de margen por región y canal |
| Clientes y descuentos | Pareto de ventas por país o cliente |
| Análisis estadístico | Boxplot o distribución por categoría |

---

## 4.5. Gráficos secundarios

Los gráficos secundarios deben complementar al gráfico principal. No deben competir con él.

| Elemento | Recomendación |
|---|---|
| Tamaño | Menor que el gráfico principal |
| Ubicación | Derecha, inferior o zona complementaria |
| Función | Explicar detalles, segmentos o comparaciones |
| Cantidad | 2 a 4 por página |

Ejemplo:

```text
┌──────────────────────────────┬───────────────┐
│ Gráfico principal            │ Treemap       │
│                              ├───────────────┤
│                              │ Mapa          │
└──────────────────────────────┴───────────────┘
```

---

## 4.6. Tablas de detalle

Las tablas deben ubicarse normalmente en la parte inferior o en una zona secundaria.

| Elemento | Recomendación |
|---|---|
| Ubicación | Parte inferior |
| Altura | 180 a 300 px |
| Uso | Cuando se necesita detalle numérico |
| Formato | Pocas columnas y ordenamiento claro |

Se evitan tablas con demasiadas columnas. Una tabla muy grande convierte el dashboard en una hoja de cálculo y dificulta la lectura visual.

---

## 5. Recomendaciones de tamaño según tipo de visual

| Tipo de visual | Tamaño recomendado | Comentario |
|---|---|---|
| Tarjeta KPI | Pequeño | Debe mostrar un valor clave |
| Gráfico de barras | Mediano o grande | Necesita espacio para etiquetas |
| Gráfico de líneas | Grande | Requiere lectura temporal clara |
| Gráfico combinado | Grande | Tiene más de una métrica |
| Treemap | Mediano | Funciona mejor con pocas categorías |
| Mapa | Mediano o grande | Requiere espacio geográfico |
| Matriz | Grande | Necesita espacio para filas y columnas |
| Tabla | Mediana | Debe usarse como apoyo |
| Boxplot | Mediano o grande | Requiere espacio para comparar grupos |
| Heatmap | Grande | Necesita lectura cruzada de filas y columnas |

---

## 6. Uso de cuadrícula y alineación

Power BI permite mover visuales libremente, pero es recomendable trabajar con una cuadrícula visual.

Buenas prácticas:

- Se alinean los bordes izquierdos de los gráficos.
- Se mantiene la misma altura en tarjetas KPI.
- Se mantiene el mismo ancho en gráficos comparables.
- Se usan márgenes constantes entre visuales.
- Se evita que los elementos queden “casi alineados”.
- Se deja espacio en blanco suficiente.

Para mejorar la alineación, se recomienda seguir este procedimiento:

1. Se seleccionan varios visuales.
2. Se usan las opciones de **Formato > Alinear**.
3. Se usa **Distribuir horizontalmente** o **Distribuir verticalmente**.
4. Se activan las guías de alineación si están disponibles.

---

## 7. Espaciado recomendado

El espacio en blanco también comunica orden. No debe llenarse toda la página con gráficos.

| Elemento | Espacio sugerido |
|---|---|
| Margen exterior de la página | 20 a 40 px |
| Separación entre tarjetas KPI | 8 a 16 px |
| Separación entre gráficos | 12 a 24 px |
| Separación entre título y filtros | 8 a 16 px |
| Separación entre filtros y KPIs | 12 a 20 px |

Un dashboard saturado obliga al usuario a esforzarse más para encontrar la información importante.

---

## 8. Jerarquía visual

No todos los gráficos deben tener el mismo tamaño. El tamaño debe reflejar importancia.

Una regla útil es:

```text
Mayor importancia analítica = mayor tamaño visual
```

| Elemento | Jerarquía |
|---|---|
| Título | Alta |
| KPI principales | Alta |
| Gráfico principal | Muy alta |
| Gráficos secundarios | Media |
| Tablas auxiliares | Media o baja |
| Filtros | Funcional |

Si todos los gráficos tienen el mismo tamaño, el usuario no sabe por dónde empezar.

---

## 9. Organización recomendada para la página “Crecimiento y Rentabilidad”

```text
┌────────────────────────────────────────────────────┐
│ Crecimiento y rentabilidad                         │
├────────────┬────────────┬────────────┬─────────────┤
│ Ventas     │ Margen     │ Margen %   │ Clientes    │
├────────────┴────────────┴────────────┴─────────────┤
│ Gráfico combinado: ventas y margen por mes         │
├──────────────────────────┬─────────────────────────┤
│ Barras por marca         │ Treemap por categoría   │
├──────────────────────────┴─────────────────────────┤
│ Mapa o tabla por país                              │
└────────────────────────────────────────────────────┘
```

Recomendaciones:

- El gráfico temporal debe ser el visual principal.
- Las tarjetas KPI deben estar arriba.
- El treemap debe ser complementario, no el centro de la página.
- El mapa debe usarse como apoyo geográfico.

---

## 10. Organización recomendada para la página “Portafolio y canales”

```text
┌────────────────────────────────────────────────────┐
│ Portafolio y canales: dónde ganar                  │
├────────────┬────────────┬────────────┬─────────────┤
│ Ventas     │ Margen     │ Cantidad   │ Descuento   │
├──────────────────────────────┬─────────────────────┤
│ Heatmap región-canal         │ Barras por categoría│
│                              │                     │
├──────────────────────────────┴─────────────────────┤
│ Matriz de productos                                │
└────────────────────────────────────────────────────┘
```

Recomendaciones:

- El heatmap debe tener buen tamaño para facilitar la comparación.
- La matriz debe ir abajo, porque funciona como detalle.
- Las barras por categoría ayudan a interpretar el portafolio.

---

## 11. Organización recomendada para la página “Clientes y descuentos”

```text
┌────────────────────────────────────────────────────┐
│ Clientes y descuentos                              │
├────────────┬────────────┬────────────┬─────────────┤
│ Clientes   │ Ventas     │ Margen %   │ Descuento   │
├──────────────────────────────┬─────────────────────┤
│ Pareto de ventas por país    │ Distribución margen │
│                              │ por categoría       │
├──────────────────────────────┴─────────────────────┤
│ Tabla de clientes o países                         │
└────────────────────────────────────────────────────┘
```

Recomendaciones:

- El Pareto debe tener suficiente ancho para ordenar países o clientes.
- La distribución de margen debe tener suficiente altura para mostrar variabilidad.
- La tabla debe complementar, no reemplazar, los gráficos.

---

## 12. Organización recomendada para la página “Análisis estadístico”

```text
┌────────────────────────────────────────────────────┐
│ Análisis estadístico                               │
├────────────────────────────────────────────────────┤
│ Filtros principales                                │
├──────────────────────────────┬─────────────────────┤
│ Boxplot o violin plot        │ KPI estadísticos    │
│                              │                     │
├──────────────────────────────┴─────────────────────┤
│ Tabla resumen por grupo                            │
└────────────────────────────────────────────────────┘
```

Recomendaciones:

- Los gráficos estadísticos necesitan más espacio que una tarjeta.
- Se evita poner muchos gráficos estadísticos pequeños.
- Se incluye una tabla resumen si el análisis requiere valores exactos.

---

## 13. Errores frecuentes de distribución

Se evitan los siguientes errores:

- Se evita ubicar gráficos sin alineación.
- Se evita usar tamaños diferentes para tarjetas KPI equivalentes.
- Se evita poner demasiados gráficos en una sola página.
- Se evita dejar gráficos importantes en zonas pequeñas.
- Se evita colocar tablas enormes en el centro del dashboard.
- Se evita repetir filtros en posiciones diferentes en cada página.
- Se evita usar gráficos pequeños con muchas etiquetas.
- Se evita dejar visuales sin espacio suficiente.
- Se evita usar mapas demasiado pequeños.
- Se evita usar treemaps con demasiadas categorías.

---

## 14. Reglas rápidas de diseño

1. Una página debe responder una pregunta principal.
2. Cada página debe tener un gráfico dominante.
3. Los KPI deben ubicarse arriba.
4. Los filtros deben estar en una zona estable.
5. Las tablas deben ubicarse abajo o en zonas secundarias.
6. Los gráficos comparables deben tener tamaños similares.
7. No se usan más de 6 a 8 visuales por página.
8. Se deja espacio en blanco.
9. Se alinean los visuales por bordes.
10. Se usa el tamaño para indicar importancia.

---

## 15. Lista de verificación final

Antes de entregar el dashboard, se verifica:

- [ ] La página tiene un título claro.
- [ ] Los filtros están ubicados de forma consistente.
- [ ] Los KPI están en la parte superior.
- [ ] Hay un gráfico principal claramente identificable.
- [ ] Los gráficos secundarios complementan el análisis.
- [ ] Las tablas no ocupan más espacio del necesario.
- [ ] Los visuales están alineados.
- [ ] Hay espacio suficiente entre elementos.
- [ ] Las etiquetas son legibles.
- [ ] Los gráficos importantes tienen mayor tamaño.
- [ ] No hay saturación visual.
- [ ] La página puede entenderse en menos de un minuto.

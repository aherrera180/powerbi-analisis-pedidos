# 📊 Análisis de pedidos | Power BI (2018–2022)

## 📖 Descripción del proyecto

Este proyecto analiza el histórico de pedidos de una empresa entre **2018 y 2022** mediante Power BI Desktop.

El objetivo es transformar los datos de pedidos, productos, clientes y vendedores en un informe interactivo que permita consultar los principales indicadores comerciales, analizar su evolución temporal y explorar los resultados desde diferentes perspectivas.

El trabajo se ha desarrollado en dos partes:

- **Análisis de pedidos (I):** carga y combinación de datos, creación del modelo, medidas DAX y dashboard inicial.
- **Análisis de pedidos (II):** análisis temporal, nuevas medidas, páginas de análisis y reporting, parámetros dinámicos y navegación mediante marcadores.

El resultado es un informe de Power BI con varias páginas conectadas, visualizaciones interactivas y herramientas de filtrado.

## 🎯 Objetivos

- Integrar los datos de pedidos correspondientes a varios años.
- Relacionar las tablas de pedidos, clientes, productos y vendedores.
- Crear medidas DAX para calcular los principales KPI comerciales.
- Analizar las ventas, los costes y los beneficios.
- Estudiar la evolución temporal de las ventas.
- Comparar clientes registrados con clientes compradores y productos disponibles con productos vendidos.
- Construir visualizaciones interactivas y tablas de detalle.
- Incorporar filtros, parámetros de campo y marcadores para facilitar la navegación.

## 🛠️ Herramientas utilizadas

| Herramienta      | Utilización                                        |
| ---------------- | -------------------------------------------------- |
| Power BI Desktop | Modelado de datos, cálculos y visualizaciones      |
| Power Query      | Importación, combinación y transformación de datos |
| DAX              | Creación de medidas y tablas calculadas            |
| Microsoft Excel  | Archivos originales de datos                       |
| GitHub           | Documentación y publicación del proyecto           |

## 📂 Datos de partida

Se han utilizado ocho archivos Excel, organizados en cuatro grupos:

| Datos      | Archivos                                | Descripción                           |
| ---------- | --------------------------------------- | ------------------------------------- |
| Pedidos    | `Pedidos2018.xlsx` a `Pedidos2022.xlsx` | Histórico de pedidos de cinco años    |
| Clientes   | `Clientes.xlsx`                         | Información de clientes               |
| Productos  | `Productos.xlsx`                        | Información y categorías de productos |
| Vendedores | `Vendedores.xlsx`                       | Información de vendedores             |

Los cinco archivos de pedidos comparten la misma estructura de columnas, lo que permite combinarlos mediante el conector de carpeta de Power BI.

El conjunto de pedidos combinado contiene **22.826 registros**.

## 🔄 Preparación y transformación de datos

### 1. Carga inicial

Inicialmente importé los pedidos de 2022 para familiarizarme con los datos y crear las primeras visualizaciones.

Después creé una medida para calcular la cantidad de productos y una tabla auxiliar llamada `Medidas`, destinada a organizar los cálculos DAX.

### 2. Combinación de pedidos de varios años

Para ampliar el análisis a todo el periodo 2018–2022, utilicé:

**Obtener datos → Carpeta → Combinar y cargar**

Power BI permitió combinar los cinco archivos Excel en una única consulta, denominada `Pedidos (2)`.

Desde Power Query:

- Comprobé que se habían incorporado los cinco años.
- Revisé la estructura y los tipos de datos.
- Eliminé la tabla inicial de pedidos de 2022, que ya no era necesaria.
- Eliminé `Source.Name`, al disponer de una columna de fecha que permitía identificar el periodo de cada pedido.
- Apliqué las transformaciones al modelo.

### 3. Incorporación de las tablas maestras

Importé los archivos de clientes, productos y vendedores.

Durante la exploración observé algunos valores nulos en la tabla de vendedores, aunque no fue necesario realizar una limpieza general de estas tablas para continuar con el ejercicio.

### 4. Modelo de datos y relaciones

Desde la vista Modelo revisé las relaciones detectadas automáticamente por Power BI.

Para comprender su funcionamiento, eliminé y reconstruí las relaciones entre las tablas utilizando los identificadores compartidos.

Se establecieron relaciones de **varios a uno (\*:1)**, con dirección de filtro cruzado única desde las tablas maestras hacia la tabla de pedidos.

Posteriormente incorporé una tabla Calendario para trabajar con las fechas.

## 📅 Creación de la tabla Calendario

Para analizar la evolución de las ventas, creé una tabla calculada mediante DAX:

```dax
Calendario =
ADDCOLUMNS(
    CALENDAR(
        MIN('Pedidos (2)'[Pedidos.Fecha]),
        MAX('Pedidos (2)'[Pedidos.Fecha])
    ),
    "Año", YEAR([Date]),
    "Mes", MONTH([Date])
)
```

La función `CALENDAR()` genera las fechas comprendidas entre el primer y el último pedido, mientras que `ADDCOLUMNS()` incorpora las columnas de año y mes.

Después relacioné:

- `Calendario[Date]`
- `Pedidos (2)[Pedidos.Fecha]`

Esta tabla permite utilizar una dimensión temporal común para analizar las ventas por años y meses.

## 🧮 Medidas DAX

Para mantener el modelo organizado, agrupé las medidas en una tabla auxiliar denominada `Medidas`.

Entre los principales cálculos utilizados se encuentran:

### Cantidad

```dax
Cantidad =
SUM('Pedidos (2)'[Pedidos.Cantidad])
```

### Clientes registrados

```dax
Clientes =
DISTINCTCOUNT(Clientes[Clientes.Cliente_ID])
```

### Clientes compradores

```dax
Clientes compradores =
DISTINCTCOUNT('Pedidos (2)'[Pedidos.Cliente_ID])
```

### Productos registrados

```dax
Productos =
DISTINCTCOUNT(Productos[Productos.Producto_ID])
```

### Productos vendidos

```dax
Productos vendidos =
DISTINCTCOUNT('Pedidos (2)'[Pedidos.Producto_ID])
```

También creé medidas para calcular **Ventas, Coste, Beneficio y % Beneficio**, utilizadas en los indicadores y visualizaciones del informe.

Las medidas permiten reutilizar los mismos cálculos en diferentes gráficos y tablas.

## 📊 Desarrollo del informe

### 1. Dashboard

La página principal ofrece una visión general de la actividad comercial.

Incluye cinco indicadores:

- Cantidad vendida.
- Total de ventas.
- Coste total.
- Beneficio.
- Porcentaje de beneficio.

Además, incorpora gráficos de pedidos por producto, ventas por comercial, ventas por categoría y ventas por división.

Se añadieron segmentadores para filtrar por país, ciudad, producto, cliente y puesto.

### 2. Análisis

La página **Análisis** amplía la información del dashboard y permite estudiar los datos con mayor detalle.

Contiene:

- Comparativa de ventas y costes por producto, vendedor y cliente.
- Evolución de ventas por año y mes.
- Distribución de beneficios por categoría mediante un treemap.
- Comparación entre clientes registrados y compradores.
- Comparación entre productos disponibles y vendidos.

Los gráficos con jerarquías permiten explorar distintos niveles de detalle.

### 3. Reporting

Creé una página específica para consultar los resultados mediante una matriz.

La matriz utiliza diferentes niveles de detalle:

**Filas:** división, vendedor, cliente y producto.

**Columnas:** categoría de producto.

**Valores:** cantidad, ventas, coste, beneficio y porcentaje de beneficio.

Este formato permite desglosar los indicadores y comprobar cómo se distribuyen entre las distintas dimensiones del modelo.

### 4. Tooltip

Para complementar el gráfico de ventas por comercial, creé una página de información sobre herramientas.

Esta página contiene un gráfico de área que representa la evolución anual de las ventas.

Al pasar el cursor sobre un comercial en el dashboard, Power BI puede mostrar información temporal adicional sin necesidad de abrir otra página.

### 5. Detalle y parámetros dinámicos

En la última fase incorporé una página llamada **Detalle**, orientada a consultar la información de manera flexible.

Utilicé la funcionalidad:

**Modelado → Nuevo parámetro → Campos**

Creé dos parámetros:

- **Parámetro dimensiones:** permite seleccionar las columnas que se desean analizar.
- **Parámetro medidas:** permite escoger los indicadores que se quieren mostrar.

Ambos parámetros se incorporaron a una tabla, de manera que el usuario puede cambiar su contenido mediante segmentadores con formato de mosaico.

También se puede realizar una selección múltiple para combinar diferentes dimensiones y medidas.

### 6. Panel de filtros y marcadores

Para mejorar la navegación y aprovechar mejor el espacio del informe, trabajé con un panel de filtros desplegable.

El proceso incluyó:

1. Crear una forma rectangular como fondo.
2. Colocar los segmentadores sobre esa forma.
3. Organizar los elementos mediante el panel Selección.
4. Alinear y agrupar los objetos bajo el nombre `Filtros`.
5. Crear marcadores para mostrar y ocultar el panel.
6. Configurar acciones sobre formas utilizadas como botones.

En los marcadores desactivé la opción **Datos**, con el objetivo de conservar las selecciones realizadas en los filtros al abrir o cerrar el panel.

Este sistema permite disponer de más espacio para las visualizaciones sin perder el acceso a los segmentadores.

## ⚠️ Incidencias y soluciones durante el desarrollo

Durante el proyecto aparecieron varios problemas que tuve que revisar y corregir mientras trabajaba con Power BI.

He incluido estas incidencias porque forman parte del proceso de aprendizaje y ayudan a entender algunas decisiones tomadas durante el desarrollo.

### 1. Referencias DAX a la tabla de pedidos

**Problema:** algunas fórmulas devolvían errores de sintaxis al utilizar la tabla `Pedidos (2)`.

**Solución:** utilizar comillas simples alrededor del nombre de la tabla:

```dax
'Pedidos (2)'[Pedidos.Fecha]
```

Esto es necesario porque el nombre contiene espacios y paréntesis.

### 2. Visualizaciones que dejaron de funcionar al sustituir la tabla original

Después de combinar los pedidos de los cinco años y eliminar la tabla inicial, algunos gráficos y medidas dejaron de funcionar.

**Solución:** actualizar las referencias DAX y sustituir los campos antiguos por los correspondientes a `Pedidos (2)`.

### 3. Cambios pendientes de aplicar

Power BI mostró repetidamente avisos sobre cambios pendientes en las consultas.

Antes de continuar guardé una copia de seguridad del archivo `.pbix`.

Después apliqué los cambios y comprobé que el dashboard y las tablas seguían funcionando.

Los avances posteriores se realizaron sobre el archivo de respaldo.

### 4. Recuentos inconsistentes de clientes

Al comparar los clientes registrados con los compradores, una visualización llegó a mostrar 34 compradores, aunque posteriormente el recuento correcto fue de 90.

En otro momento apareció un resultado de 5 compradores.

Se revisaron las medidas, los filtros y las relaciones del modelo. Al reconstruir o actualizar las visualizaciones se recuperaron los resultados esperados, pero no se identificó con certeza la causa de todas las discrepancias.

### 5. Diferencias en el recuento de productos vendidos

La medida original:

```dax
Productos vendidos =
DISTINCTCOUNT('Pedidos (2)'[Pedidos.Producto_ID])
```

Llegó a mostrar 59 productos vendidos, aunque la columna contenía 74 identificadores distintos.

Para comprobarlo creé una medida auxiliar:

```dax
Productos prueba =
CALCULATE(
    DISTINCTCOUNT('Pedidos (2)'[Pedidos.Producto_ID]),
    REMOVEFILTERS()
)
```

Esta medida devolvió los 74 productos esperados.

**Limitación:** `REMOVEFILTERS()` elimina el contexto de filtros, por lo que esta medida no se comporta igual que la original ante los segmentadores.

La discrepancia original quedó pendiente de investigar. La medida auxiliar se utilizó para comprobar el total sin filtros.

### 6. Problemas con la relación temporal

Esta fue una de las incidencias más importantes.

Al crear el gráfico de ventas por año y mes, Power BI mostraba únicamente la categoría `(En blanco)`.

Aunque la relación entre `Calendario` y `Pedidos (2)` estaba activa y aparentemente bien configurada, los registros no encontraban fechas coincidentes.

Para diagnosticarlo creé una medida auxiliar:

```dax
Fechas sin coincidencia =
COUNTROWS(
    FILTER(
        'Pedidos (2)',
        ISBLANK(RELATED(Calendario[Date]))
    )
)
```

Inicialmente devolvía aproximadamente 23.000 registros sin correspondencia.

**Solución efectiva:** acceder a Power Query y convertir directamente `Pedidos.Fecha` al tipo **Fecha**, eliminando la componente horaria antes de cargar los datos al modelo.

Después de aplicar esta transformación, el gráfico temporal comenzó a funcionar correctamente y dejaron de aparecer las fechas sin coincidencia.

### 7. Parámetros de campo y fechas

Durante la configuración de los parámetros dinámicos, la dimensión de fechas no funcionó como esperaba.

Para continuar con el ejercicio decidí retirarla del parámetro.

Esta incidencia quedó pendiente de revisar.

## 📈 Resultados y conclusiones

El informe permite consultar la actividad comercial del periodo analizado y explorar sus resultados desde diferentes perspectivas.

En la vista general, sin filtros de selección aplicados, los indicadores mostraban:

| Indicador             |                  Resultado |
| --------------------- | -------------------------: |
| Cantidad vendida      |                     75.784 |
| Ventas totales        |              8,70 millones |
| Coste total           |              6,39 millones |
| Beneficio             | Aproximadamente 2 millones |
| % Beneficio           |                    36,15 % |
| Clientes registrados  |                         92 |
| Clientes compradores  |                         90 |
| Productos registrados |                         78 |
| Productos vendidos    |                         74 |

Estos valores corresponden al estado del informe mostrado durante el desarrollo y deben interpretarse según las medidas utilizadas.

### Evolución temporal

El gráfico de ventas por año muestra una evolución positiva hasta 2020, seguida de un descenso en 2021 y una cifra inferior en 2022.

Sin embargo, **los datos de 2022 no abarcan el año completo**, por lo que no sería correcto comparar directamente ese resultado con los años anteriores sin tener en cuenta la diferencia de periodos.

### Aprendizajes principales

Este proyecto me ha permitido practicar:

- La combinación de archivos Excel con una estructura común.
- La construcción de un modelo de datos con relaciones entre tablas.
- La creación y reutilización de medidas DAX.
- El uso de tablas Calendario para análisis temporales.
- La identificación de errores relacionados con filtros, medidas y fechas.
- La creación de dashboards, matrices y visualizaciones jerárquicas.
- El uso de parámetros de campo y marcadores para mejorar la interacción con el informe.

Uno de los aprendizajes más importantes ha sido comprobar que **una fórmula correcta no garantiza por sí sola un resultado correcto**. También es necesario revisar el contexto de filtros, las relaciones y las transformaciones aplicadas a los datos.

## 📂 Estructura del repositorio

```text
powerbi-analisis-pedidos/
│
├── README.md
├── Reto_02_Analisis_de_Pedidos.pbix
├── Images/
│   ├── Analisis.png
│   ├── Dashboard.png
│   ├── Detalle.png
│   ├── Reporting.png
│   └── Tooltip.png
└── IntroPowerBI-CasoPractico_Datos/
    ├── Clientes.xlsx
    ├── Productos.xlsx
    ├── Vendedores.xlsx
    └── Pedidos/
        ├── Pedidos2018.xlsx
        ├── Pedidos2019.xlsx
        ├── Pedidos2020.xlsx
        ├── Pedidos2021.xlsx
        └── Pedidos2022.xlsx
```

El archivo `.pbix` contiene el informe completo de Power BI. La carpeta `Images` reúne las capturas de sus cinco páginas y `IntroPowerBI-CasoPractico_Datos` conserva los archivos Excel originales utilizados en el análisis.

## 🚀 Cómo abrir el proyecto

1. Descargar o clonar el repositorio.
2. Abrir el archivo `.pbix` con Power BI Desktop.
3. Comprobar las rutas de origen de los archivos Excel en Power Query.
4. Si es necesario, actualizar las rutas para que apunten a la carpeta local donde se han descargado los datos.
5. Aplicar los cambios y actualizar el modelo.
6. Explorar las páginas y utilizar los segmentadores, jerarquías y parámetros disponibles.

Para utilizar el informe es necesario disponer de **Power BI Desktop**.

## 🔄 Próximos pasos

Aunque el informe ya incluye las principales funcionalidades del ejercicio, hay algunas mejoras que podría incorporar:

- Investigar definitivamente las diferencias observadas en las medidas de clientes y productos.
- Revisar el comportamiento de los parámetros de campo con las fechas.
- Validar los cálculos de ventas, costes y beneficios a partir de las columnas originales.
- Mejorar la presentación de las matrices con muchas categorías y niveles jerárquicos.
- Incorporar comparaciones temporales entre periodos equivalentes.
- Revisar las rutas de origen para facilitar la reutilización del proyecto en otros equipos.

## 🤝 Contribuciones

Las sugerencias de mejora son bienvenidas. Si encuentras algún problema o tienes alguna propuesta relacionada con el modelo, las medidas o las visualizaciones, puedes abrir una *issue* en el repositorio.

## 🤖 Uso de inteligencia artificial

Durante el desarrollo del proyecto utilicé ChatGPT como herramienta de apoyo para comprender los errores que surgieron en Power BI, identificar sus posibles causas y encontrar soluciones. Su uso se centró en resolver dudas técnicas y entender mejor el funcionamiento de las herramientas, mientras realizaba personalmente el ejercicio y aplicaba las correcciones.

## ✒️ Autoría y contexto

**Angelo Herrera**

GitHub: [@aherrera180](https://github.com/aherrera180)

Proyecto práctico de aprendizaje realizado durante el módulo de Power BI de **ThePower**. Las transformaciones y las incidencias descritas documentan el trabajo realizado durante el reto guiado.

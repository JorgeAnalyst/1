# Proyecto 1: Reporte de ventas utilizando SQL y Power BI

Descripción del proyecto:
- Utilicé una base de datos preexistente de Microsoft (AdventureWorks Sample Databases)
- Simulé una solicitud comercial con la finalidad de generar un informe de ventas ejecutivo. Definí las historias de usuarios (user stories) para cumplir con los - requerimientos y garantizar que los criterios de aceptación se mantuviesen durante todo el proyecto.
- Creé el modelo de datos necesario para realizar análisis y satisfacer las necesidades comerciales definidas en las user stories.
- Limpié y preparé los datos necesarios.
- Finalmente, confeccioné el dashboard de ventas en Power BI con visualizaciones claras para mostrar las ventas a lo largo del tiempo, por clientes y por productos.

# Recursos utilizados
- Base de datos: https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2019.bak
- Power BI Desktop 
- Microsoft SQL Server Management Studio


# Paso 1: Requerimientos del negocio

En primer lugar recopilo y estandarizo la solicitud recibida por el gerente de ventas y defino exactamente la información que necesita cada personal dentro del área. 

Luego, en base a la solicitud que se hizo desde la empresa, definí las historias de usuarios para lograr un marco bien definido y así cumplir con la entrega y garantizar que los criterios de aceptación se mantengan durante todo el proyecto.

| N#             | Cargo                       | Requerimiento                                                      | Objetivo                                                              | Criterio de Aceptación                                      |
|     :---:      |     :---:                   |     :---:                                                          |     :---:                                                             |     :---:                                                   |
| 1              | Gerente de ventas           | Dashboard global de ventas                                       | Hacer un mejor seguimiento de los clientes y productos más rentables| Dashboard en Power BI que actualice los datos una vez al día|
| 2              | Ejecutivo de ventas 1       | Descripción detallada de ventas por cliente                        | Hacer seguimiento a los clientes que compran más                      | Dashboard en Power BI que permita filtrar datos por cliente | 
| 3              | Ejecutivo de ventas 2       | Descripción detallada de ventas por producto                       | Hacer seguimiento a los productos que más se venden| Dashboard en Power BI que permita filtrar datos por producto|
| 4              | Ejecutivo de ventas 3       | Descripción detalllada de ventas por internet                      | Hacer seguimiento de las ventas y presupuesto                         | Dashboard en Power BI con gráficos, KPI y presupuesto |


# Paso 2: Limpieza y transformación de datos (SQL)

Para crear el modelo de datos necesario para realizar análisis y satisfacer las necesidades comerciales definidas en los requerimientos de trabajo, importé las tablas mediante SQL.

Además, armé un presupuesto de ventas en Excel y la conecté en el modelo de datos.

A continuación se muestran las instrucciones SQL para limpiar y transformar los datos necesarios.

imagen


# Paso 3: Modelamiento de datos

A continuación, se muestra una captura de pantalla del modelo de datos después de que las tablas fuesen limpiadas y preparadas. Se implementaron en Power BI.

Este modelo de datos también muestra cómo se ha conectado FACT_Budget a FACT_InternetSales y otras tablas DIM necesarias.

imagen


# Paso 4: Dashboard de ventas
Finalmente, en la siguiente imagen evidencio el resultado final: 

Un dashboard de ventas completo con una página que funciona como panel y una descripción general, con otras dos páginas enfocadas en combinar tablas para obtener los detalles y visualizaciones necesarias para mostrar las ventas a lo largo del tiempo, por clientes y por productos.

imagen



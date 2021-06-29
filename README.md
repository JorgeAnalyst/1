<h2 align="center">PROYECTO 1: REPORTE DE VENTAS UTILIZANDO SQL Y POWER BI</h2>


#### Descripción del proyecto:
- Utilicé una base de datos preexistente de Microsoft (AdventureWorks Sample Databases)
- Simulé una solicitud comercial con la finalidad de generar un informe de ventas ejecutivo. Definí las historias de usuarios (user stories) para cumplir con los - requerimientos y garantizar que los criterios de aceptación se mantuviesen durante todo el proyecto.
- Creé el modelo de datos necesario para realizar análisis y satisfacer las necesidades comerciales definidas en las user stories.
- Limpié y preparé los datos necesarios.
- Finalmente, confeccioné el dashboard de ventas en Power BI con visualizaciones claras para mostrar las ventas a lo largo del tiempo, por clientes y por productos.

#### Recursos utilizados
- Base de datos: https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2019.bak
- Power BI Desktop 
- Microsoft SQL Server Management Studio


<h2 align="center">Paso 1: Requerimientos del negocio</h2>


En primer lugar recopilo y estandarizo la solicitud recibida por el gerente de ventas y defino exactamente la información que necesita cada personal dentro del área. 

Luego, en base a la solicitud que se hizo desde la empresa, definí las historias de usuarios para lograr un marco bien definido y así cumplir con la entrega y garantizar que los criterios de aceptación se mantengan durante todo el proyecto.

| N#             | Cargo                       | Requerimiento                                                      | Objetivo                                                              | Criterio de Aceptación                                      |
|     :---:      |     :---:                   |     :---:                                                          |     :---:                                                             |     :---:                                                   |
| 1              | Gerente de ventas           | Dashboard global de ventas                                       | Hacer un mejor seguimiento de los clientes y productos más rentables| Dashboard en Power BI con los indicadores más importantes del el área|
| 2              | Ejecutivo de ventas 1       | Descripción detallada de ventas por cliente                        | Hacer seguimiento a los clientes que compran más                      | Dashboard en Power BI que permita filtrar datos por cliente | 
| 3              | Ejecutivo de ventas 2       | Descripción detallada de ventas por producto                       | Hacer seguimiento a los productos que más se venden| Dashboard en Power BI que permita filtrar datos por producto|
| 4              | Ejecutivo de ventas 3       | Descripción detalllada de ventas por internet                      | Hacer seguimiento de las ventas y costos                         | Dashboard en Power BI con gráficos interactivos e indicadores |



<h2 align="center">Paso 2: Limpieza y transformación de datos en SQL</h2>
Para realizar el análisis y satisfacer las necesidades comerciales definidas en los requerimientos de trabajo, primero importé las tablas mediante SQL las cuales limpié, ordené, y transforme en formato.csv, esto con la finalidad de adquirir sólo los datos que efectivamente necesito para cumplir el objetivo de una forma limpia y ordenada.


### Fechas

```
-- Resultado tabla DIM Fechas trabajada -- 
SELECT 
  [DateKey], 
  [FullDateAlternateKey] AS Fecha, 
  --,[DayNumberOfWeek] 
  --,[EnglishDayNameOfWeek]
  [SpanishDayNameOfWeek] AS Dia, 
  --,[FrenchDayNameOfWeek]
  --,[DayNumberOfMonth]
  --,[DayNumberOfYear]
  [WeekNumberOfYear] AS NrSemana, 
  --,[EnglishMonthName]
  [SpanishMonthName] AS Mes, 
LEFT([SpanishMonthName], 3) as MesCorto,  
  --,[FrenchMonthName]  
  [MonthNumberOfYear] AS NrMes, 
  --,[CalendarQuarter]
  [CalendarYear] AS Año, 
  [CalendarSemester] AS Semestre 
  --,[FiscalQuarter]
  --,[FiscalYear]
  --,[FiscalSemester]
FROM 
  [AdventureWorksDW2019].[dbo].[DimDate] 
WHERE 
  CalendarYear >= 2019  

```

### Clientes

```
-- Resultado tabla DIM Clientes trabajada --
SELECT 
  c.customerkey AS CustomerKey, 
  --,[GeographyKey]
  --,[CustomerAlternateKey]
  --,[Title]
  c.firstname AS Nombre, 
  --,[MiddleName]
  c.lastname AS Apellido, 
  c.firstname + ' ' + lastname AS [Nombre Completo], 
  --,[NameStyle]
  --,[BirthDate]
  --,[MaritalStatus]
  --,[Suffix]
CASE c.gender WHEN 'M' THEN 'Hombre' WHEN 'F' THEN 'Mujer' END AS Género, 
  --,[Gender]
  --,[EmailAddress]
  --,[YearlyIncome]
  --,[TotalChildren]
  --,[NumberChildrenAtHome]
  --,[EnglishEducation]
  [SpanishEducation] AS Educación, 
  --,[FrenchEducation]
  --,[EnglishOccupation]
  [SpanishOccupation] AS Ocupación, 
  --,[FrenchOccupation]
  --,[HouseOwnerFlag]
  --,[NumberCarsOwned]
  --,[AddressLine1]
  --,[AddressLine2]
  --,[Phone]
  c.DateFirstPurchase AS PrimeraCompra, 
  --,[CommuteDistance]
  g.city AS Ciudad 
FROM 
  [AdventureWorksDW2019].[dbo].[DimCustomer] as c 
LEFT JOIN dbo.dimgeography AS g ON g.geographykey = c.geographykey 
ORDER BY 
  CustomerKey ASC  


```

### Productos
```
-- Resultado tabla DIM Productos trabajada --
SELECT 
  p.[ProductKey], 
  p.[ProductAlternateKey] AS Código, 
  --,[ProductSubcategoryKey],
  --,[WeightUnitMeasureCode]
  --,[SizeUnitMeasureCode]
  --,[EnglishProductName]
  p.[SpanishProductName] AS Nombre, 
  ps.SpanishProductSubcategoryName AS [Sub Categoría], 
  pc.SpanishProductCategoryName AS Categoría,  
  --,[FrenchProductName]
  --,[StandardCost]
  --,[FinishedGoodsFlag]
  p.[Color] AS Color, 
  --,[SafetyStockLevel]
  --,[ReorderPoint]
  --,[ListPrice]
  p.[Size] AS Tamaño, 
  --,[SizeRange]
  --,[Weight]
  --,[DaysToManufacture]
  p.[ProductLine] AS [Linea del Producto], 
  --,[DealerPrice]
  --,[Class]
  --,[Style]
  p.[ModelName] AS [Nombre del Modelo], 
  --,[LargePhoto]
  p.[EnglishDescription] AS [Descripcion del Producto], 
  --,[FrenchDescription]
  --,[ChineseDescription]
  --,[ArabicDescription]
  --,[HebrewDescription]
  --,[ThaiDescription]
  --,[GermanDescription]
  --,[JapaneseDescription]
  --,[TurkishDescription]
  --,[StartDate]
  --,[EndDate]
  ISNULL (p.Status, 'Agotado') AS [Estado Actual] 
FROM 
  [AdventureWorksDW2019].[dbo].[DimProduct] AS p 
  LEFT JOIN dbo.DimProductSubcategory AS ps ON ps.ProductSubcategoryKey = p.ProductSubcategoryKey 
  LEFT JOIN dbo.DimProductCategory AS pc ON ps.ProductCategoryKey = pc.ProductCategoryKey 
order by 
  p.ProductKey asc

```

### Ventas
```
-- Resultado tabla FACT Ventas trabajada --
SELECT 
  [ProductKey], 
  [OrderDateKey], 
  [DueDateKey], 
  [ShipDateKey], 
  [CustomerKey], 
  --,[PromotionKey]
  --,[CurrencyKey]
  --,[SalesTerritoryKey]
  [SalesOrderNumber], 
  --,[SalesOrderLineNumber]
  --,[RevisionNumber]
  --,[OrderQuantity]
  [UnitPrice], 
  --,[ExtendedAmount]
  --,[UnitPriceDiscountPct]
  --,[DiscountAmount]
  --,[ProductStandardCost]
  --,[TotalProductCost]
  [SalesAmount] 
  --,[TaxAmt]
  --,[Freight]
  --,[CarrierTrackingNumber]
  --,[CustomerPONumber]
  --,[OrderDate]
  --,[DueDate]
  --,[ShipDate]
FROM 
  [AdventureWorksDW2019].[dbo].[FactInternetSales] 
WHERE 
  LEFT (OrderDateKey, 4) >= YEAR(GETDATE()) -2
ORDER BY 
  OrderDateKey ASC

```




<h2 align="center">Paso 3: Modelamiento de datos</h2>
En esta fase, cargé los datos a Power BI y elaboré un modelo después de que las tablas fuesen limpiadas y preparadas. 

Este modelo de datos muestra cómo se ha conectado la tabla Fecha al presupuesto, y las tablas DIM a la tabla Ventas por Internet.

![](/imagen/Modelo.JPG)



<h2 align="center">Paso 4: Dashboard de ventas</h2>


Finalmente, en las siguientes imágenes se evidencia el resultado final, un dashboard de ventas completo con una página que funciona como panel y una descripción general, además de una hoja con información de los clientes enfocadas en obtener los detalles y visualizaciones necesarias para mostrar las ventas a lo largo del tiempo, por clientes y por productos. 

![](/imagen/Ventas1.JPG)

![](/imagen/Clientes3.JPG)

*Si estás interesado en conocer e interactuar con el dashboard, te invito a hacer click en el siguiente enlace:*
https://app.powerbi.com/groups/me/reports/0468830c-240c-406c-96e4-6cc4051261d9?ctid=6a6202fc-b14f-49a0-97e8-9f47d94fca85&pbi_source=linkShare


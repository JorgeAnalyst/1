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
| 1              | Gerente de ventas           | Dashboard global de ventas                                       | Hacer un mejor seguimiento de los clientes y productos más rentables| Dashboard en Power BI que actualice los datos una vez al día|
| 2              | Ejecutivo de ventas 1       | Descripción detallada de ventas por cliente                        | Hacer seguimiento a los clientes que compran más                      | Dashboard en Power BI que permita filtrar datos por cliente | 
| 3              | Ejecutivo de ventas 2       | Descripción detallada de ventas por producto                       | Hacer seguimiento a los productos que más se venden| Dashboard en Power BI que permita filtrar datos por producto|
| 4              | Ejecutivo de ventas 3       | Descripción detalllada de ventas por internet                      | Hacer seguimiento de las ventas y presupuesto                         | Dashboard en Power BI con gráficos, KPI's y presupuesto |



<h2 align="center">Paso 2: Limpieza y transformación de datos en SQL</h2>
Para realizar el análisis y satisfacer las necesidades comerciales definidas en los requerimientos de trabajo, primero importé las tablas mediante SQL las cuales limpié, ordené, y transforme en formato.csv, esto con la finalidad de adquirir sólo los datos que efectivamente necesito para cumplir el objetivo de una forma limpia y ordenada.

A continuación se muestra el resultado final (antes y después del procedimiento) de las queries en SQL: 

### Ejemplo

```
-- Ejemplo: Tabla DIM Fecha requerida, sin modificar --
SELECT TOP (1000) [DateKey]
      ,[FullDateAlternateKey]
      ,[DayNumberOfWeek]
      ,[EnglishDayNameOfWeek]
      ,[SpanishDayNameOfWeek]
      ,[FrenchDayNameOfWeek]
      ,[DayNumberOfMonth]
      ,[DayNumberOfYear]
      ,[WeekNumberOfYear]
      ,[EnglishMonthName]
      ,[SpanishMonthName]
      ,[FrenchMonthName]
      ,[MonthNumberOfYear]
      ,[CalendarQuarter]
      ,[CalendarYear]
      ,[CalendarSemester]
      ,[FiscalQuarter]
      ,[FiscalYear]
      ,[FiscalSemester]
  FROM [AdventureWorksDW2019].[dbo].[DimDate]
```


## Resultados (Datos filtrados, limpiados, y ordenados): 

### Fecha

```
-- Resultado tabla DIM Fecha limpiada -- 
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
LEFT([SpanishMonthName], 3) as MesCorto,  --> Acorté las tres primeras letras el nombre del mes para que al visualizarse se vea más representativo y limpio
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
  CalendarYear >= 2019  --> Elegí sólo los años 2019 en adelante, ya que la base de datos contenía datos demasiado antiguos

```

### Clientes

```
-- Resultado tabla DIM Clientes limpiada --
SELECT 
  c.customerkey AS CustomerKey, 
  --,[GeographyKey]
  --,[CustomerAlternateKey]
  --,[Title]
  c.firstname AS Nombre, 
  --,[MiddleName]
  c.lastname AS Apellido, 
  c.firstname + ' ' + lastname AS [NombreCompleto], --> Concatené Nombre y Apellido
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
  g.city AS Ciudad  --> Vinculé la Ciudad del cliente con la tabla DimGeography
FROM 
  [AdventureWorksDW2019].[dbo].[DimCustomer] as c 
  LEFT JOIN dbo.dimgeography AS g ON g.geographykey = c.geographykey 
ORDER BY 
  CustomerKey ASC  --> Ordené la lista de forma ascendente


```

### Producto
```
-- Resultado tabla DIM Producto limpiada --
SELECT 
  p.[ProductKey], 
  p.[ProductAlternateKey] AS Código, 
  --,[ProductSubcategoryKey],
  --,[WeightUnitMeasureCode]
  --,[SizeUnitMeasureCode]
  --,[EnglishProductName]
  p.[SpanishProductName] AS Nombre, 
  ps.SpanishProductSubcategoryName AS [Sub Categoría],  -->Se unió desde la tabla de Sub Categoría
  pc.SpanishProductCategoryName AS Categoría,  -->Se unió dese la tabla de Categoría
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
  ISNULL (p.Status, 'Agotado') AS [Estado Actual] --> Se designó el estado 'Agotado´ Si no se encuentra disponible
FROM 
  [AdventureWorksDW2019].[dbo].[DimProduct] AS p 
  LEFT JOIN dbo.DimProductSubcategory AS ps ON ps.ProductSubcategoryKey = p.ProductSubcategoryKey 
  LEFT JOIN dbo.DimProductCategory AS pc ON ps.ProductCategoryKey = pc.ProductCategoryKey 
order by 
  p.ProductKey asc

```

### Ventas por Internet 
```
-- Resultado tabla FACT Ventas por Internet limpiada --
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
  [SalesAmount] --,[TaxAmt]
  --,[Freight]
  --,[CarrierTrackingNumber]
  --,[CustomerPONumber]
  --,[OrderDate]
  --,[DueDate]
  --,[ShipDate]
FROM 
  [AdventureWorksDW2019].[dbo].[FactInternetSales] 
WHERE 
  LEFT (OrderDateKey, 4) >= YEAR(GETDATE()) -2 --> Se asegura que siempre se obtenga información de los últimos dos años a contar del año actual
ORDER BY 
  OrderDateKey ASC

```




<h2 align="center">Paso 3: Modelamiento de datos</h2>
En esta fase, cargé los datos a Power BI y creé un modelodespués de que las tablas fuesen limpiadas y preparadas. 

Este modelo de datos también muestra cómo se ha conectado el presupuesto junto a las tablas DIM a la tabla FACT (Ventas por Internet).

![](/imagen/Modelo Tablas Power BI.PNG)



<h2 align="center">Paso 4: Dashboard de ventas</h2>
Finalmente, en la siguiente imagen evidencio el resultado final: 

Un dashboard de ventas completo con una página que funciona como panel y una descripción general, con otras dos páginas enfocadas en combinar tablas para obtener los detalles y visualizaciones necesarias para mostrar las ventas a lo largo del tiempo, por clientes y por productos.

imagen



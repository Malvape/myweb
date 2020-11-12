---
title: "Microsoft SQL Server"
date: 2020-11-10T15:21:30-05:00
categories:
  - SQL
tags:
  - MicrosoftSQL
  - AWS
toc: true
toc_label: "En esta página"
related: true
---

Hace unos días rendí un examen de SQL en SQL Server y algunas cosas me resultaron interesantes.
<!--more-->

## Data

Conseguí un repositorio en github, de un peruano que recopila data del covid 19.
Acá dejo el link : [Data-peru-covid][jmcastagnetto].

De ese repositorio elegí estos archivos csv porque son los que estaban mejor documentados:

- covid-peru-fallecimientos [Fallecidos][fallecidos]
![Muestra la tabla Fallecidos](/myweb/assets/images/SQLAWS/falle.png)
- hospitalizados [Hospitalizados][hospitalizados]
![Muestra la tabla hospitalizados](/myweb/assets/images/SQLAWS/hosp.png)
- dataCovid [Covid-19-Peru][general]
![Muestra la tabla data covid](/myweb/assets/images/SQLAWS/datacov.png)

### Limpiar data

#### Valores 'NA'
En algunas columnas de los archivos se muestra el valor 'NA' para indicar NULL (No se registra valor), pero SQL toma este valor como un STRING
por lo que es necesario editar todas las celdas que contengan estos valores, esto se hace asi:

```sql
UPDATE dataCovid
  SET region = NULL
  WHERE region ='NA'

  UPDATE dataCovid
  SET deaths = NULL
  WHERE deaths ='NA'

  UPDATE dataCovid
  SET recovered = NULL
  WHERE recovered ='NA'

  UPDATE dataCovid
  SET total_tests = NULL
  WHERE total_tests ='NA'

  UPDATE dataCovid
  SET negative_tests = NULL
  WHERE negative_tests ='NA'

  UPDATE dataCovid
  SET pcr_test_positive = NULL
  WHERE pcr_test_positive ='NA'

  UPDATE dataCovid
  SET pcr_serological_test_positive = NULL
  WHERE pcr_serological_test_positive ='NA'

  UPDATE dataCovid
  SET serological_test_positive = NULL
  WHERE serological_test_positive ='NA'
```

Tambien sucede este problema en la tabla "hospitalizados", se resuelve de la misma manera.

#### Columnas con tipo de valor incorrecto

Al importar los valores a SQL uno elige el tipo de valor de cada columna y si es que
va a permitir valor `NULL` o no, en caso no elegir el tipo correcto, se puede solucionar desde
la opcion Design.

 ![Design](/myweb/assets/images/SQLAWS/design.png)

A todos los valores numericos los hice tipo int, fechas datetime y las columnas que contengan nulls
tienen que tener la opcion `Allow Nulls`

![Allow Nulls](/myweb/assets/images/SQLAWS/tabla.png)

## Ejercicios

### Aumento de confirmados

Quise probar que el dato de confirmados es acumulado, este codigo entrega solo los confirmados por fecha en `Lima`

``` sql
  SELECT *
  FROM
  dataCovid
  WHERE region = 'Lima'
```
Output:
![Confirmados en Lima](/myweb/assets/images/SQLAWS/confir.png)

Aparentementi si es un valor acumulado, para agregarle una columna con la diferencia con el dia anterior:

Primero tengo que agrupar la informacion por fecha, e insertarla en una nueva tabla.

``` sql

CREATE TABLE [dbo].[Confirma2](
	[date] [datetime2](7) NOT NULL,
	[confirmed] [int] NULL,
) ON [PRIMARY]
GO


INSERT INTO Confirma2
SELECT date, SUM(dataCovid.confirmed)
FROM dataCovid
GROUP BY dataCovid.date
Order by dataCovid.date  FROM
  dataCovid
  WHERE region = 'Lima'

```
La nueva tabla no se rellena de acuerdo al order by, es por eso que se tiene que hacer una
columna que sirva de indice, y que este de acuerdo al orden de las fechas.

``` sql
CREATE CLUSTERED INDEX ID ON dbo.Confirma2
(date ASC)

ALTER TABLE Confirma2
ADD ID INT IDENTITY(1, 1)
```

Finalmente para agregarle una columna que entregue el aumento de confirmados respecto al
dia anterior:

``` sql
select *,
       confirmed - coalesce(lag(confirmed) over (order by ID), 0) as diff
from Confirma2
```
Output:
![Output](/myweb/assets/images/SQLAWS/aea.png)

### Fallecimientos

Quise encontrar la cantidad de hombres y mujeres fallecidos por provincia para poder compararlos.

Es importante mencionar que la informacion de esta tabla sólo contiene información de marzo y abril.
{: .notice}


``` sql
SELECT dbo.[covid-peru-fallecimientos].región,dbo.[covid-peru-fallecimientos].sexo, COUNT(*) AS Cantidad
FROM dbo.[covid-peru-fallecimientos]
GROUP BY dbo.[covid-peru-fallecimientos].región, dbo.[covid-peru-fallecimientos].sexo
```
Output:
![Intento](/myweb/assets/images/SQLAWS/pip.png)

Pero esto no nos deja compararlos, necesito tener la cuenta de hombres en una columna y la de mujeres en la otra.

``` sql
SELECT dbo.[covid-peru-fallecimientos].región,
   COUNT( CASE WHEN sexo='masculino' then 1 END) AS NroHombres,
   COUNT( CASE WHEN sexo='femenino' then 1 END) AS NroMujeres
FROM dbo.[covid-peru-fallecimientos]
GROUP BY dbo.[covid-peru-fallecimientos].región
```
Resultado:

![Intento](/myweb/assets/images/SQLAWS/uwu.png)



[jmcastagnetto]: https://github.com/jmcastagnetto/covid-19-peru-data
[fallecidos]:{{ site.url }}{{ site.baseurl }}/assets/downloads/covid-19-peru-fallecimientos.csv
[hospitalizados]:{{ site.url }}{{ site.baseurl }}/assets/downloads/covid-19-peru-hospitalizados.csv
[general]:{{ site.url }}{{ site.baseurl }}/assets/downloads/covid-19-peru-data.csv

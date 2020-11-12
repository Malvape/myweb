---
title: "Cómo conectar una base de datos AWS RDS a SQL server"
date: 2020-11-11T15:21:30-05:00
categories:
  - SQL
tags:
  - MicrosoftSQL
  - AWS
toc: true
toc_label: "En esta página"
related: true
---

Tener tu base de datos en un servicio en la nube tiene sus ventajas, aca muestro como conectar
tu base de datos de Amazon Web Services (gratis), con Microsoft SQL Server
<!--more-->

## Crear una Base de datos en RDS en Amazon

Vamos a utilizar el servicio gratuito de base de datos de [Amazon][Amazon], primero hay que
crearse una cuenta. Una vez creada la cuenta, buscar el servicio de RDS.

 ![AWS](/myweb/assets/images/SQLAWS/AWS.png)

Luego dirigirse a la Opcion Databases

 ![123](/myweb/assets/images/SQLAWS/123.png)

 Y clickear "Create a Database".  Eliges la opcion gratuita (tiene menos capacidad
   que una pagada, y menos CPUs disponibles), te va a pedir crear una **clave** para poder ingresar
   a la base de datos, el usuario siempre es **admin**.

## Crdenciales de la base de datos
Una vez creada la base de datos, buscar la opcion **Connectivity & security** y
fijarse en estos datos:

   -Endpoint
   -port (casi siempre es 1433)
   -Public accessibility (viene por defecto en **NO**, pero se debe cambiar a **Yes**)

Debe quedar algo asi:

![ert](/myweb/assets/images/SQLAWS/ert.png)

## conectarlo con SQL Server
Luego Entra a **Microsoft SQL Server**. Las opciones de la ventana inicial deben ser:

-Authentication: SQL Server Authentication
-Server name: (aca debe ir el endpoint seguido de **,1433**)

    Ejemplo:  database-1.#############.us-west-2.rds.amazonaws.com,1433
    {: .notice}

 -El usuario y contraseña que utilizó para crear la base de datos.

Finalmente desbloquear el puerto 1433 en las opciones de Firewall.
aca dejo un link: [Como desbloquear el puerto 1433][1433]

## Ventajas de tener una base de datos en la nube

-Ahorro en Costos (es relativo pero a mediana y grande escala es rentable)
-Seguridad (La seguridad se terceriza a una empresa enorme)
-Flexibilidad (la capacidad de la base puede cambiar facilmente)
-Facilita la colaboración (No se conecta por cables)

Estas son solo algunas, para una lista mas completa:

- [12 beneficios del cloud computing](https://www.salesforce.com/products/platform/best-practices/benefits-of-cloud-computing/)


[Amazon]: https://aws.amazon.com/es/
[1433]: https://www.youtube.com/watch?v=T3tLcuVNtUQ

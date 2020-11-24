---
title: "El poder de la información"
date: 2020-11-23T15:34:30-05:00
categories:
  - Python
tags:
  - GitHub
  - Política
  - Barplot
toc: true
toc_label: "En esta página"
related: true
header:
  teaser: /assets/images/Golpistas/golpistas.png

---


Hace una semanaHace una semana se vacó al presidente del Perú, cosa que no agradó a nadie, especialmente a los jóvenes. En respuesta a esto se dio un esfuerzo para que, utilizando herramientas de programación, visualización de información y diseño  se pueda aportar a la situación.

<!--more-->

## De qué se trata?

Se hizo viral la iniciativa de [éste](https://twitter.com/RoTorresT) joven `twitero`, que sugería hacer una página web con información publica extraída de los canales del estado (Principalmente web), consolidarla y presentarla en una página web, de manera que se pueda entender fácilmente que sucedió y quienes son los responsables.

Es asi que nace:

- [https://golpistas.pe/][Web]

 ![Golpistas](/myweb/assets/images/Golpistas/golpistas.png)

## Quienes colaboran?

Se Hizo un grupo de trabajo en [Slack](https://slack.com/intl/es-pe/), cosa que ahí se pueden comunicar más fácilmente quienes quieran apoyar, PERO también se hizo un repositorio en GitHub (Plataforma que utilizo para mantener esta página web) que es una plataforma diseñada para facilitar el trabajo colaborativo mediante Git. Este es el repositorio:

- [Repositorio en Github][Git]

De manera que cualquiera puede sugerir soluciones y apoyar con su código.

### Equipos

Rápidamente se formaron equipos de gente que pueda aportar con su conocimiento:

- UX/UI
- Data
- Comunicadores
- front-end
- Back-end
- redes (seo)

Yo me uní al equipo de Data.

## Mi aporte

En el repositorio, los administradores publican que cosas son las que necesitan trabajarse aún.
Elegí esta tarea:

- Agregar grafica de barras agrupando por tipo de delito sobre toda la poblacion del congreso

### Database

El dataset que utilicé fue este:

- [Investigaciones-limpio][dataset]

Decidí trabajarlo en Python, porque no he realizado un post en Python en este blogg, y porque los otros
colaboradores estaban utilizando principalmente esta herramienta.

### Solución

Primero importé los paquetes necesarios:

``` python
import pandas as pd
import plotly.express as px
import plotly.graph_objects as go
import numpy as np
import matplotlib.pyplot as plt

```
- Luego visualizar las columnas de la tabla:

``` python
df = pd.read_csv ('data/investigaciones-limpio.csv')
df.head()

```
![Golpistas](/myweb/assets/images/Golpistas/1ra.png)

- Me di cuenta que la columna `delito` primero indicaba el tipo de delito y luego entre paréntesis indicaba algún detalle.
Tuve que separar esta columna en 2:

``` python
df[['TIPO DE DELITO','DETALLE']]=df['DELITO'].str.split('(',n=1,expand=True)
df.head()

```
![Golpistas](/myweb/assets/images/Golpistas/2da.png)

- al momento de agrupar, encontré que muchos tipos de delitos se repetían, y esto porque los espacios en blanco también cuentan como caracteres y si uno tiene y otro no, no es posible agruparlos, lo solucioné así:

``` python
texto = [texto for texto in df["TIPO DE DELITO"]]
print(texto)

Lista = []

for i in texto:
    i = i.strip()
    Lista.append(i)

for i in (sorted(set(Lista))):
  print(i)

df["TIPO DE DELITO"] = Lista

```
- Habiendo hecho esto, ya era posible agrupar por tipo de delito. Hice esto y lo asigné a un dataset nuevo.

``` python
ConDelito = df.groupby(['APELLIDOS Y NOMBRE','TIPO DE DELITO'])[['TIPO DE DELITO']].count()
ConDelito.head(20)

```
![Golpistas](/myweb/assets/images/Golpistas/3ra.png)

- Habiendo hecho esto, ya solo quedaba hacer la visualización.

``` python
plt.rcParams['figure.figsize'] = (30,6)
ConDelito['TIPO DE DELITO'].unstack(1).plot(kind='bar', stacked=True)
plt.legend(bbox_to_anchor=(0, 1, 1, 0), loc="lower left")

```

<figure>
	<a href="/myweb/assets/images/Golpistas/4ta.png"><img src="/myweb/assets/images/Golpistas/4ta.png"></a>
	</figure>


- Cada color de las barras, representa un tipo de delito distinto, aca esta la leyenda (una parte):


<figure>
	<a href="/myweb/assets/images/Golpistas/5ta.png"><img src="/myweb/assets/images/Golpistas/5ta.png"></a>
	</figure>

[Este](https://colab.research.google.com/drive/14BVmGVGbyjbuncduR6_r2w9bZ84cuUVs) es el link para poder ver mi código en colab.



[dataset]:{{ site.url }}{{ site.baseurl }}/assets/downloads/investigaciones-limpio.csv
[python-project-template]: https://github.com/yxtay/python-project-template
[pip]: https://pip.pypa.io/en/stable
[1]:{{ site.url }}{{ site.baseurl }}/assets/downloads/covid-19-peru-data.csv
[Web]: https://golpistas.pe/
[Git]: https://github.com/GolpistasPe/congreso-golpista-data

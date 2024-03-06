![FastAPI](https://img.shields.io/badge/-FastAPI-333333?style=flat&logo=fastapi)
![Docker](https://img.shields.io/badge/-Docker-333333?style=flat&logo=docker)
![Render](https://img.shields.io/badge/-Render-333333?style=flat&logo=render)
![Pandas](https://img.shields.io/badge/-Pandas-333333?style=flat&logo=pandas)
![Numpy](https://img.shields.io/badge/-Numpy-333333?style=flat&logo=numpy)
![Matplotlib](https://img.shields.io/badge/-Matplotlib-333333?style=flat&logo=matplotlib)
![Seaborn](https://img.shields.io/badge/-Seaborn-333333?style=flat&logo=seaborn)
![Scikit-learn](https://img.shields.io/badge/-Scikitlearn-333333?style=flat&logo=scikitlearn)

<br>

# :construction: Proyecto Integrador 01 steam :construction:

<br>

<p align="center">
<img src="https://github.com/SebitaElGordito/PI_01_steam/blob/main/Images/steam_banner_trabajo.jpg?raw=true" alt="steam banner" width="800" height="350">

<br>

## Índice

* [Contexto](#contexto)

+ [Creación y conexión de la máquina virtual](#hammer-creación-y-conexión-de-la-máquina-virtual-entorno-linux)

  * [Limpieza de la máquina virtual (opcional)](#limpieza-de-la-máquina-virtual-opcional)

* [Clonación del Repositorio](#calling-clonación-del-repositorio-inicio-y-ejecución-de-los-servicios-docker-composeyml)

* [HDFS](#floppy_disk-hdfs---hadoop-distributed-file-system)

* [Hive](#bookmark_tabs-hive)

* [Formatos de Almacenamiento](#file_cabinet-formatos-de-almacenamiento)

* [SQL](#computer-sql)

+ [No-SQL](#card_file_box-no-sql)
  * [H-base](#h-base)

  * [Mondodb](#mongodb)

  * [Neo4j](#neo4j)

+ [Spark](#magic_wand-spark)

  * [Spark y Scala](#spark-y-scala)

  * [Kafka](#kafka)

  * [Comparativa Datasets y Dataframe en Scala](#comparativa-datasets-y-dataframe-en-scala)

* [Carga Incremental con Spark](#hourglass-carga-incremental-con-spark)

* [Herramientas de Orquestación de flujo de datos](#abacus-herramientas-de-orquestación-de-flujo-de-datos)

* [Participantes y compañeros del pair programming](#people_hugging-participantes-y-compañeros-del-pair-programming)

<br>

## :hammer: Contexto

Este proyecto es un sistema de recomendación para la plataforma de videojuegos Steam, siguiendo las prácticas de Machine Learning Operations (MLOps). Incluye todas las etapas necesarias para llevar a cabo un proyecto de ciencia de datos: desde la Extracción, Transformación y Carga (ETL) de los datos, hasta el Análisis Exploratorio de Datos (EDA) y la implementación de un modelo de recomendación basado en aprendizaje automático.

Steam es una de las plataformas más grandes y populares para la distribución de videojuegos. Con millones de usuarios activos y un catálogo extenso, existe una gran oportunidad para desarrollar sistemas de recomendación que mejoren la experiencia del usuario al sugerir juegos de su interés. En este proyecto, aplicamos técnicas de ciencia de datos y aprendizaje automático para crear un modelo que realice estas recomendaciones de manera efectiva.

<br>

## :memo: Datos

Nuestro cliente nos proporciona 3 datasets:

* **steam_games.json** es un dataset que contiene datos relacionados con los juegos, como los título, el desarrollador, el publicador, fecha de lanzamiento, los precios, el género al que pertenecen, las etiquetas, etc.

* **australian_user_reviews.json** es un dataset que contiene identificador de juego, si es recomendado o no por el usuario, los comentarios que los usuarios realizaron sobre los juegos que consumen, etc.  

* **australian_users_items.json** es un dataset que contiene información sobre la cantidad de tiempo que dedican los usuarios a los juegos, cantidad de items que tienen los usuarios en sus bibliotecas


Esta informacion se encuentra detallada en el documento [Diccionario de datos](https://github.com/SebitaElGordito/PI_01_steam/blob/main/Diccionario_de_datos.md).

<br>

## ETL

Se realizó la extracción, transformación y carga (ETL) de los tres conjuntos de datos entregados. Dos de los conjuntos de datos se encontraban anidados, es decir había columnas con diccionarios o listas de diccionarios, por lo que aplicaron distintas estrategias para transformar las claves de esos diccionarios en columnas. Luego se rellenaron algunos nulos de variables necesarias para el proyecto, se borraron columnas con muchos nulos o que no aportaban valor al proyecto, para optimizar el rendimiento de la API y teneniendo en cuenta las limitaciones de almacenamiento del deploy.

Los detalles del ETL se puede ver en: 
+ [ETL steam_games](https://github.com/SebitaElGordito/PI_01_steam/blob/main/ETL/ETL_steam_games.ipynb) 
+ [ETL australian_users_items](https://github.com/SebitaElGordito/PI_01_steam/blob/main/ETL/ETL_users_items.ipynb)  
+ [ETL australian_user_reviews](https://github.com/SebitaElGordito/PI_01_steam/blob/main/ETL/ETL_user_reviews.ipynb).

<br>

## Feature engineering

Uno de los pedidos para este proyecto fue aplicar un análisis de sentimiento a los reviews de los usuarios. Para ello se creó una nueva columna llamada 'sentiment_analysis' que reemplaza a la columna que contiene los reviews donde clasifica los sentimientos de los comentarios con la siguiente escala:

* 0 si es malo,
* 1 si es neutral o está sin review
* 2 si es positivo.

<br>

<p align="center">
<img src="https://github.com/SebitaElGordito/PI_01_steam/blob/main/Images/sentiment_analysis.png?raw=true" alt="imagen de creación de columna sentiment analysis" width="750" height="550">
</p>
<p align="center">
<i>creación de la columna sentiment analysis en jupiter notebook.</i>
</p>

<br>

Para la creación de esta columna, se utilizó la librería nltk que es la abreviatura de Natural Language Toolkit, una biblioteca para Python que se utiliza para trabajar con datos de lenguaje humano y análisis de sentimiento. VADER es una herramienta de análisis de sentimientos basada en un léxico y reglas, especialmente calibrada para entender los sentimientos expresados en las redes sociales. Utiliza un conjunto de palabras etiquetadas según su orientación semántica como positivas o negativas y aplica una serie de reglas gramaticales y sintácticas para estimar la valencia sentimental de un texto.

Por otra parte, y bajo el mismo criterio de optimizar los tiempos de respuesta de las consultas en la API y teniendo en cuenta las limitaciones de almacenamiento en el servicio de nube para deployar la API, se realizaron dataframes auxiliares para cada una de las funciones solicitadas. En el mismo sentido, se guardaron estos dataframes en formato *parquet* que permite una compresión y codificación eficiente de los datos.

<br>

## :bar_chart: EDA

Se realizó el EDA a los tres conjuntos de datos sometidos a ETL con el objetivo de identificar las variables que se pueden utilizar en la creación del modelo de recmendación. Para ello se utilizó la librería Pandas para la manipulación de los datos y las librerías Matplotlib y Seaborn para la visualización.

<br>

<p align="center">
<img src="https://github.com/SebitaElGordito/PI_01_steam/blob/main/Images/grafico_eda.jpg?raw=true" alt="imagen de gráfico en el EDA" width="800" height="400">
</p>
<p align="center">
<i>Gráfico de barras cantidad de juegos lanzados por año.</i>
</p>

<br>

En particular para el modelo de recomendación, se terminó eligiendo construir un dataframe específico con el id del usuario que realizaron reviews y los nombres de los juegos a los cuales se le realizaron comentarios.

El desarrollo de este análisis se encuentra en: 
+ [EDA](https://github.com/SebitaElGordito/PI_01_steam/blob/main/EDA/EDA.ipynb)
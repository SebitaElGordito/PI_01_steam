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
<img src="https://github.com/SebitaElGordito/PI_01_steam/blob/main/Images/steam_banner_trabajo.jpg?raw=true" alt="steam banner" width="900" height="250">

<br>

## Índice

* [Contexto](#hammer-contexto)

* [Datos](#memo-datos)

* [ETL](#etl)

* [Feature engineering](#feature-engineering)

* [EDA](#bar_chart-eda)

* [Modelo de Recomendación](#robot-modelo-de-recomendación)

* [Desarrollo de API](#desarrollo-de-api)

* [Deployment](#deployment)

* [Video](#video)

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
<img src="https://github.com/SebitaElGordito/PI_01_steam/blob/main/Images/sentiment_analysis.png?raw=true" alt="imagen de creación de columna sentiment analysis" width="750" height="500">
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
<img src="https://github.com/SebitaElGordito/PI_01_steam/blob/main/Images/grafico_eda.png?raw=true" alt="imagen de gráfico en el EDA" width="800" height="400">
</p>
<p align="center">
<i>Gráfico de barras cantidad de juegos lanzados por año.</i>
</p>

<br>

En particular para el modelo de recomendación, se terminó eligiendo construir un dataframe específico con el id del usuario que realizaron reviews y los nombres de los juegos a los cuales se le realizaron comentarios.

El desarrollo de este análisis se encuentra en: 
+ [EDA](https://github.com/SebitaElGordito/PI_01_steam/blob/main/EDA/EDA.ipynb)

<br>

## :robot: Modelo de recomendación

Se creaó el modelo de recomendación, que generan una lista de 5 juegos ingresando el id_producto.

El modelo tiene una relación ítem-ítem. Se toma un juego y en base a que tan similar es ese juego con el resto de los juegos se recomiendan similares. Para ello, se aplicaron filtro previos a aplicar la **similitud del coseno**, tales como que los juegos potencialmente recomendados debían compartir al menos una categoría de genero con el item_id ingresado para la consulta. Esto generaba una lista de 10 productos recomendados, luego de aplicar el filtro de género y la similitud del coseno, lista de la cual se seleccionaban los 5 juegos con mayor porcentaje de recomendación por el usuario.

<br>

<p align="center">
<img src="https://github.com/SebitaElGordito/PI_01_steam/blob/main/Images/modelo_recomendacion.png?raw=true" alt="imagen de función modelo de recomendacón" width="700" height="380">
</p>
<p align="center">
<i>Función de modelo de recomendación.</i>
</p>

<br>

La **similitud del coseno** que es una medida comúnmente utilizada para evaluar la similitud entre dos vectores en un espacio multidimensional. En el contexto de sistemas de recomendación y análisis de datos, la similitud del coseno se utiliza para determinar cuán similares son dos conjuntos de datos o elementos, y se calcula utilizando el coseno del ángulo entre los vectores que representan esos datos o elementos.

<br>

## Desarrollo de API

Para el desarrolo de la API se decidió utilizar el framework FastAPI, creando las siguientes funciones:

* **developer**: Esta función recibe como parámetro 'developer', que es la empresa desarrolladora del juego, y devuelve la cantidad de items que desarrolla dicha empresa y el porcentaje de contenido Free por año por sobre el total que desarrolla.

* **user_data**: Esta función tiene por parámentro 'user_id' y devulve la cantidad de dinero gastado por el usuario, el porcentaje de recomendaciones que realizó sobre la cantidad de reviews que se analizan y la cantidad de items que posee en su biblioteca de juegos.

* **user_for_genre**: Esta función recibe como parámetro el género de un videojuego y devuelve el usuario con más horas de juego en el género ingresado, y una lista de acumulación de horas jugadas por año.

* **best_developer_year**: En esta función se ingresa un año como parámetro y devuelve el top 3 de desarrolladoras con mas recomendaciones positivas por usuarios, para ese año dado.

* **developer_review_analysis**: Esta función recibe como parámetro una desarrolladora y devuelve un diccionario con la cantidad de reviews positivas y reviews negativas hechas por los usuarios, para esa desarrolladora.

* **recomendacion_juego**: Esta función recibe como parámetro el id de un juego y devuelve una lista con 5 juegos recomendados similares al ingresado.

<br>

<p align="center">
<img src="https://github.com/SebitaElGordito/PI_01_steam/blob/main/Images/fastapi.png?raw=true" alt="imagen de funciones en fastapi local" width="700" height="380">
</p>
<p align="center">
<i>Funciones en fastAPI local.</i>
</p>

<br>

El desarrollo de las funciones de consultas generales se puede ver en: [main.py](https://github.com/SebitaElGordito/PI_01_steam/blob/main/main.py).

<br>

## Deployment

Para el deploy de la API se decidió crear un nuevo repositorio [repo_render](https://github.com/SebitaElGordito/PI_render). que contara solo con las unciones a consumir en la API, liberandonos de las carpetas o jupiters notebooks innecesarios. Se seleccionó la plataforma Render que es una nube unificada para crear y ejecutar aplicaciones y sitios web, permitiendo el despliegue automático desde GitHub. Se utilizó un Dockerfile cuya imagen es Python 3.10. Esto se hace porque Render usa por defecto Python 3.7, lo que no es compatible con las versiones de las librerías trabajadas en este proyecto, por tal motivo, se optó por deployar el proyecto dentro de este contenedor. Se puede ver el detalle del documento [Dockerfile](https://github.com/SebitaElGordito/PI_render/blob/main/Dockerfile).
- Se generó un servicio nuevo  en `render.com`, conectado al presente repositorio y utilizando Docker como Runtime.
- Finalmente, el servicio queda corriendo en [https://pi-render-tt44.onrender.com/](https://pi-render-tt44.onrender.com/).



## Video

En este [video](https://www.youtube.com/watch?v=MWNqfuX1hUQ) se explica brevemente este proyecto mostrando el funcionamiento de la API en render y en fastApi local.

<br>

* **`Sebastian Vazquez`**..................   [![linkedin](https://img.shields.io/badge/linkedin-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/sebastian-vazquez-67353722b/)




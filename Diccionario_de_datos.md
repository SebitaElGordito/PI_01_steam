# Diccionario de datos para el proyecto

## steam_games.json

Es un dataset que contiene datos relacionados con los juegos en sí, como el género, el desarrollador, el título, los precios, características técnicas, etiquetas, fecha de lanzamiento, etc.  

* **publisher**: Es la empresa publicadora del contenido.  
* **genres**: Es el género del item, es decir, del juego. Esta formado por una lista de uno o mas géneros por registro.  
* **app_name**: Es el nombre del item, es decir, del juego.  
* **title**: Es el título del item.  
* **url**: Es la url del juego.  
* **release_date**: Es la fecha de lanzamiento del item en formato 2018-01-04.  
* **tags**: Es la etiqueta del contenido. Esta formado por una lista de uno o mas etiquetas por registro.
* **discount_price**: Es el precio de descuento del item.  
* **reviews_url**: Es la url donde se encuentra el review de ese juego.  
* **specs**: Son especificaciones de cada item. Es una lista con uno o mas string con las especificaciones.
* **price**: Es el precio del item.  
* **early_access**: Indica el acceso temprano con un True/False.  
* **id**: Es el identificador único del contenido.  
* **developer**: Es el desarrollador del contenido.
* **metascore**: Es el puntaje obtenido en metacritic.


## user_reviews.json  

Es un dataset que contiene los comentarios que los usuarios realizaron sobre los juegos que consumen, además de datos adicionales como si recomiendan o no ese juego, emoticones de gracioso y estadísticas de si el comentario fué útil o no para otros usuarios. También presenta el id del usuario que comenta, con su url del perfil.

* **user_id**: Es un identificador único para el usuario.  
* **user_url**: Es la url del perfil del usuario en streamcommunity.  
+ **reviews**: Contiene una lista de diccionarios. Para cada usuario se tiene uno o mas diccionario con el review. Cada diccionario contiene:  
    * **funny**: Indica si alguien puso emoticón de gracioso al review.  
    * **posted**: Es la fecha de posteo del review en formato Posted April 21, 2011.  
    * **last_edited**: Es la fecha de la última edición.  
    * **item_id**: Es el identificador único del item, es decir, del juego.  
    * **helpful**: Es la estadística donde otros usuarios indican si fue útil la información.  
    * **recommend**: Es un booleano que indica si el usuario recomienda o no el juego.  
    * **review**: Es una sentencia string con los comentarios sobre el juego.  


## user_items.json

Es un dataset que contiene información sobre los juegos que juegan los usuarios, así como el tiempo acumulado que cada usuario jugó a un determinado juego.

* **user_id**: Contiene un identificador único del usuario.   
* **user_url**: Es la url del perfil del usuario  
+ **items**: Contiene una lista de uno o mas diccionarios de los items que consume cada usuario. Cada diccionario tiene las siguientes claves:  
    * **item_id**: Es el identificados del item, es decir, del juego.  
    * **item_name**: Es el nombre del contenido que consume, es decir, del juego.  
    * **playtime_forever**: Es el tiempo acumulado que un usuario jugó a un juego.  
    * **playtime_2weeks**: Es el tiempo acumulado que un usuario jugó a un juego en las últimas dos semanas.  



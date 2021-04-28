# Fake news detection model for early stages of the spread

Proyecto de tesis para el optar al grado de Magíster en Ciencias de la Ingeniería Informática. En este trabajo se propone un modelo de detección de noticias falsas para las primeras etapas de su difusión en la red social Twitter llamado Early Rumor Detecion Model (ERDM). Para esto se utilizaron tanto características del contexto de difusión como el contenido en los tweets.

## Datasets

Los datos usados en la experimentación corresponden a los datasets Twittter 15 y Twitter 16, disponibles oficialmente [aquí](https://www.dropbox.com/s/7ewzdrbelpmrnxu/rumdetect2017.zip) y también en la carpeta ```data/```. La información contenida es la siguiente:

- ```label.txt``` : contiene el id de cada noticia con su clase, la que puede ser true rumor, false rumor, non-rumor y unverified rumor.
    ```
    true:614467824313106432
    unverified:715515982584881152
    ```
- ```source_tweets.txt``` : id de las noticias con el texto contenido en ellas.
    ```
    767223774072676354	former 3 doors down guitarist matt roberts has died at age 38, according to his father. URL URL
    715515982584881152	craigslist ad: ‘get paid $15 an hour to protest at the trump rally’ - URL URL
    ```
- ```tree/``` : carpeta con los árboles de propagación de cada noticia, donde cada nodo representa una tripla (id usuario, id tweet, marca temporal), indicando en cada línea cual es el nodo padre y nodo hijo.
    ```
    ['972651', '80080680482123777', '0.0']->['189397006', '80080680482123777', '1.8']
    ```

El contenido de los tweets y usuarios no está disponible por políticas de Twitter por lo que deben ser descargados utilizando su API. Para mayor información de los archivos del dataset, consultar el README asociado.

## Modelo de detección

El modelo de detección propuesta está conformado por una red recurrente bidireccional que genera una codificación de la secuencia de entrada y un módulo de atención que rescata información relevante del contexto de la noticia. El resultado de ambos componentes es utilizado como entrada en una red feedforward para realizar la predicción de la noticia en estudio.

La entrada de la red corresponde a las secuencias de embeddings de cada ruta de propagación ordenados secuencialmente según la marca temporal, desde el primer mensaje (la noticia) hasta el último tweet de la cadena. Cada uno de estos embeddings contiene la representación AWE GloVE del tweet y la información del usuario. El vector de usuario pasará por una capa lineal y luego se junta con el embedding del tweet para ingresar a la BiGRU. El archivo ```twitter_data.ipynb``` contiene el código usado para la generación de las secuencias de embeddings. Una vez procesados los datos, ```rumor_model.ipynb``` puede ser utilizado para entrenar y evaluar la arquitecutra propuesta ERDM. El modelo está implementado usando PyTorch y puede ser ejecutado en Google Colab en el caso de no tener acceso a una GPU. El código viene diseñado para pruebas binarias, pero en el caso de las pruebas a cuatro clases se debe cambiar la dimensión de salida del modelo.


## Otros métodos

Para repoducir los experimentos se utilizaron diversos métodos de la literatura para los cuales algunos de sus códigos están disponibles en github.

- [GCAN](https://github.com/l852888/GCAN)
- [NEC](https://github.com/maryram/NEC)
- [CSI](https://github.com/sungyongs/CSI-Code)
- [RvNN](https://github.com/majingCUHK/Rumor_RvNN)


# Where are They looking?

## Resumen
Los humanos tienen la notable capacidad de seguir la mirada de otras personas, cuando conversas con alguien y observas algo destrás de él (ella) volteará por instinto.
Esta habilidad va de mano con la empatía, que nos permite asumir lo que otra persona esta pensando, como se siente o si va a realizar a alguna acción.

En el presente se expone la propuesta del paper
_[Where are they looking?](http://gazefollow.csail.mit.edu/)_ cuyo enfoque comprende el uso de redes neuronales profundas para el seguimiento de miradas con un nuevo conjunto de datos de referencia 'GazeFollow'


El enfoque consiste en tomar la posición de la cabeza en la imagen, para identifica el rango de visión y los objetos contenidos en dicho rango; y por medio de la red profunda concluir que se está observando. Este enfoque produce resultados confiables, incluso cuando solo se ve la parte posterior de la cabeza. Pero aún no se tiene el rendimiento humano en esta tarea.

---------------------------------------------

## Introducción ##
texto seccion 1

### Trabajos Relacionados
- __Saliency:__

- __Gaze:__

---------------------------------------------

## Gaze Follow: Un Conjunto de Datos a Gran Escala
Con el objetivo de realizar este trabajo, se construyó GazeFollow, que es un basto conjunto de datos que incluye imágenes y objetivos visuales que contempla:

<center>
  <table>
    <tr>
      <th>__Conjunto de Datos__</th>
      <th>__Cantidad de Imágenes__</th>
    </tr>
    <tr><td>[SUN][1] </td><td><p style="text-align:right">1548</p></td></tr>
    <tr><td>[MS COCO][2] </td><td><p style="text-align:right">33790</p></td></tr>
    <tr><td>[Actions 40][3] </td><td><p style="text-align:right">9135</p></td></tr>
    <tr><td>[PASCAL][4] </td><td><p style="text-align:right">7791</p></td></tr>
    <tr><td>[ImageNet][5]. </td><td><p style="text-align:right">508</p></td></tr>
    <tr><td>[MIT Places][6]. </td><td><p style="text-align:right">198097</p></td></tr>
  </table>
</center>

[1]: (https://groups.csail.mit.edu/vision/SUN/)
[2]: (http://cocodataset.org/#home).
[3]: (http://vision.stanford.edu/Datasets/40actions.html).
[4]: (http://host.robots.ox.ac.uk/pascal/VOC/databases.html).
[5]: (http://www.image-net.org/)
[6]: (http://places.csail.mit.edu/)

Cuya concatenación resultó en un desafiante conjunto de datos que incluye una inmensa colección de personas en diferentes tipos de escenarios.

__Un poco sobre los Datos__
> Este conjunto de datos no incluye la verdad sobre la tierra de la mirada de cada persona, así que fue añadido usando Mechanical Turk de Amazon ([AMT](https://www.mturk.com/mturk/welcome)); con el cual se marcaron manualmente los centros focales y el punto donde de observación. Lo que incluye detalles como si el punto de observación está fuera de la imagen o si la cabeza no era visible

> Para mejorar la data se incluye imágenes con el valor certero de visión con objetivo y se descarta casos deficientes obteniendo: _130339_ personas en _122143_ imágenes con punto focal incluido en la imagen.

__Preparación__

Se separó la data en train (117361) y test (4782); con la condición que cada persona en una imagen forme parte de la misma división.
Para evitar bias, las imágenes de prueba se distribuyeran uniformemente en toda la imagen.
Además, para evaluar la consistencia humana en el seguimiento de la mirada, recogimos 10 anotaciones de la mirada por persona para el conjunto de test.

<table>
  <tr><img src="imagenes/fig2.1.png"></tr>
  <tr><p style="text-align:center"><b>GazeFollow Dataset:</b><i>Se contemplan las anotaciones del objetivo de la mirada para cierto grupo de imágenes</i></p>
  </tr>
</table>


<table>
  <tr>
    <th colspan="3"><p style="text-align:center">Densidad de Probabilidad</p></th>
    <th colspan="2"><p style="text-align:center">Dirección Objetivo Promedio</p></th>
  </tr>
  <tr>
    <td width="150"><img src="imagenes/fig2.b1.png"></td>
    <td width="150"><img src="imagenes/fig2.b2.png"></td>
    <td width="150"><img src="imagenes/fig2.b3.png"></td>
    <td width="150"><img src="imagenes/fig2.b4.png"></td>
    <td width="150"><img src="imagenes/fig2.b5.png"></td>
  </tr>
  <tr>
    <td>Densidad de probabilidad para la ubicación de la cabeza.</td>
    <td>Densidad de probabilidad para la ubicación de la fijación objetivo.</td>
    <td>Densidad de probabilidad para la fijación normalizada respecto a la posición de la cabeza.</td>
    <td>Densidad de dirección promedio de fijación objetivo según posición de la cabeza.</td>
    <td>Color de código de intensidad de fijación objetivo (centro punto más probable).</td>
  </tr>
</table>

---------------------------------------------

## Learning to Follow Gaze
Principalmente el modelo está inspirado en la tendencia de los humanos a seguir la mirada hacia objetos en particular. Cuando las personas desean saber donde un persona está viendo en un momento determinado, generalmente ven primero la cabeza y ojos para saber el campo de visión y luego analizar que objetos podrían ser los que esta persona este observando de acuerdo a su perspectiva.
![seccion3](imagenes/fig3.png "Arquitectura de Red")

### Gaze and Saliency Pathways
Suponiendo que tenemos una imagen en particular x<sub>i</sub> y una persona para la cual deseamos predecir su mirada. Parametrizamos a esta persona con una ubicación espacial cuantificada de la cabeza de la persona x<sub>p</sub>, un recortado, una imagen de primer plano de su cabeza x<sub>h</sub>. Lo que se busca es predecir la ubicación espacial de la fijación de la persona representado por ***y*** utilizando redes profundas.
EL diseño de la red esta basada principalmente en dos vías, la primera para la mirada(gaze) y la segunda para los rasgos sobresalientes(saliency). Para la primera vía solo se tiene acceso a la imagen de primer plano de la cabeza de la persona y su ubicación, y se produce un mapa espacial.  
- __Saliency map:__
Para formar la vía saliency se usa una red convolucional en toda la imagen para producir una representación oculta de tamaño ***D x D x K***
- __Gaze mask:__
De forma similar, para el camino de la mirada usamos tambien una red convolucional pero en la imagen de la cabeza, luego se concatena su salida con la posición de la cabeza y se utiliza varias capas completamente conectadas y un sigmoide para predecir la mascara de mirada de dimensiones ***D x D***.
- __Pathway visualization:__
A continuación se mostrará imagenes que representan el mapa de saliency y la máscara de gaze aprendida por nuestra red la cual aprende una noción de saliencia que es relevante para la tarea de seguimiento de mirada. La primera imagen muestra la salida de la máscara de mirada para distintas posiciones de cabeza. En la segunda se muestra tres partes en una solo imagen, la primera parte es la imagen de entrada, la segunda parte es la saliencia de visualización libre estimada y la saliencia que sigue la mirada estimada usando nuestro modelo.

| Gaze mask    | Saliency     |
| :------------- | :------------- |
|![seccion3](imagenes/fig4.a1.png "Gaze mask") | ![seccion3](imagenes/fig4.b1.png "Saliency")|
| ![seccion3](imagenes/fig4.a2.png "Gaze mask")| ![seccion3](imagenes/fig4.b2.png "Saliency")|
| | ![seccion3](imagenes/fig4.b3.png "Saliency") |


### Multimodal Predictions
A pesar que los humanos casi siempre son capaces de seguir la mirada de una persona de manera confiable en algunas oportunidades esta puede ser ambigua, por ejemplo si hay muchos objetos salientes en la imagen o la dirección de la vista de la persona no es muy bien percibida entonces habrá este tipo de incertidumbre.

__Shifted grids:__
Para la clasificación, en primer lugar se debe elegir el número de celdas, ***N***. Pero la eleción de este parámetro es importante ya que si se eligiera un valor bajo de ***N*** tendríamos muy poca precisión en los resultados, en cambio si eligieramos una valor alto de ***N*** tendríamos más precisión pero el proceso de aprendizaje sería más difícil porque las perdidas de clasificación estandar no penalizarían adecuadamente las categorías espaciales.
### Training
La red end-to-end que utilizamos es creada usando backpropagation además se usó un ***softmax loss***(se define como la combinación de un ***cross-entropy loss***, una ***softmax function*** y la última capa completamente conectada) para cada ***shifted grid*** y promediar sus pérdidas. Además debido a que el modelo solo es supervisado con fijaciones de la mirada, no se considera que las vías de la mirada y la saliencia resuelvan sus respectivos subproblemas, mas bién se espera que la propia estructura de nuestro modelo resuelva automaticamente estos conflictos.

__Implementation details:__
Para la implementación del modelo se usó un framework de deep learning llamado ***Caffe***, las capas convolucionales en las dos vías, tanto de la de saliency como en la de gaze, están basadas en la arquitectura de las 5 primeras capas de la arquitectura de AlexNet.

![seccion3](imagenes/fig5.1.png "Resultados Cualitativos")
![seccion3](imagenes/fig5.2.png "Resultados Cualitativos")
![seccion3](imagenes/fig5.3.png "Resultados Cualitativos")

<img src="imagenes/fig5.4.png" width="295"/> <img src="imagenes/fig5.5.png" width="295"/> <img src="imagenes/fig5.6.png" width="290"/>

---------------------------------------------

## Experiments
### Setup
### Resultados
### Análisis
__Ablation study:__
__Internal representation:__
__Automatic head detection:__

---------------------------------------------

## Conclusión


## Referencias

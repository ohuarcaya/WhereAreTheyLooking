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

  Conjunto de Datos| Cantidad de Imágenes
  :--------- | --------:
  [SUN][1]. | 1548
  [MS COCO][2] | 33790
  [Actions 40][3] | 9135
  [PASCAL][4] | 7791
  [ImageNet][5]. | 508
  [MIT Places][6]. | 198097
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

<center>
  <img src="imagenes/fig2.1.png">
  <p><b>GazeFollow Dataset:</b><i>Se contemplan las anotaciones del objetivo de la mirada para cierto grupo de imágenes</i></p>
</center>

<table>
  <tr>
    <td>
      <p><b>Heat Maps:</b><i>Resumen estadístico de las particiones del dataset.</i></p>
      <p>Los tres primeros mapas de calor de la figura muestran la densidad de probabilidad  para la ubicación de la cabeza (1), ubicación de la fijación (2) y la fijación normalizada respecto a la posición de la cabeza(3).</p>
      <p>Las imágenes de la parte inferior denotan la dirección promedio de rango de visión (4) para las posiciones de cabeza. Siendo el punto sin color el objetivo más óptimo(5).</p>
    </td>
    <td>
        <img src="imagenes/fig2.2.png">
    </td>
  </tr>
</table>



---------------------------------------------

## Learning to Follow Gaze
![seccion3](imagenes/fig3.png "Arquitectura de Red")
### Gaze and Saliency Pathways
- __Saliency map:__

- __Gaze mask:__

- __Pathway visualization:__
### Multimodal Predictions
__Shifted grids:__
### Training
__Implementation details:__

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

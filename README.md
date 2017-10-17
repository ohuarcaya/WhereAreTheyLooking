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
- 1548 imágenes de SUN [19].
- 33790 imágenes de MS COCO [13].
- 9135 imágenes de Actions 40 [20].
- 7791 imágenes de PASCAL [4].
- 508 imágenes de ImageNet detection challenge [17].
- 198097 imágenes de The Places dataset [22].

Cuya concatenación resultó en un desafiante conjunto de datos que incluye una inmensa colección de personas en diferentes tipos de escenarios.

![seccion3.1](imagenes/fig2.1.png "Test images examples")


![seccion3](imagenes/fig2.2.png "Test set Statistic")

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
 see [@Ref01] xD


## Referencias
- [19] J. Xiao, J. Hays, K. A. Ehinger, A. Oliva, and A. Torralba. SUN database: Large-scale scene recognition from abbey to zoo. In CVPR, 2010.


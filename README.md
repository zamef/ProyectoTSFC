# Proyecto Final de Temas Selectos de Física Computacional. (Inteligencia Artificial)

## Clasificación de radiografias de torax con o sin neumonía.
### Por:

*   Cuevas Garcia Ana Cristina
*   Medrano Flores Zahid

### *¿Qué es la Neumonía?*

La neumonía es una infección que inflama los sacos aéreos de uno o ambos pulmones. Estos sacos pueden llenarse de líquido o pus, lo que provoca síntomas como tos con flema, fiebre, escalofríos y dificultad para respirar. La neumonía puede ser causada por diversos microorganismos, incluyendo bacterias, virus y hongos.

#### *Detonantes de la neumonía*
Los principales detonantes de la neumonía son:

* **Bacterias**: La más común es el Streptococcus
pneumoniae.
* **Virus**: Como el de la gripe (influenza) y el virus sincitial respiratorio (VSR).
* **Hongos**: Menos comunes, pero pueden afectar a personas con sistemas inmunitarios debilitados

#### *Personas más vulnerables*
Algunas personas son más susceptibles a contraer neumonía debido a varios factores:

* **Niños menores de 5 años**: Su sistema inmunológico aún está en desarrollo.
* **Adultos mayores de 65 años**: El sistema inmunológico se debilita con la edad.
* **Personas con enfermedades crónicas**: Como asma, diabetes, enfermedades cardíacas, enfermedad pulmonar obstructiva crónica (EPOC) o cáncer.
* **Personas inmunocomprometidas**: Aquellos con VIH, en tratamiento con quimioterapia o que toman medicamentos inmunosupresores.
* **Fumadores**: El tabaquismo daña las defensas naturales del sistema respiratorio.

### Objetivo del proyecto

Puesto que casi 2000 niños mueren diariamente en el mundo por culpa de la neumonía, la principal causa de muerte en niños menores de cinco años y la principal causa de hospitalización de adultos en Estados Unidos, con más de un millón de ingresos al año; unos 40 000 estadounidenses mueren anualmente por esta enfermedad.
El propósito del proyecto es poder identificar la presencia de neumonía, dado que existen varios tipos de la misma (nos encantaría implementar en el futuro, que el modelo nos pueda ayudar a decirnos el tipo de neumonía, de momento solo nos dirá si la padecemos o no) y puede ser peligrosa para un porcentae grande de personas.
como objetivo principal es dar un **Diagnóstico temprano y preciso**:
* *Reducción de errores humanos*: Los radiólogos pueden pasar por alto signos sutiles de neumonía, especialmente en etapas tempranas. Un modelo de IA puede ayudar a identificar estos signos con mayor precisión.
* *Detección rápida*: La IA puede analizar radiografías en segundos, lo que permite un diagnóstico más rápido y, por ende, un tratamiento más oportuno.

Ademas de ser un **Apoyo a profesionales de la salud**:

* *Una segunda opinión*: Los modelos de IA pueden servir como una segunda opinión para los médicos, ayudándoles a confirmar sus diagnósticos.
* *Capacitación y educación*: Los modelos pueden ser utilizados como herramientas educativas para entrenar a nuevos radiólogos.

Sabemos que el **acceso en áreas remotas** puede ser crucial para la salud en ciertas localidades:

* *Telemedicina*: En regiones con escasez de especialistas, un modelo de IA puede ser utilizado para analizar radiografías enviadas digitalmente, facilitando el acceso a diagnósticos precisos en áreas rurales o desatendidas.

Podemos crear una **eficiencia en el sistema de salud**:
* *Optimización de recursos*: Al automatizar parte del proceso de diagnóstico, se pueden liberar recursos humanos para casos más complejos.
* *Reducción de costos*: Un diagnóstico más rápido y preciso puede reducir los costos asociados con tratamientos tardíos o incorrectos.

Y claramente un mantenimiento de éste modelo puede ser clave para tratar esta enfermedad.

### Preprocesamiento del dataset

Usaremos una base de datos en *Kaggle*, que recopila imágenes de radiografías de tórax, etiquetadas en 2 categorías:

* Normal (muestra pulmones limpios, visíbles)

![imagen](https://github.com/user-attachments/assets/aefd7899-84dc-46a0-9cdd-e807d4481bb6)

* Pneumonía (Podemos apreciar blancuzca la zona, no podemos ver los dos pulmones en su totalidad)
  
![imagen](https://github.com/user-attachments/assets/2780a43e-1d90-4284-8919-83ba40c4c361)

Este dataset se encuentra en el siguiente link:
https://www.kaggle.com/datasets/tolgadincer/labeled-chest-xray-images

El dataset contiene 5,856 imagenes de radiografías de torax, Las imágenes están etiquetadas como (enfermedad:NORMAL/BACTERIA/VIRUS)-(ID aleatoria del paciente)-(número de imagen de un paciente). y las imágenes se dividen en un conjunto de entrenamiento y un conjunto de prueba de pacientes independientes.

Se preprocesaron las imágenes de la siguiente manera.
* Primero verificamos las dimensiones de las imágenes para encontrar la de menor resolución, la de mayor resolución y una imágen cercana al promedio.
* Al darnos cuenta que la mas pequeña recortaba el área de interés (en éste caso los pulmones), y la imagen cercana al promedio se encontraba con una resolucion de alrededor de 1000x1000, decidimos descartar toda imagen menor a esas medidas.
* De igual manera no todas la imágenes se encontraban en una escala de grises. Por ende se pasaron las imagenes a escalas de grises donde encontramos que el brillo promedio en las imagenes fue de aproximadamente 140.

Después de preprocesar el dataset nos quedamos con 2,389 imágenes. Las imágenes seguian variando en tamaño, por lo cual se redimensionaron para igualarlas y reducir el poder computacional requerido en la red neuronal.

Se creó un nuevo dataframe donde juntamos todas las imágenes y se les agregó una etiqueta que nos dijera si el pulmón esta sano o enfermo.

### Implementación de Modelos

#### CNN

Implementamos una red convolucional con el API de Keras. En lugar de usar un modelo preentrenado se realizó desde "cero".
Creamos el modelo con tres capas principales que realizan la convolución asi como un pooling 2D y un par de capas de Dropout (que ayudan a mejorar la diversidad en el entrenamiento), para pasar a una capa densa. Además utilizamos un par de Callbacks, uno que reduce el LearningRate cuando el aprendizaje se "estanca" y otro para detener el modelo si ya no está aprendiendo más.

Entrenamos el modelo con 50 épocas, pero debido al Callback se detuvo en las 25 épocas. Obtuvo una precisión de 0.8558 con una pérdida de 0.3709 en su entrenamiento.

Evaluamos el modelo, y realizamos una matriz de confusión, que se presenta a continuación.

![imagen](https://github.com/user-attachments/assets/07649b4d-5be7-4d8f-872d-ef1b9a3193fd)

Podemos notar que el modelo fue muy malo, con muchos errores y alcanzando una precisión de alrededor del **50%**.

#### SVM

Como la CNN fue poco satisfactoria, se decidió realizar otros modelos para ver que tanto podiamos alcanzar de precisión en el limitado dataset con el que contamos.

Implementamos una Máquina de Soporte de Vectores, con ayuda de la API de Scikit. Aqui lo que podemos destacar fue el cambio del kernel a uno polinómico (se intentaron algunos más). Es de notar que tiene mayor simplicidad que una red neuronal. Además las imagenes se volvieron a redimensionar a un menor tamaño, 128x128, lo cual ayudó mucho en el tiempo que tomó entrenarse.

Ya entrenado, nuevamente utilizamos una matriz de confusión para poder ver la Precisión en las predicciones que hace el modelo, y se muestra a continuación.

![imagen](https://github.com/user-attachments/assets/0a71f29a-c624-4786-8154-13caa0f1d2d3)

Como es de notarse, el modelo realiza muy pocos errores. Hay que enfocarse mucho en los falsos positivos y negativos, que es donde realmente podemos afectar personas en el mundo real. El modelo alcanza una precisión de aproximadamente un **88%**

#### RandomForest

Finalmente utilizamos un RandomForest, de igual manera con el API de scikit.

Implementamos el modelo con 150 árboles y una profundidad de 10, de igual manera se experimentó con diversos parámetros. Y muy parecido al SVM es un modelo mas simple que una red neuronal.Entrenamos el modelo, siendo éste el más rápido, tardando un par de segundos. 

Y pues nuevamente lo evaluamos con una matriz de confusión, la cual se presenta a continación.

![imagen](https://github.com/user-attachments/assets/66c05d72-4212-4cf5-8117-feb12b55171e)

El modelo fue bastante bueno, con pocos errores y con una precisión de alrededor del **86%**.

### Conclusiones

El objetivo principal era utilizar una red convolusional para atacar el problema, pero como puede verse no es la mejor opción por como se comporta. El CNN muestra tener una alta precisión y una pérdida relativamente alta, pero, lo que mas afecta es como se ve la matriz de confusión que muestra como se equivoca mucho al intentar identificar la neumonia en los pacientes. Se muestra una precisión real de aproximadamente el 50%. Otro problema grande que presenta una red convolucional es el poder computacional que requiere, y por ende el tiempo que tarda en ejecutarse.

El SVM nos proporciona mejores resultados todo esto con imagenes mas pequeñas que llegan a acortar el tiempo y poder necesario para realizar un mejor trabajo. Se nos presenta una precision muy alta rondando el 90%.

Finalmente el RandomForest de igual manera nos arroja mejores resultados, y con incluso menor poder necesario, haciendo la misma tarea en cuestión de segundos. Tuvo una precisión igualmente alta, llegando a aproximadamente el 85%.

El proyecto fue una cuestión de experimentación tanto de modelos, asi como de parametros en los mismos que nos ayudaran a exprimir toda mejora que se pudiera.

# Proyecto Final de Temas Selectos de Física Computacional.

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

### Procedimiento del proyecto
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

### Implementación de la red neuronal

Utilizamos el API de Keras, con el cual construimos una red neuronal convolucional con 3 capas.

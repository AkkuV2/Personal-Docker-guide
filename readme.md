# Guia personal de docker.
Esta es una guia basica de docker para comenzar a guardar los avances de estudio de docker como un bloc de notas personal guardado en git para tener acceso al contenido en cualquier lado.

## ¿Que es docker?
Docker es una herramienta de desarrollo de software que permite crear contenedores en los cuales se pueden almacenar proyectos u servicios, con el objetivo de aislar de un equipo dichos proyectos, facilitar el manejo de despliegue de una app e inclusive permitir la reproduccion de un sistema de forma mucho mas sencilla al tener en un contenedor las dependencias y codigo de una aplicacion.

### ¿como funciona Docker?
Docker funciona con 2 bases - imagenes y contenedores -

#### Imagen
Una imagen es la que define todo lo necesario para ejecutar un programa dentro de un contenedor de docker, esta se crea a partir de un Dockerfile el cual tiene todo lo necesario.

Una imagen se puede interpretar como una plantilla u receta que contiene todas las instrucciones necesarias para que un programa se ejecute correctamente.
Si bien hay imagenes subidas a dockerhub, todas parten del mismo proceso para poder crear una imagen.

Ej: Una imagen de Debian 13 para poder ser subida solo requiere el filesystem de debian, no se sube nada referente al kernel porque los **contenedores de docker obtienen el kernel por el host que esta ejecutando dichos contenedores. El kernel lo comparte el pc. 

De esta manera cuando se sube una imagen, se sube solo lo necesario para que funcione en otros equipos.



#### Contenedor
Un contenedor de docker es una instancia ejecutando en tiempo real una imagen de docker que contiene todas las dependencias de un programa.

En base a los contenedores docker se puede reproducir un mismo programa en diferentes equipos evitando la problematica de que no funcione en otro equipo por faltar dependencias. 

#### Capas
Docker a la hora de utilizar un contenedor decide llenarlo con todo lo necesario para ejecutar un programa, una serie de capas que en conjunto permiten la ejecucion de cualquier programa dentro de si.

Un orden de capas de ejemplo seria:
- Dependencias del programa (Segunda capa)
- Kernel del host(no se descarga, el propio host lo comparte automaticamente dentro del contenedor - segunda capa)
- Debian/Alpine(systemfile de dicho sistema operativo - primera capa)
> **Nota:** El kernel no forma parte de la imagen ni de sus capas. Docker no lo descarga; el host lo comparte automáticamente con el contenedor.


## Comandos basicos de docker
Ya con toda la informacion explicada de que es docker y como funciona, se mostraran una lista de comandos de docker para poder usarlo correctamente.


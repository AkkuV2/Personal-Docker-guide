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

##### Ejemplo
 Una imagen de Debian 13 para poder ser subida solo requiere el filesystem de debian, no se sube nada referente al kernel porque los **contenedores de docker obtienen el kernel por el host que esta ejecutando dichos contenedores. El kernel lo comparte el pc. 

De esta manera cuando se sube una imagen, se sube solo lo necesario para que funcione en otros equipos.



#### Contenedor
Un contenedor de docker es una instancia ejecutando en tiempo real una imagen de docker que contiene todas las dependencias de un programa.

En base a los contenedores docker se puede reproducir un mismo programa en diferentes equipos evitando la problematica de que no funcione en otro equipo por faltar dependencias. 

#### Capas
Docker a la hora de utilizar un contenedor decide llenarlo con todo lo necesario para ejecutar un programa, una serie de capas que en conjunto permiten la ejecucion de cualquier programa dentro de si.

Un orden de capas de ejemplo seria:
- Dependencias del programa (Segunda capa)
- Codigo fuente del programa(Segunda capa)
- Kernel del host(no se descarga, el propio host lo comparte automaticamente dentro del contenedor - segunda capa)
- Debian/Alpine(systemfile de dicho sistema operativo - primera capa)
> **Nota:** El kernel no forma parte de la imagen ni de sus capas. Docker no lo descarga; el host lo comparte automáticamente con el contenedor.



## Comandos basicos de docker
Ya con toda la informacion explicada de que es docker y como funciona, se mostraran una lista de comandos de docker para poder usarlo correctamente.

### build
El comando build se usa cuando ya se tiene una Dockerfile definido y se quiere crear una instancia activa para contenirizar el proyecto, el comando es el siguiente:

```
docker build -t miapp .
```
>**Nota:** El comando -t es para especificar que le daras el siguiente nombre, y el . es para especificar 
que el comando se basara en el Dockerfile que encuentre en el directorio actual

### Dockerhub
Para descargar imagenes de dockerhub que puedas utilizar en tus imagenes, tienes el siguiente comando:

```
docker pull <paquete>
```

Un ejemplo mas explicito seria

```
docker pull debian:trixie-backports
```

> **Nota:** Algunas imagenes estan construidas a partir de otras imagenes generando una cadena de dependencias, por ello ciertas imagenes de dockerhub ya vienen con un system file incluido como en el caso de PHP

### imagenes de docker
Para ver una imagen hay un solo comando con varias opciones para especificar que quieres mostrar.

Para ver las imagenes instaladas y en uso:
```
docker images
```

Para ver todas las imagenes (incluyendo las no tageadas/eliminadas)
```
docker images -a
```
Para ver un resumen de informacion de todas las imagenes actuales de docker:
```
docker images --digests
```

Para ver solo las IDs de cada imagen:
```
docker images -q 
docker images --quiet
```

## Dockerfile
Un Dockerfile es un archivo que contiene una serie de instrucciones que representan como crear una imagen para poder usarla en un contenedor de docker (que es una instancia activa de una imagen de docker), cada instruccion del Dockerfile puede representar una capa del contenedor. Este tiene que ser organizado correctamente para poder beneficiarse de funcionalidades como la layer cache.

### Como crear un archivo Dockerfile
La estructura basica de un archivo Dockerfile es la siguiente:
```
FROM node:17-alpine   -------> primera capa, se especifica que imagen se usara, en este caso, se usa la imagen de node que a su vez esta basada en el file system de alpine

WORKDIR /app          -------> Segunda capa, en esta se declara que la ruta a trabajar (workdir) es /app, haciendo que cualquier ruta especificada sera por defecto en esta

COPY . .              -------> Segunda capa, el copy se encarga de copiar los archivos raiz del directorio actual y pegarlos en un directorio destino (solo que como ahora
                               tiene un workdir la ruta por defecto va a ser /app, por lo tanto no se tiene que especificar pues la ruta se vuelve absoulta)

RUN npm install       -------> Segunda capa, se instalan las dependencias del proyecto.

EXPOSE 4000           -------> Segunda capa, se expone el puerto que requiere el proyecto(esto es enteramente opcional)

CMD ["node", "app.js"] -------> Tercera capa, Se ejecutan comandos dentro de la instancia activa de la imagen, se usa CMD para especificar que son comandos a ejecutar 
                                atraves de la terminal, ya que los comandos RUN son para declarar pasos de la imagen en una instancia inactiva.
```

## Dockerignore
Dockerignore es una extension de archivo que especifica a docker que archivos u directorios no se deben de incluir como archivos copiados dentro de un Dockerfile. El archivo se escribe como **.dockerignore**

Dentro de este archivo se tiene la siguiente forma para especificar que archivos u directorios no copiar o tener en cuenta.
```
archivo.txt     ---> archivo unico.
carpeta-ejemplo/ ---> toda una carpeta, incluido su contenido.
```

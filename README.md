<!-- README.md is generated from README.Rmd. Please edit that file -->
ralparque
=========

Que es lo que obtienes cuando organizas una agrupacion (cluster) de roqueros (rockers)? Un festival!

R Al Parque es una prueba de concepto para coordinar contenedores Docker y hacerlos trabajar en conjunto como un grupo (*EN:cluster*) de trabajadores R bajo coordinación del paquete `snow`. En este momento no le tenemos una aplicación otra que casos ejemplos, y si tienes una aplicación para la aproximación que describimos aquí nos encantaría escucharlo.

Referencia
----------

En la carpeta `"manual/referencia` hay un ejemplo de hacer un snow-cluster dentro de una misma maquina. En nuestro caso lo usamos para hacer correr el codigo en un mismo contenedor, y lo usamos como referencia para comparar el resultado después.

Para correrlo sigue los siguientes pasos (asumimos que tienes [Docker](https://docker.com) instalado y funcionando).

    $ cd manual/referencia
    $ sudo docker build -t referencia .
    $ sudo docker run --name ejemplo -ti referencia

Verás en como parte del output, después de ver las funciones que asignamos a cada uno de los tres maquinas asignadas:

    [1] "Listo! Termine el script del Organizador en la implementacion 'referencia'"
    [1] "el resultado es: \n"
             1          2          3          4          5          6          7 
      7.662232  14.892228  28.978089   8.820645  22.477977  49.578876  34.956109 

Estos valores los tenemos que volver a ver cuando logremos crear un conjunto de contenedores separados.

Orquestración manual
====================

El Parque
---------

El primero paso es crear una red para los contenedores que vamos a iniciar. En otras palabras: necesitamos un parque para poner el podio. En Docker lo podemos [hacer asi](http://stackoverflow.com/questions/27937185/assign-static-ip-to-docker-container/35359185#35359185)

    $sudo docker network create --subnet=172.18.0.0/16 simonbolivar

El organizador
--------------

Necesitamos un [Mario Duarte](https://es.wikipedia.org/wiki/Rock_al_Parque) para tomar la iniciativa. En nuestro caso es el contenedor docker central que corre como el contenedor primario que dirije contenedores secundarios.

### Arranca el organizador

El organizador tiene su Dockerfile propio.

    $ cd manual/organizador
    $ sudo docker build -t organizador .
    $ sudo docker run -name duarte -ti organizador

Los artistas
------------

Nuestros artistas son los contenedores secundarios. Se construyen como instancias de un mismo Dockerfile

### Arranca los artistas

Primero construimos la imagen genérica para los artistas

    $ cd manual/artistas
    $ sudo docker build -t artista .

Y después incluimos tres actos en el festival

    $ sudo docker run --net simonbolivar --ip 172.18.0.02 --name aterciopelados -it artista
    $ sudo docker run --net simonbolivar --ip 172.18.0.03 --name fabulosos_cadillacs -it artista
    $ sudo docker run --net simonbolivar --ip 172.18.0.04 --name mana -it artista

Orquestración Automatica.
=========================

Docker Compose
--------------

TODO: Orchestrar los contenedores para que arranquen en conjunto
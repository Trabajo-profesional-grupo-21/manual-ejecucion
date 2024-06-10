# Manual de ejecución

Manual para poder ejecutar el sistema.



## Indice

[TOC]

## Requisitos

En cuanto a software:

- Docker

- Docker compose

  

En cuanto a hardware recomendamos contar con al menos 12GB de memoria RAM y procesador relativamente potente ya que los procesadores que corren los modelos de Machine Learning consumen una cantidad considerable de recursos.

**Importante:** se debe tener en cuenta que las imágenes de Docker de los componentes son relativamente grandes, llegando a ~10GB para los procesadores, por lo cual el proceso de build o descarga de las imágenes puede ser lento la primera vez. Luego, docker mantendrá un cache que permitirá realizar esto de forma mas rápida.



## Ejecución en simultaneo (recomendado)

Para poder probar el sistema y ejecutar todos los componentes en simultaneo se brinda a continuación un `docker-compose.yaml` el cual contiene todos los servicios necesarios.

Con este método no es necesario clonar los repositorios ya que todas los servicios tomaran las imágenes desde DockerHub, en donde las almacenamos con los contenidos de las ramas `main` de cada repositorio.

A su vez, el `docker-compose.yaml` creara servicios locales de RabbitMQ, Redis, MongoDB y tambien un emulador de Google Cloud Object Storage para que se pueda probar el sistema sin necesidad de crear instancias reales de estos servicios.

Pasos a seguir:

- Descargar el siguiente `docker-compose.yaml`: [Archivo](https://raw.githubusercontent.com/Trabajo-profesional-grupo-21/manual-ejecucion/main/resources/docker-compose.yaml)

- En el directorio donde se encuentra el archivo ejecutar: 

  ```bash
  docker compose up
  ```

- Esto abrirá un navegador con la interfaz web listo para ser usado

- Una vez finalizado puede eliminar los contenedores ejecutando:

  ```
  docker compose down
  ```

  

## Ejecución por separado (para desarrollo)

Para poder ejecutar el sistema de forma sencilla todos los repositorios cuentan con un `docker-compose.yalm` el cual sirve para levantar los diferentes componentes.



WIP

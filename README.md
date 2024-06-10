# Manual de ejecución

Manual para poder ejecutar el sistema.



## Indice

- [Manual de ejecucion](#manual-de-ejecución)
  - [Indice](#indice)
  - [Requisitos](#requisitos)
  - [Ejecución en simultaneo (recomendado)](#ejecución-en-simultaneo)
  - [Ejecución por separado (para desarrollo)](#ejecución-por-separado)

## Requisitos

En cuanto a software:

- Docker

- Docker compose

  

En cuanto a hardware recomendamos contar con al menos 12GB de memoria RAM y procesador relativamente potente ya que los procesadores que corren los modelos de Machine Learning consumen una cantidad considerable de recursos.

**Importante:** se debe tener en cuenta que las imágenes de Docker de los componentes son relativamente grandes, llegando a ~10GB para los procesadores, por lo cual el proceso de build o descarga de las imágenes puede ser lento la primera vez. Luego, docker mantendrá un cache que permitirá realizar esto de forma mas rápida.



## Ejecución en simultaneo

Para poder probar el sistema y ejecutar todos los componentes en simultaneo se brinda a continuación un `docker-compose.yaml` el cual contiene todos los servicios necesarios.

Con este método no es necesario clonar los repositorios ya que todas los servicios tomaran las imágenes desde DockerHub, en donde las almacenamos con los contenidos de las ramas `main` de cada repositorio.

A su vez, el `docker-compose.yaml` creara servicios locales de RabbitMQ, Redis, MongoDB y tambien un emulador de Google Cloud Object Storage para que se pueda probar el sistema sin necesidad de crear instancias reales de estos servicios.

Pasos a seguir:

- Descargar el siguiente `docker-compose.yaml`: [Archivo](https://github.com/Trabajo-profesional-grupo-21/manual-ejecucion/blob/main/resources/docker-compose.yaml)

- En el directorio donde se encuentra el archivo ejecutar: 

  ```bash
  docker compose up
  ```

- Una vez levantados todos los servicios, se puede interactuar con el Frontend abriendo el navegador en `http://localhost:3000/`  o clickeando en: [Link al Frontend](http://localhost:3000/)

- Una vez finalizado puede eliminar los contenedores ejecutando:

  ```
  docker compose down
  ```

  

## Ejecución por separado

Para poder realizar pruebas en componentes particulares, para desarrollo de nuevas funcionalidad, o si se quiere levantar el sistema sin utilizar los emuladores, es posible ejecutar cada componente por separado.

Para esto todos los repositorios cuentan con un `docker-compose.yaml` el cual sirve para iniciar los diferentes componentes.

WIP

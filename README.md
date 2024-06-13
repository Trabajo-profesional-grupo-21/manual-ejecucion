# Manual de ejecución

Manual para poder ejecutar el sistema.



## Indice

- [Manual de ejecución](#manual-de-ejecución)
  - [Indice](#indice)
  - [Requisitos](#requisitos)
  - [Ejecución en simultaneo (recomendado)](#ejecución-en-simultaneo)
  - [Ejecución por separado (para desarrollo)](#ejecución-por-separado)
    - [Utilizando servicios locales](#utilizando-servicios-locales)
    - [Utilizando servicios productivos](#utilizando-servicios-productivos)

## Requisitos

En cuanto a software:

- Docker

- Docker compose

  

En cuanto a hardware se recomienda contar con al menos 12GB de memoria RAM y procesador relativamente potente ya que los procesadores que corren los modelos de Machine Learning consumen una cantidad considerable de recursos.

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

Los diferentes componentes pueden ser ejecutados de forma independiente. Para ello, todo los repositorios cuentan con un `docker-compose.yaml` el cual sirve para iniciar los componentes de forma sencilla.

Existen dos formas para correr el sistema, utilizando servicios locales para RabbitMQ, Redis, Mongo y Google object storage, o utilizando servicios reales. 

### Utilizando servicios locales

En primer lugar se deben iniciar los emuladores para luego iniciar los componentes de nuestro sistema.

**Pasos**

- Clonar repositorio de [RabbitMQ](https://github.com/Trabajo-profesional-grupo-21/rabbitmq) y ejecutar `docker compose up`

  Es importante iniciar por RabbitMQ ya que es quien crea la `network` de docker a la cual se conectan todos los servicios. En caso de que por algún motivo no se necesite iniciar el servicio de rabbit, se puede crear esta red de forma manual ejecutando:

  ```bash
  docker network create custom_network
  ```
  
  
  
- Clonar repositorio de [Storage](https://github.com/Trabajo-profesional-grupo-21/storage) y ejecutar `docker compose up`

- Clonar los componentes [Api](https://github.com/Trabajo-profesional-grupo-21/api), [Arousal processor](https://github.com/Trabajo-profesional-grupo-21/arousal-processor), [Valence processor](https://github.com/Trabajo-profesional-grupo-21/valence-processor) y [Joiner](https://github.com/Trabajo-profesional-grupo-21/joiner) y ejecutar en cada uno `docker compose up`. 

  Considerar que la construcción de las imágenes puede demorar un tiempo considerable, así como también la descarga de los modelos pre-entrenados en `valence-processor`.

- Luego de que todos los componente se encuentran corriendo se puede ejecutar el Frontend, clonando [el repositorio](https://github.com/Trabajo-profesional-grupo-21/front) y ejecutando nuevamente `docker compose up`



Si se desea parar los servicios se debe ejecutar `docker compose down` en cada uno de ellos.



### Utilizando servicios productivos

En caso de querer implementar el sistema en un ambiente productivo y conectarse a los servicios deben indicar las credenciales en los archivos `.env`de cada componente (en cada repositorio se encuentra un template `.env_example`) y estos sobrescribiran los valores por defecto.

Ejemplo de `.env` para la API:

```python
# These values override the defaults in config/config.py
GOOGLE_APPLICATION_CREDENTIALS="path to service account credentials file (json)"

REDIS_HOST="redis url"
REDIS_PORT="redis port"
REDIS_PASSWORD="redis password"

MONGODB_URL="mongodb url"
MONGODB_DB_NAME="mongodb database name"

REMOTE_RABBIT=False # If false, it will connect to a local instance of RabbitMQ, otherwise you must set the variables below
RABBIT_HOST="Rabbit host"
RABBIT_PORT="Rabbit port"
RABBIT_VHOST="Rabbit virtual host"
RABBIT_USER="Rabbit user"
RABBIT_PASSWORD="Rabbit password"

ACCESS_TOKEN_EXPIRE_MINUTES="expiration time of jwt access token"
SECRET_KEY="secret key for jwt"
JWT_ALGORITHM="jwt algorithm"
```



Como se puede notar, para el caso de `GCP Object Storage`, lo único que se debe indicar es la ruta al archivo de configuración de tipo `service-account`.

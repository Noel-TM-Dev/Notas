
# DOCKER 🐋
  
> ## Que es un contenedor? 
Es una manera de empaquetar las aplicaciones, incluyendo dependencias y archivos de configuracion entre otras cosas.

 #### *Ventajas*
 - Los contenedores son portables y pueden ser compartidos entre desarrolladores y los operadores (devops).´
 - Hace que el despliegue y el desarrollo más fácil

  
> ### Donde se almacenan dichos contenedores?
  
  Se almacenan en un repositorio de contenedores los cuales son:

  - **Repositorios Privados**
  - **Repositorios Publicos** `(DockerHub)`


> ### Que se almacena en esos contenedores?

  En esos contenedores se alamcenan imagenes, en esas imagenes contienen diferentes dependencias basadas en linux, algunos ejemplos son: Nodejs, Mysql, etc.

> ### Que es una imagen?

 Una imagen es el empaquetado que es lo que contiene las dependencias, el codigo y esta es la que finalmente se comparte.

> ### Que es finalmente un container?

Es un conjunto de imagenes de las cuales la imagen mas inferior es la distribucion de linux y las superiores como Mysql.

*******
> # Comandos

### Comandos para imagenes

 - `docker images` -> comando para mostrar las imagenes descargadas              
 - `docker pull {nombre_imagen}:{?version}` -> Comando para descargar una imagen si no se especifica la version se descargara la version mas reciente de dicha imagen.

   - *Ejemplo* `docker pull node:16`

- `docker image rm {nombre}:{?version} ` => Comando para eliminar una imagen, si se cuenta con varias imagenes con el mismo nombre se usan las versiones para identificarlas.


### Comandos para Contenedores

- `docker container create { nombre_imagen_base } ` o `docker create { nombre_imagen_base }` -> Comando para crear un contenedor que se basa sobre la imagen descrita. Al correr el comando correctamente este nos devuelve un ID el cual nos sirve para identificarlo. 

  - *Ejemplo* `docker create node`
- `docker create --name { nombre_contenedor } { nombre_imagen_base }` -> Comando para crear un contenedor con un nombre para identificar mejor dicho contenedor
- `docker start { container_id | name_container }` -> Comando para iniciar el contenedor.

- `docker ps` -> comando que devuelve una tabla donde se encuentran los contenedores que estan corriendo.

- `docker stop { container_id }` -> comando para parar el contenedor.

- `docker ps -a` ->Comando para ver todos los contenedores incluso los que no estan corriendo, todos los que han sido creados.

- `docker rm { name_table_containers }` -> comando para eliminar un contenedor identificado con el nombre descrito en la tabla de contenedores.

- `docker create -p{ physical_port }:{ port_container } --name { nombre } { nombre_imagen_base }` -> comando para crear puertos mapeados, esto quiere decir que se utilizan para tener conexion entre los puertos interiores del contenedor con los puertos exteriores de la maquina fisica.

- `docker logs { name_container | id_container }` -> comando para saber todos los logs que se han realizado con ese contenedor.

- `docker logs follow { name_container | id_container }` -> comando para estar viendo los logs sin interrupciones en tiempo real.

- `docker run { ?nombre_imagen }` -> comando utilizado como acceso directo o rapido para crear un contenedor, la secuencia es la siguiente: 
   1. Revisa si existe una imagen, si no se encuentra la imagen la descarga.
   2. Crea un contenedor
   3. Inicia el contenedor

- `docker run -d { nombre_imagen }` ->Comando en modo dettached para evitar los logs y evitar parar el contenedor que se ha iniciado

- `docker run --name { name } -p{port}:{port_container} -d { nombre_imagen }` -> Comando similar a  ``docker create`` con la diferencia que este se salta muchos pasos y si no se encuentra la imagen descrita la descarga y al finalizar inicia el proceso del contenedor.

### Comandos para las redes de Docker

- `docker network ls`-> Comando utilizado para listar todas las redes que tiene configurado docker

- `docker network create { nombre_red }` -> Comando para crear una nueva red en la cual se le especifica el nombre y nos devuelve su id de identificación

- `docker network rm { nombre_red }` -> Comando para eliminar una red
*******
> ## Crear imagen desde un Dockerfile

- `docker build -t { nombre_app }:{ version_app } .`-> comando para crear una imagen por medio de un archivo ***Dockerfile*** 

> ### Codigo de Dockerfile
 
 Para crear un archivo DockerFile es necesario crearlo en la raiz del proyecto en el cual se escriben los siguientes comandos:

```Dockerfile
  FROM node:18  //De que imagen se va a basar 
   
  RUN mkdir -p /home/app //Crear carpeta en el SO de Docker p`ara guardar el proyecto

   COPY . /home/app //Permitirnos acceder a la carpeta del proyecto para poder moverlo desde nuestro sistema
   
   EXPOSE 3000  //puerto por el cual se va a exponer la aplicacion
   
   CMD ["node","/home/app/index.js"] //colocar el comando para iniciar el proyecto
   
```


****

> ## Agrupar contenedores por red

 Los contenedores se pueden agrupar, por lo cual se pueden asociar a una misma red, para que se comuniquen entre ellos mismos, normalmente los contenedores estan separados pero cuando se asignan a una misma red tanto mongo, nodejs, mysql entre otros. Puedan estar enlazados. 

- `docker create -p{ puerto_pc }:{ puerto_docker } --name { nombre_contenedor } --network { nombre_red }` -> Comando para crear un contenedor ligandole una red

- `docker create -p{ puerto_pc }:{ puerto_docker } --name { nombre_contenedor } --network { nombre_red } { nombre_imagen_que_creamos }:{ no_version }` -> comando para crear un contenedor ligandole una red y una imagen creada a partir de un proyecto
------------------
## Docker Compose
Compose es una herramienta para definir y ejecutar aplicaciones Docker de varios contenedores. Con Compose, utiliza un archivo YAML para configurar los servicios de su aplicación. Luego, con un solo comando, crea e inicia todos los servicios desde su configuración.

1. ### Crear archivo docker-compose.yml
   
   ```yaml
     version: "3.9",
     services: //se dan de alta los nombre de los contenedores
       miapp: 
         build: .
         ports: 
           - "3000:3000"
         links:  //nombres de los contenedores los cuales vamos a ligar
           - monguito       
       monguito:  
         image: mongo,
         ports: 
           - "27017:27017"
         environment: 
           - MONGO_INITDB_ROOT_USERNAME= noel
           - MONGO_INITDB_ROOT_PASSWORD= password
 
   ```
2. ### Correr comando de compose `docker compose up`


> ### Otros comandos para compose

 - `docker compose down` -> comando para eliminar los contenedores generados por el archivo **docker-compose.yml**

----
## Volumes

 Los volúmenes son el mecanismo preferido para conservar los datos generados y utilizados por los contenedores de Docker. Esto quiere decir que los datos almacenados en bases de datos y archivos de codigo se quedan pesistiendo aunque se eliminen los contenedores.

 > ### Tipos
   - **Anonimo** : Docker se encarga de guardar donde el decida. Lo malo es que despues no podremos referenciarlo hacia otros contenedores que deseemos utilizar.

   - **Anfitrion o Host**: el usuario decide donde desea guardarlo, que carpeta guardar y donde guardarlo.

   -**Nombrado**: el usuario decide el nombre y este es posible referenciarlo para poder utilizarlo.

> ### Trabajar con Volumes
   En el mismo archivo de docker-compose.yml se agrega lo siguiente:
   ```yaml
     version: "3.9",
     services: //se dan de alta los nombre de los contenedores
       miapp: 
         build: .
         ports: 
           - "3000:3000"
         links:  //nombres de los contenedores los cuales vamos a ligar
           - monguito       
       monguito:  
         image: mongo,
         ports: 
           - "27017:27017"
         environment: 
           - MONGO_INITDB_ROOT_USERNAME= noel
           - MONGO_INITDB_ROOT_PASSWORD= password

         //se agrega una propiedad mas como nodo de monguito  
         volumes: 
           - mongo-data:/data/db 
           # mysql -> /var/lib/mysql
           # postgres -> /var/lib/postgresql/data

     //se agrega en la raiz el nombre de volumes para agregar aqui todos los volumes 
     volumes:
       mongo-data:   //se agrega el nombre del volume

   ```

***
## Ambientes y Hot reload

Para crear archivos en el caso de modo desarrollo para trabajar con dependencias determinadas como nodemon para realizar pruebas de desarrollo

 > ### Crear archivo Dockerfile.dev
  
  ```Dockerfile
   FROM node:18  //De que imagen se va a basar 
   
   RUN npm i -g nodemon
   RUN mkdir -p /home/app //Crear carpeta en el SO de Docker para guardar el proyecto

   WORKDIR /home/app

   EXPOSE 3000  //puerto por el cual se va a exponer la aplicacion
   
   CMD ["nodemon","index.js"] //colocar el comando para iniciar el proyecto en modo desarrollo
    
  ```
  
 > ### Crear archivo docker-compose-dev.yml

  ```yaml
     version: "3.9",
     services: //se dan de alta los nombre de los contenedores
       miapp: 
         build: 
           context: .
           dockerfile: Dockerfile.dev
         ports: 
           - "3000:3000"
         links:  //nombres de los contenedores los cuales vamos a ligar
           - monguito
         volumes:
            #{ruta_actual}:{ruta_destino_contenedor}
           - .:/home/app         
       monguito:  
         image: mongo,
         ports: 
           - "27017:27017"
         environment: 
           - MONGO_INITDB_ROOT_USERNAME= noel
           - MONGO_INITDB_ROOT_PASSWORD= password

         //se agrega una propiedad mas como nodo de monguito  
         volumes: 
           - mongo-data:/data/db 
           # mysql -> /var/lib/mysql
           # postgres -> /var/lib/postgresql/data

     //se agrega en la raiz el nombre de volumes para agregar aqui todos los volumes 
     volumes:
       mongo-data:   //se agrega el nombre del volume
  ```

  Para arrancar un archivo personalizado que no sea el archivo compose se usar el siguiente comando:

  - `docker compose -f docker-compose-dev.yml up` 
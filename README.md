# Prueba Tecnica Backend MOK
Se desarrolló una arquitectura de micro servicios mediante el patron de arquitectura Api-Gateway,  se construyeron:

1.	un API-Gateway
2.	Un micro servicio de productos
3.	Un micro servicio de ordenes

Cada uno de estos se desarrolló mediante Java, a través el framework Spring-boot utilizando a gradle como gestor de dependencias. Para la comunicación entre cado uno de los componentes se optó por utilizar el Broker NATS https://nats.io/ 
además se utilizó postgresql para la persistencia de la data en cado uno de los microservicios.

!["arquitectura"](/assets/arquitectura.svg)

## API-GATEWAY
El Api-Gateway, es el único punto de entrada del sistema, el cual expone el puerto **3000** desde donde se podrá consumir cada uno de los microservicios. Ademas permite aplicar reglas de seguridad y orquestar la composición de de cada uno de los request.
https://github.com/asuridev/mok-api-gateway

## MICROSERVICIO DE PRODUCTOS
Este microservicio expone una API para gestionar las *categorias de los productos* y otra para los productos propiamente, mediante una relación 1:N dado que una categoría podría estar en muchos productos. El listado de las categorías y los productos se realiza mediante la API de pagination-repository que suministra spring-JPA y mediante query-params en el request se puede definir la pagina como la cantidad de elementos requeridos. (/product?page=1&limit=10)
https://github.com/asuridev/mok-products-msvc

## MICROSERVICIO DE ORDENES
El microservicio de ordenes expone una API para gestionar ordenes de productos, tambien gestiona la información de los items de las ordenes, mediante una relación 1:N dado que una orden podría tenes muchos items. Igual que el microservicio de productos implementa la API de pagination-repository que suministra spring-JPA para realizar paginacion.
https://github.com/asuridev/mok-orders-msvc

## DESPLIEGUE 📦
Para desplegar localmente el proyecto se debe clonar el repositorio  https://github.com/asuridev/mok-launcher.git este repositorio posee a las referencia a nivel de modulos del API-Gateway y los microservicio.

```
git clone https://github.com/asuridev/mok-launcher.git
```

```
cd mok-launcher
```

```
git submodule update --init --recursive
```

```
docker compose up --build
```
 Esto debe construir las las imagenes de docker y correr en un contenedor de docker-compose con el API-Gateway, el servidor de NATS, los dos microservicio, las dos bases de datos postgresql.
 #### IMPORTANTE
 El comando **git submodule update --init --recursive** realiza la clonación de los repositorios de los proyectos en el repositorio launcher que solo posee la referencia de estos.

 ## FUNCIONAMIENTO
 Una Vez en funcionamiento, se puede iniciar a interactuar con los microservicio mediante el API-Gateway que solo expone el puerto 3000 y el único punto de entrada del sistema.
 para el verificación comparto la documentación desarrollada en postman.

 https://documenter.getpostman.com/view/19057359/2sA3JDikwZ

 ### NOTA

 Para el desarrollo de los proyecto utilicé una CLI de mi autoría, la cual me permite construir un scaffolding de los proyecto para garantizar clean architecture.
 comparto repositorio.

 https://github.com/asuridev/jcup




# SPRING BOOT 3 COOKBOOK

Configuración de un Authorization Server en Spring.

Se usa `Spring` y la dependencia `OAuth2 Authorization Server`.

## Creación de proyecto

Uso Spring Initializr: `https://start.spring.io/`

![alt Spring Initialzr](./images/01-Spring-Initializr.png)

## Ejecución del proyecto

- Clonar/descargar este proyecto
- Ejecutar el proyecto con el comando: `./mvnw spring-boot:run`
    - O ejecutar directamente desde IntelliJ Idea
- Podemos recuperar la configuración de nuestro server haciendo una petición a `http://localhost:9000/.well-known/openid-configuration`
  - Este endpoint lo implementan todos los proveedores de OAuth2 para exponer la configuración de las aplicaciones cliente
- Para obtener un token se puede ejecutar el siguiente comando curl, con el programa en ejecución:
  ```
  curl --location 'http://localhost:9000/oauth2/token' \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'grant_type=client_credentials' \
  --data-urlencode 'client_id=football' \
  --data-urlencode 'client_secret=SuperSecret' \
  --data-urlencode 'scope=football:read'
  ```
  - O ver la carpeta postman, con la siguiente configuración (y pulsar el botón `Get New Access Token`):
  ![alt Postman_Config](./images/02-Postman-Config.png)
- Después de obtener el token, podemos ir a la web `jwt.ms` y pegarlo ahí, y pulsar el botón `Decoded Token`
  

## Configuración del Authorization Server

Creamos un fichero `application.yml` en la carpeta `resources` y lo informamos, definiendo una aplicación que se puede autenticar utilizando el flujo de credenciales del cliente.
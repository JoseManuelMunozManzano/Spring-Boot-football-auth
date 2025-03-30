# SPRING BOOT 3 COOKBOOK

Configuración de un Authorization Server en Spring.

Se usa `Spring` y la dependencia `OAuth2 Authorization Server`.

## Creación de proyecto

Uso Spring Initializr: `https://start.spring.io/`

![alt Spring Initialzr](./images/01-Spring-Initializr.png)

## Ejecución del proyecto

- Clonar/descargar este proyecto
- Renombrar `application_template.yml` por `application.yml` e indicar `client-id` y `client-secret` (OAuth2)
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
  - Si indicamos en scope `football:read football:admin` entonces mandamos los dos scopes
  - Si indicamos en scope solo uno, entonces mandamos solo ese scope
- Después de obtener el token, podemos ir a la web `jwt.ms` y pegarlo ahí, y pulsar el botón `Decoded Token`
- Ejecutar también el proyecto de Resource Server: `https://github.com/JoseManuelMunozManzano/Spring-Boot-football-resource`
- Como se ha añadido la gestión de distintos scopes (solo lectura o admin) hay que tenerlo en cuenta
- Para el proyecto `https://github.com/JoseManuelMunozManzano/footballui-openid-authentication` tenemos otra forma de crear un token. Es `AuthenticationUI`
- Para el proyecto `Logging con Google Accounts` acceder a la ruta `http://localhost:9080/myself` 

## Configuración del Authorization Server

Creamos un fichero `application.yml` en la carpeta `resources` y lo informamos, definiendo una aplicación que se puede autenticar utilizando el flujo de credenciales del cliente.

## Protegiendo un API RESTful usando OAuth2 con diferentes scopes

Necesitamos aplicar diferentes niveles de acceso a la aplicación. Una forma general de acceso de lectura para los clientes y un acceso administrativo para poder cambiar data. 

Para conseguir esto, OAuth2 utiliza el concepto de `scopes`. Un `scope` es un parámetro que se usa para especificar el nivel de acceso y permisos que una aplicación cliente solicita a nuestro authorization server.

Los `scopes` nos ayudar a garantizar que los usuarios tengan el control sobre aquello a lo que realmente tienen permiso.

Vamos a modificar `application.yml` para creawr dos scopes, uno de solo lectura y otro para acceso admin.

![alt Scopes](./images/03-Scopes.png)

Ver también el proyecto de resource server `https://github.com/JoseManuelMunozManzano/Spring-Boot-football-resource` donde tendremos que aplicar la configuración aquí realizada.

## Configurando una aplicación MVC con autenticación OpenId

Para este proyecto, ver también el proyecto `https://github.com/JoseManuelMunozManzano/footballui-openid-authentication`.

Reutilizamos esta aplicación Authorization Server y vamos a usar Redis para mantener sesiones de la aplicación.

Redis lo estoy ejecutando en mi Raspberry Pi.

Tenemos que crear el registro del cliente en este Authorization Server. Para ello modificamos `application.yml` y añadimos el nuevo registro de cliente.

## Logging con Google Accounts

Vamos a hacer login en la app usando Google Accounts. En otros repositorios ya tengo como hacer esto, así que aquí no se vuelve a explicar.

Para este proyecto, ver también los proyectos `https://github.com/JoseManuelMunozManzano/Spring-Boot-football-resource` y `https://github.com/JoseManuelMunozManzano/footballui-openid-authentication`. 

Añadimos la siguiente dependencia al POM:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-client</artifactId>
</dependency>
```

En `application.yml` añadimos la configuración de cliente para Google.
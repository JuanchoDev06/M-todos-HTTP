# Métodos HTTP
En el mundo del desarrollo web existen diferentes métodos HTTP **(Protocolo de Transferencia de Hipertexto)**, también conocidos como verbos son los comandos que se utilizan para indicar la acción que se va a realizar sobre un recurso específico.

En este trabajo se hablará sobre los diferente y más usados métodos dentro del ambiente HTTP.

## **Get:** 
Para este método, cómo su nombre lo indica se encarga de **recuperar.** Este método principalmente se aplica para solicitar y recuperar datos de un servidor sin modificarlos. Así como se ha hablado en clase el método GET permite que un **cliente** (por ejemplo, una aplicación frontend) le pueda pedir datos al servidor. 

Los **casos en que se utiliza** este método son los siguientes:

•	Consultar datos públicos o de solo lectura, aquí un claro ejemplo podría ser el de **obtener todos los usuarios: GET /api/usuarios**

•	Otro ejemplo basándose en el anterior podría ser el de **obtener un usuario por su ID: GET /api/usuarios/10**

•	Cargar recursos estáticos, esto es comúnmente cuando estamos creando una web, una vez la desplegamos el navegador realiza peticiones como: 

  - GET /index.html
  
  - GET /styles.css
  
  -	GET /main.js
  
•	Y una de las más utilizadas en muchas páginas y aplicaciones web que es la búsqueda o filtración de información:

  - **GET /api/productos?categoria=ropa&precio_min=100000**

Pasando a la relación del método GET con la arquitectura Web este se hace un método clave en la arquitectura **REST,** pero se vuelve casi que nulo su uso en la arquitectura **SOAP** (el método POST es el estándar). ¿Por qué es más utilizada en la arquitectura **REST**?

•	Es la arquitectura más usada hoy en día.

•	Se basa en el uso de métodos HTTP estándar (GET, POST, PUT, DELETE) para interactuar con recursos. 

•	Se usa para **leer o recuperar recursos** sin modificarlos.

•	Es el método “de lectura” más usado dentro de REST.

Por otro lado, GET no se usa tanto en la arquitectura SOAP puesto que es una arquitectura más antigua y compleja que usa **XML** y generalmente **solo el método POST** realiza este tipo de cosas.

**Formas de uso:** Existen varias formas de uso en una petición GET, aquí algunas de ellas:

•	Petición HTTP estándar 

  -	GET /api/productos HTTP/1.1
  
  -	Host: tiendaonline.com
  
  -	Accept: application/json
  
  -	Authorization: Bearer <token>

•	Existen peticiones por parámetro URL

  -	GET /api/productos?categoria=ropa&precio_min=50000 HTTP/1.

•	Y también peticiones con herramientas de prueba, como POSTMAN
  -	curl -X GET "https://api.tienda.com/productos"

**Ejemplo práctico:** Un ejemplo práctico el cual estoy usando en mi PPI Jobsi es el siguiente.

  ```java
  @RestController
  @RequestMapping("/api/trabajos")
  public class TrabajoController {
      @GetMapping
      public List<Trabajo> obtenerTodosLosTrabajos() {
          return trabajoService.listarTodos();
      }
      @GetMapping("/{id}")
      public Trabajo obtenerTrabajoPorId(@PathVariable Long id) {
          return trabajoService.buscarPorId(id);
      }
  }
  ```
En este caso usar la anotación **@GetMapping** permite que el servidor sea capaz de responder diferentes solicitudes HTTP GET.


## **POST:** 
El método POST, como su nombre lo indica es un método el cual sirve para enviar datos hacia el servidor, todo esto con el fin de crear o procesar un recurso nuevo en el sistema.

Se aplica principalmente en situaciones donde el cliente **debe enviar información nueva** al servidor para que esta sea procesada o almacenada.

Se usa normalmente en casos como:


1.	Registrar un nuevo usuario
  -	**POST /api/usuarios**
  En donde se envían datos al servidor
```json
  {
    "nombre": "Juancho",
    "email": "juancho@jobsi.com",
    "password": "12345"
  }
```

2.	Iniciar sesión (login), en donde se envían credenciales para validarlas
  -	POST /api/auth/login

Pasando a la **relación con la arquitectura web**, POST cumple un rol de creación de recursos nuevos cuando se habla de un ámbito **REST**, el servidor devuelve un código HTTP **201 Created** si la operación se hizo de manera exitosa.

| **Acción**        | **Método** | **Endpoint**       | **Descripción**                                      |
|--------------------|------------|--------------------|------------------------------------------------------|
| Crear usuario      | POST       | `/api/usuarios`    | Crea un nuevo usuario en la base de datos.           |
| Crear trabajo      | POST       | `/api/trabajos`    | Publica un nuevo trabajo en Jobsi.                   |
| Enviar reseña      | POST       | `/api/resenas`     | Crea una nueva reseña.                               |

En **SOAP** es la arquitectura en donde el método POST es el más usado y casi que obligatorio. Aquí las peticiones se envían en un campo XML, no JSON.

Ejemplo con SOAP:
POST /WeatherService HTTP/1.1

Content-Type: text/xml

```xml
<soapenv:Envelope>
   <soapenv:Body>
      <GetWeather>
         <City>Medellín</City>
      </GetWeather>
   </soapenv:Body>
</soapenv:Envelope>
```

Siguiendo la metodología como con el método GET, aquí planteo un **ejemplo práctico** basándose en el PPI **Jobsi.**

```java
@RestController
@RequestMapping("/api/trabajos")
public class TrabajoController {
    @PostMapping
    public ResponseEntity<Trabajo> crearTrabajo(@RequestBody Trabajo nuevoTrabajo) {
        Trabajo trabajoGuardado = trabajoService.guardar(nuevoTrabajo);
        return ResponseEntity.status(HttpStatus.CREATED).body(trabajoGuardado);
    }
}
```
Un método en el cual se basa en recibir un JSON con los datos del nuevo trabajo y guardarlos en la base de datos.

## **PUT:**
El método **PUT** se utiliza para **actualizar un recurso existente** en el servidor.

La gran diferencia con **POST** es que este crea, mientras que **PUT** reemplaza o actualiza completamente un recurso ya existente.

Su aplicabilidad se basa principalmente en cuando un cliente tiene la necesidad de enviar datos nuevos, los cuales sustituyan un recurso **ya existente.**

En la mayoría de los casos, se usa para **actualizaciones completas** (no parciales).

A continuación, algunos casos comunes con el uso de PUT:
1.	Actualizar toda la información de un usuario
    -	PUT /api/usuarios/5
```json
{
  "nombre": "Juan Andrés",
  "email": "juancho@elpoli.edu.co",
  "rol": "estudiante"
}
```
2.	Actualizar un trabajo o publicación
    -	PUT /api/trabajos/8
```json
{
  "titulo": "Apoyo taller matematicas",
  "descripcion": "Apoyar con conocimientos sobre taller de ecuaciones",
  "estado": "PUBLISHED"
}
```

## **Relación con la arquitectura Web:**

En la arquitectura **REST, PUT** es un método **idempotente**, lo que significa que realizar la misma petición **varias veces**  no cambia el resultado.

Ejemplo de REST:
POST --> Crear Usuario, en el endpoint **/api/usuarios**  

PUT --> Actualizar Usuario, en el endpoint **/api/usuarios/5**, aquí se remplazan completamente los datos de usuario con ID 5

Por otro lado, en la **arquitectura SOAP** el método PUT no es muy usado puesto que SOAP funciona con mensajes XML dentro de un solo método HTTP (normalmente POST).

**Ejemplo práctico con Jobsi**
```java
@RestController
@RequestMapping("/api/trabajos")
public class TrabajoController {
    @PutMapping("/{id}")
    public ResponseEntity<Trabajo> actualizarTrabajo(@PathVariable Long id, @RequestBody Trabajo trabajoActualizado) {
        Trabajo trabajo = trabajoService.actualizar(id, trabajoActualizado);
        return ResponseEntity.ok(trabajo);
    }
}
```
Aquí en este método de Jobsi **@PutMapping** indica que se trata de una actualización completa del recurso.





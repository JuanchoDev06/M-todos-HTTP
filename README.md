# Métodos HTTP
En el mundo del desarrollo web existen diferentes métodos HTTP **(Protocolo de Transferencia de Hipertexto)**, también conocidos como verbos son los comandos que se utilizan para indicar la acción que se va a realizar sobre un recurso específico.

En este trabajo se hablará sobre los diferente y más usados métodos dentro del ambiente HTTP.

### **Get:** 
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
En este caso usar la anotación **@GetMapping** permite que el servidor sea capaz de responder diferentes solicitudes HTTP GET.















Jieke Xie
# DOCUMENTACIÓN API
## ¿Que es un API?
(Application Programming Interface) o (Interfaz de Programación de Aplicaciones)

Una API es un conjunto de reglas y protocolos que permiten a diferentes aplicaciones comunicarse
entre sí. 

- Ejemplo:

    Supongamos que quiero mirar el tiempo en mi teléfono, la aplicación del clima obtiene los datos 
    a través de una API, en vez de que la aplicación tenga que recolectar y procesar todos los datos
    por sí misma, utiliza la API para solicitar la información directamente al servicio meteorológico.

## API Estándarizada

Tenemos que tener en cuenta que la API que estemos desarrollando tiene que ser válida para cualquier 
aplicación. Para ello tendremos que cumplir unos estándares establecidos.

### API 
Una API proporcionar una interfaz estandarizada; esto permite integrar y utilizar
servicios externos de manera eficiente y segura, sin tener que preocuparse por los detalles
de implementación subyacentes.

Lo que vamos a crear es un conjunto de endpoints o URLs que permitirán a los
clientes (como Postman u otras aplicaciones) realizar solicitudes HTTP para
acceder y manipular los datos.


### API REST
Si una API es REST, quiere decir que va a respetar 6 principios de implementación, REST es una 
recomendación, no es un estándar como SOAP.

#### REST PRINCIPIOS

1. **Arquitectura Cliente-Servidor**
   - Separa la interfaz del usuario y la lógica del usuario (cliente) de la lógica del servidor.
   - El API y el Cliente son dos entes independientes.
   

2. **Sin Estado (Stateless)**
   - Cada solicitud del cliente al servidor debe contener toda la información necesaria para entender y procesar la solicitud.
   - El estado de sesión se mantiene en el cliente usando tokens.


3. **Cacheable (Almacenamiento en Caché)**
   - Las respuestas pueden ser marcadas como cacheables, lo que significa que el cliente puede almacenar en caché las respuestas para su uso futuro. 
   - Esto reduce la latencia y la carga en el servidor, mejorando el rendimiento y la eficiencia de la API.


4. **Sistema de Capas (Layered System)**
   - La arquitectura puede estar compuesta por múltiples capas, donde cada capa solo tiene conocimiento de la capa inmediatamente inferior.
   - Esto proporciona una mayor modularidad, ya que cada capa puede ser desarrollada, desplegada y escalada de forma independiente.


5. **Código Bajo Demanda (Code on Demand) -Opcional-**
   - Establece que, dentro de una respuesta a una solicitud realizada a una API REST, el servidor puede enviar código ejecutable al cliente. 
   - Este código es interpretado por el cliente para extender o modificar su funcionalidad.


6. **Interfaz Uniforme**
   - Solicitud ejecución API:
     - **Identificación de Recursos**: Cada recurso se identifica mediante un URI.
     - **Manipulación de Recursos a través de Representaciones**: Los recursos pueden ser representados y manipulados en diferentes formatos, como JSON XML.
     - **Mensajes Autodescriptivos**: Cada mensaje incluye suficiente información para describir cómo procesar la solicitud.
     - **HATEOAS (Hypertext As The Engine Of Application State)**: El servidor debe de facilitar información que nos diga cómo navegar por la API, no solo facilitar datos.

    

### REST FULL
Una API es REST FULL cuando además de utilizar las 6 restricciones, utiliza el protocolo **http** para su implementación,
los verbos que utiliza son: 

- **GET**: Solicitar un recurso o lista de recursos.
- **POST**: Enviar un recurso al servidor que queremos crear.
- **DELETE**: Eliminar un recurso.
- **PUT**: Modificar un recurso completo.
- **PATCH**: Modificar un recurso parcialmente.

### Respuestas HTTP
| Código | Tipo de información  | Tipos de códigos más concretos                                                               |
|--------|----------------------|----------------------------------------------------------------------------------------------|
| 1xx    | Información          |                                                                                              |
| 2xx    | Éxito                | 200 OK <br/> 201 Creado<br/>204 No content                                                   |
| 3xx    | Redireccionamiento   | 301 Movido<br/> 302-303 otra URL<br/>307 Redirección temporal<br/>308 Redirección permanente |
| 4xx    | Error en el cliente  | 400 Mala solicitud<br/> 401 Unauthorized<br/> 403 Forbidden                                  |
| 5xx    | Error en el servidor | 500 Error en el servidor<br/> 502 Gateway incorrecta<br/> 503 Servicio no alcanzable         |

### Payload de la respuesta: el Json
La respuesta puede estar en un json con diferente estructura. Todas
correctas si son conocidas y gestionadas por el cliente:
```json
{
    "object": "articles",
    "data": {
        "tittle": "Mi artículo",
        "body": "Contenido"
    }
}
```

```json
{
    "status": "success",
    "article": {
        "tittle": "Mi artículo",
        "body": "Contenido"
    }
}
```

```json
{
    "status": "success",
    "data": [
        {
            "tittle": "Mi artículo",
            "body": "Contenido"
        }
    ]
}
```

```json
{
    "message": "OK",
    "result": [
        {
            "tittle": "Mi artículo",
            "body": "Contenido"
        }
    ]
}
```

### JSON:API Specification
Es un intento de estandarizar la respuesta json, la comunicación entre cliente y servidor
Estructura del json:
- **data** y **errors**: excluyentes entre sí.
- **data**: es el principal y tendrá más contenido.
- **meta**: información libre.
- **included**
- **links**
- **jsonapi**

```json
{
    "data": [],
    "errors": [],
    "meta": [],
    "included": [],
    "links": [],
    "jsonapi": []
}
```

###  Comunicaciñón entre cliente y API: Content Negotiation
Tanto la solicitud, como la respuesta deben contener el header
**Content-Type”: “application/vnd.api+json**

| Request Type | Method(s)                    | Header                                         | Status Code                |
|--------------|------------------------------|------------------------------------------------|----------------------------|
| Request      | **GET, POST, PATCH, DELETE** | Accept: <br/> **application/vnd.api+json**     | 406 Not Acceptable         |
| Request      | **POST, PATCH**              | Content-Type:<br/>**application/vnd.api+json** | 415 Unsupported Media Type |
| Response     | **ALL**                      | Content-Type:<br/>**application/vnd.api+json** | 415 Unsupported Media Type |



<br>

## CREACIÓN DE UNA API

### Creación del proyecto

Abrimos una terminal y nos movemos al directorio de laravel, una vez dentro ejecutamos el siguiente comando:
```bash
  laravel new nombre_Proyecto_API --git
```
_**--git**_ nos inicia un repositorio Git en el directorio de nuestro proyecto creado.

Una vez ejecutado el comando nos saldrán una serie de opciones, seleccionamos las que están **por defecto** y en la base de datos
seleccionamos: **MySql**.

Una vez creado nuestro proyecto nos movemos al directorio.

<div style="background-color: black; padding: 10px; text-align: justify">
<b style="color: yellow">IMPORTANTE</b><br> Las rutas se establecen en <b>api.php</b> en lugar de <b>web.php</b>, lo que los diferencian son los middleware que se le 
aplican a cada uno, estos están descritos en <em><b>/app/http/kernel.php</b></em>.
<br><br>
Dentro de <b>kernel.php</b> nos encontraremos con dos grupos:
<ul>
    <li>
        <b>$middleware</b>: En el que se definen los middleware globales que se aplican a todas las solicitudes HTTP que llegan a la aplicación.
    </li>
    <li>
        <b>$middlewareGroups</b>: Organiza los middleware en grupos:
    </li>
</ul>
</div>

```php
    protected $middlewareGroups = [
            'web' => [
                \App\Http\Middleware\EncryptCookies::class,
                \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
                \Illuminate\Session\Middleware\StartSession::class,
                \Illuminate\View\Middleware\ShareErrorsFromSession::class,
                \App\Http\Middleware\VerifyCsrfToken::class,
                \Illuminate\Routing\Middleware\SubstituteBindings::class,
            ],
    
            'api' => [
                // \Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful::class,
                \Illuminate\Routing\Middleware\ThrottleRequests::class.':api',
                \Illuminate\Routing\Middleware\SubstituteBindings::class,
            ],
        ];
```

### Creación del modelo

```bash
  php artisan make:model Alumno --api -fm
```
<div style="background-color: black; padding: 10px;text-align: justify">
<b style="color: yellow">NOTA</b><br>
<em><b>--api</b></em> lo que nos hace es generarnos los métodos específicos para una API RESTful, que incluyen los siguientes métodos:
<ul>
    <b>
        <li>index()</li>
        <li>store()</li>
        <li>show()</li>
        <li>update()</li>
        <li>destroy()</li>
    </b>
</ul>
<em><b>-fm</b></em> nos crea la factoría y la migración
</div>

Una vez ejecutado el comando nos crea el modelo, controlador, factoría y migración.

### Modificar en archivo .env
```php
    APP_NAME=Laravel
    APP_ENV=local
    APP_KEY=base64:2LsZA1WpEGaIoFovyisJ8NXwi+oFyqCmn9nRmLk/Sxg=
    APP_DEBUG=true
    APP_URL=http://localhost
    
    LOG_CHANNEL=stack
    LOG_DEPRECATIONS_CHANNEL=null
    LOG_LEVEL=debug
    
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=23306
    DB_DATABASE=instituto
    DB_USERNAME=alumno
    DB_PASSWORD=alumno
    DB_PASSWORD_ROOT=root
    DB_PORT_PHPMYADMIN=8080
    
    BROADCAST_DRIVER=log
    CACHE_DRIVER=file
    FILESYSTEM_DISK=local
    QUEUE_CONNECTION=sync
    SESSION_DRIVER=file
    SESSION_LIFETIME=120
    
    MEMCACHED_HOST=127.0.0.1
    
    REDIS_HOST=127.0.0.1
    REDIS_PASSWORD=null
    REDIS_PORT=6379
    
    MAIL_MAILER=smtp
    MAIL_HOST=mailpit
    MAIL_PORT=1025
    MAIL_USERNAME=null
    MAIL_PASSWORD=null
    MAIL_ENCRYPTION=null
    MAIL_FROM_ADDRESS="hello@example.com"
    MAIL_FROM_NAME="${APP_NAME}"
    
    AWS_ACCESS_KEY_ID=
    AWS_SECRET_ACCESS_KEY=
    AWS_DEFAULT_REGION=us-east-1
    AWS_BUCKET=
    AWS_USE_PATH_STYLE_ENDPOINT=false
    
    PUSHER_APP_ID=
    PUSHER_APP_KEY=
    PUSHER_APP_SECRET=
    PUSHER_HOST=
    PUSHER_PORT=443
    PUSHER_SCHEME=https
    PUSHER_APP_CLUSTER=mt1
    
    VITE_APP_NAME="${APP_NAME}"
    VITE_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
    VITE_PUSHER_HOST="${PUSHER_HOST}"
    VITE_PUSHER_PORT="${PUSHER_PORT}"
    VITE_PUSHER_SCHEME="${PUSHER_SCHEME}"
    VITE_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"
    
    LANG_FAKE ="es_ES"
```

### Crear y editar el archivo docker-compose.yaml
```php
    #Nombre de la version
    version: "3.8"
    services:
      mysql:
        # image: mysql <- Esta es otra opcion si no hacemos el build
        image: mysql
    
        # Para no perder los datos cuando destryamos el contenedor, se guardara en ese derectorio
        volumes:
          - ./datos:/var/lib/mysql
        ports:
          - ${DB_PORT}:3306
        environment:
          - MYSQL_USER=${DB_USERNAME}
          - MYSQL_PASSWORD=${DB_PASSWORD}
          - MYSQL_DATABASE=${DB_DATABASE}
          - MYSQL_ROOT_PASSWORD=${DB_PASSWORD_ROOT}
    
      phpmyadmin:
        image: phpmyadmin
        container_name: phpmyadmin2  #Si no te pone por defecto el nombre_directorio-nombre_servicio
        ports:
          - ${DB_PORT_PHPMYADMIN}:80
        depends_on:
          - mysql
        environment:
          PMA_ARBITRARY: 1 #Para permitir acceder a phpmyadmin desde otra maquina
          PMA_HOST: mysql
```

### Levantar la base de datos con docker
```bash
  docker compose up -d
```
<div style="background-color: black; padding: 10px;text-align: justify">
<b style="color: yellow">NOTA</b><br>
Si tenemos problemas al levantar el docker podemos ejecutar uno de los siguientes comandos:
    <ul>
        <li><b>docker stop $(docker ps -a -q)</b>: detiene todos los contenedores Docker en ejecución, sin importar su estado actual.</li>
        <li><b>docker rm $(docker ps -a -q)</b>: elimina todos los contenedores Docker, sin importar su estado actual.</li>
    </ul>
</div>

### Levantar el servidor en local
```bash
  php artisan serve
```

### Poblar la base de datos

#### Factoy
Modificar la factoría de nuestro modelo en _**/database/factories/AlumnoFactory.php**_

```php
    public function definition(): array
        {
            return [
                "nombre"=>fake()->name(),
                "direccion"=>fake()->address(),
                "email"=>fake()->email()
            ];
        }
```
#### Seeder
Modificar el seeder de nuestro modelo en _**/database/seeders/DatabaseSeeder.php**_
```php
    public function run(): void
    {
        Alumno::factory(20)->create();
    }
```
<div style="background-color: black; padding: 10px; text-align: justify">
<b style="color: yellow">NOTA</b><br>
El seeder nos puebla la base de datos con los datos ficticios del factory.
<br>
<br>
En el código le indicamos que usamos la factory asociada al modelo para generar 20 instancias.
<br>
<br>
Los datos por defecto que nos genera el factory están en ingles, si queremos cambiar el idioma de esos datos tenermos que ir 
a <em><b>/config/app.php</b></em> y donde pone <em><b>'faker_locale' => 'en_US'</b></em> lo cambiamos a <em><b>'faker_locale' => 'es_ES'</b></em>.
</div>

#### Migration
Modificar la migración de nuestro modelo en _**/database/migrations/2024_02_20_092346_create_alumnos_table.php**_
```php
    public function up(): void
    {
        Schema::create('alumnos', function (Blueprint $table) {
            $table->id();
            $table->string("nombre");
            $table->string("direccion");
            $table->string("email");
            $table->timestamps();
        });
    }
```
<div style="background-color: black; padding: 10px; text-align: justify">
<b style="color: yellow">NOTA</b><br>
En la migración le estamos definiendo como queremos nuestra tabla.  
<br>
<br>
<em><b>$table->timestamps();</b></em> es una forma de agregar automáticamente registros de la fecha y la hora de creación
y modificación a la tabla en la base de datos.
</div>

Una vez modificado los archivos correspondientes, ejecutamos el siguiente comando para crear la tabla y poblarla:
```bash
  php artisan migrate:fresh --seed
```
<div style="background-color: black; padding: 10px; text-align: justify">
<b style="color: yellow">NOTA</b><br>
El comando anterior si ponemos el <em><b>:fresh</b></em> nos crea una base de datos limpia, eliminando previamente todas las tablas de la base de datos.
<br>
<br>
Si solo queremos actualizar o repoblar la base de datos quitamos el <em><b>:fresh</b></em>.
</div>

Una vez poblada nuestra base de datos generamos las clases de solicitud (**Request**), recurso (**Resource**) y recursos (**Resource Collection**) con los siguientes comandos:
```bash
    php artisan make:request AlumnoFormRequest
```
```bash
    php artisan make:resource AlumnoResource
```
```bash
    php artisan make:resource AlumnoCollection
```

Ya creado las clases, las modificamos las clases que creamos anteriormente:

#### AlumnoCollection.php
##### /app/Http/Resources/AlumnoCollection.php
Esta clase se utiliza para transformar las colecciones de modelos en una estructura de datos JSON.
```php
    public function toArray(Request $request): array
    {
        return parent::toArray($request);
    }
    public function with(Request $request)
        {
        return[
            "jsonapi" => [
                "version"=>"1.0"
            ]
        ];
    }
```
<div style="background-color: black; padding: 10px; text-align: justify">
<b style="color: yellow">NOTA</b><br>
La función <em><b>toArray</b></em> convierte la colección en un array. En este caso, nos devuelve <em><b>parent::toArray($request)</b></em>
que obtiene el array predeterminado de la colección y lo devuelve.
<br>
<br>
La función <em><b>with</b></em> nos agrega datos adicionales con el formato JSON:API.
</div>


<br>

#### AlumnoResource.php
##### /app/Http/Resources/AlumnoResource.php
Esta clase nos permite transformar los modelos y las colecciones en una estructura de datos JSON para nuestra API.
```php
    public function toArray(Request $request): array
    {
        return [
            "id"=>(string)$this->id,
            "type"=>"Alumnos",
            "attributes"=>[
                "nombre"=>$this->nombre,
                "direccion"=>$this->direccion,
                "email"=>$this->email,
            ],
            "links"=>[
                'self' => url('api/alumnos' . $this->id)
            ]
        ];
    }
    
    public function with(Request $request)
    {
        return[
            "jsonapi"=>[
                "version"=>"1.0"
            ]
        ];
    }
```
<div style="background-color: black; padding: 10px; text-align: justify">
<b style="color: yellow">NOTA</b><br>
La función <em><b>toArray</b></em> definimos como queremos representar los datos de nuestro modelo <em><b>Alumno</b></em> en formato JSON cuando devuelve una respuesta HTTP. 
<br>
<br>
La función <em><b>with</b></em> que tenemos en esta clase se sobreescribe con el <em><b>with</b></em> de la clase <em><b>AlumnoResource</b></em>, a la hora de mostrar la version del API   
</div>

<br>

### Indicar las rutas en /routes/api.php
Es donde se definen las rutas específicas para nuestra API, nos permite manejar los tipos de solicitudes HTTP y como interactúa con los recursos de nuestra aplicación.

```php
    Route::apiResource("alumnos" ,AlumnoController::class);
```
<div style="background-color: black; padding: 10px; text-align: justify">
<b style="color: yellow">NOTA</b><br>
Cundo indicamos la ruta en <em><b>api.php</b></em> laravel crea las rutas para realizar las operaciones de un CRUD, al definir estas rutas proporcionamos una API RESTful para interactuar con los recursos de alumnos.
</div>

<br>

### AlumnoController.php
Esta clase nos permite manejar las solicitudes entrantes (GET, POST, PUT, PATCH, DELETE) e interactuar con el modelo.
#### index
```php
    public function index()
    {
        $alumnos=Alumno::all();
        return new AlumnoCollection($alumnos);
        //return ($alumnos);
    }
```
<div style="background-color: black; padding: 10px; text-align: justify">
<b style="color: yellow">NOTA</b><br>
La función <em><b>index</b></em> maneja la solicitud para obtener todos los registros de alumnos de la base de datos y devolver una respuesta.
</div>

<br>

#### show
```php
    public function show(int $id)
    {
        $resource = Alumno::find($id);
        if (!$resource) {
            return response()->json([
                'errors' => [
                    [
                        'status' => '404',
                        'title' => 'Resource Not Found',
                        'detail' => 'The requested resource does not exist or could not be found.'
                    ]
                ]
            ], 404);
        }
        return new AlumnoResource($resource);
    }
```

<div style="background-color: black; padding: 10px; text-align: justify">
<b style="color: yellow">NOTA</b><br>
La función <em><b>show</b></em> maneja la solicitud para mostrar los detalles/datos de un alumno específico.
</div>

<br>

### Controlar los errores de fallo de conxión a la base de datos
#### /app/Exceptions/Handler.php
Esta clase nos permite manejar las excepciones de la aplicación, proporciona un punto centralizado para controlar y manejar todas las excepciones que se produzcan.

```php
     public function render($request, Throwable $exception)
        {
            // Manejo personalizado de ValidationException
            if ($exception instanceof ValidationException) {
                return $this->invalidJson($request, $exception);
            }
            //invalidJson
    
            // Errores de base de datos)
            if ($exception instanceof QueryException) {
                return response()->json([
                    'errors' => [
                        [
                            'status' => '500',
                            'title' => 'Database Error',
                            'detail' => 'Error procesando la respuesta. Inténtelo más tarde.'
                        ]
                    ]
                ], 500);
            }
            // Delegar a la implementación predeterminada para otras excepciones no manejadas
            return parent::render($request, $exception);
        }
```

<div style="background-color: black; padding: 10px; text-align: justify">
<b style="color: yellow">NOTA</b><br>
La función <em><b>render</b></em> maneja la presentación de las excepciones antes de ser enviadas al usuario.
</div>

<br>

<div style="background-color: black; padding: 10px; text-align: justify">
<b style="color: yellow">NOTA</b><br>
Para comprobar que nos funciona el control de excepción apagamos el docker con el siguiente comando:
</div>

```bash
    docker compose down
```

Y en el postman hacemos una petición GET a la URL de nuestra API:

```bash
    http://localhost:8000/api/alumnos/
```
Y nos muestra lo siguiente:
```json
    {
        "errors": [
            {
                "status": "500",
                "title": "Database Error",
                "detail": "Error procesando la respuesta. Inténtelo más tarde."
            }
        ]
    }
```

### Creación del Middleware
El middleware nos permite filtrar las solicitudes HTTP que ingresan a nuestra aplicación antes de que lleguen a las rutas o los controladores.

Creamos un middleware con el siguiente comando:
```bash
    php artisan make:middleware HandleMiddleware
```

Una vez creado nuestro middleware, lo modificamos:
```php
    public function handle($request, Closure $next)
    {
        if ($request->header('accept') != 'application/vnd.api+json') {
            return response()->json([
                'errors' => [
                    "status"=>406,
                    "title"=>"Not Acceptable",
                    "details"=>"Content File not specified"
                ]
            ], 406);
        }
        return $next($request);
    }
```
Una vez modificado hay que añadir la ruta dentro de _**/app/Http/Kernel.php**_ en _**$middlewareGroups**_ y dentro de _**api**_ ponemos la ruta:
```php
    protected $middlewareGroups = [
        'web' => [
            \App\Http\Middleware\EncryptCookies::class,
            \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
            \Illuminate\Session\Middleware\StartSession::class,
            \Illuminate\View\Middleware\ShareErrorsFromSession::class,
            \App\Http\Middleware\VerifyCsrfToken::class,
            \Illuminate\Routing\Middleware\SubstituteBindings::class,
        ],

        'api' => [
            // \Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful::class,
            \Illuminate\Routing\Middleware\ThrottleRequests::class.':api',
            \Illuminate\Routing\Middleware\SubstituteBindings::class,
            \App\Http\Middleware\HandleMiddleware::class
        ],
    ];
```

### Creación de artículo
Modificamos el controlador de nuestro modelo.
```php
  public function store(Request $request)
    {
        $datos = $request -> input("data.attributes");
        $alumno = new Alumno($datos);
        $alumno->save();
        return new AlumnoResource($alumno);
    }
```

Y en _**/app/Http/Requests/AlumnoFormRequest.php**_ agregamos la validación de los datos y autorizamos:
```php
    public function authorize(): bool
    {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array<string, \Illuminate\Contracts\Validation\ValidationRule|array<mixed>|string>
     */
    public function rules(): array
    {
        return [
            "data.attributes.nombre"=>"required|min:5",
            "data.attributes.direccion"=>"required",
            "data.attributes.email"=>["required|email|Rule::alumnos,email"]
        ];
    }
```
#### Alumno.php
Tenemos que ir a nuestro modelo (_**app/Models/Alumno.php**_) y asignamos que atributos podemos asignar masivamente.

```php
    protected $fillable=["nombre","direccion","email"];
```
Para comprobar que nos funcione, vamos a POSTMAN y hacemos una solicitud POST a nuestra URL de la API.

Antes de agregar el nuevo elemento tendremos que agregar un nuevo Header _**Accept**_ con el valor _**application/vnd.api+json**_ e insertamos el nuevo elemento en el apartado de _**body**_->_**raw**_
```json
{
    "data":
    {
        "type": "Alumnos",
        "attributes":
        {
            "nombre": "Jieke",
            "direccion": "Casa",
            "email": "J@gmail.com"
        }
    }
}
```
Y si nos lo ha creado correctamente nos devolverá un código _**201**_ y la salida de nuestro elemento creado con formato JSON.
```json
{
    "data": {
        "id": "21",
        "type": "Alumnos",
        "attributes": {
            "nombre": "Jieke",
            "direccion": "Casa",
            "email": "J@gmail.com"
        },
        "links": {
            "self": "http://localhost:8000/api/alumnos21"
        }
    },
    "jsonapi": {
        "version": "1.0"
    }
}
```

<br>

### Eliminación de un artículo
Modificamos el controlador de nuestro modelo.
```php
    public function destroy(int $id){
            $alumno = Alumno::find($id);
            if (!$alumno) {
                return response()->json([
                    'errors' => [
                        [
                            'status' => '404',
                            'title' => 'Resource Not Found',
                            'detail' => 'The requested resource does not exist or could not be found.'
                        ]
                    ]
                ], 404);
            }
            $alumno->delete();
            return response()->json(null, 204);
        }
```

Para comprobar que nos funciona vamos a POSTMAN y hacemos una solicitud DELETE a nuestra URL de nuestra API especificando el artículo que queremos eliminar en la URL.

```bash
    http://localhost:8000/api/alumnos/1
```

Y si se ha eliminado correctamente nos tendrá que devolver un código 204, que nos indica que el servidor ha procesado correctamente la solicitud pero no hay ningún contenido que devolver.

Y si intentamos eliminar un artículo que no existe nos tendrá que mostrar lo siguiente:
```json
    {
    "errors": [
        {
            "status": "404",
            "title": "Resource Not Found",
            "detail": "The requested resource does not exist or could not be found."
        }
    ]
}
```

### Actualizar los datos del artículo
Modificamos el controlador de nuestro modelo.
```php
    public function update(Request $request, int $id)
    {
        $alumno = Alumno::find($id);
        if (!$alumno) {
            return response()->json([
                "errors" => [
                    "status" => 404,
                    "title" => "Resource not found",
                    "details" => "$id Alumno not found"
                ]
            ], 404);
        }
        $verbo = $request->method();
        //En función del verbo creo unas reglas de
        // validación u otras
        if ($verbo == "PUT") { //Valido por PUT
            $rules = [
                "data.attributes.nombre" => ["required", "min:5"],
                "data.attributes.direccion" => "required",
                "data.attributes.email" => ["required", "email",
                    Rule::unique("alumnos", "email")
                        ->ignore($alumno)]
            ];
        } else { //Valido por PATCH
            if ($request->has("data.attributes.nombre"))
                $rules["data.attributes.nombre"]= ["required", "min:5"];
            if ($request->has("data.attributes.direccion"))
                $rules["data.attributes.direccion"]= ["required"];
            if ($request->has("data.attributes.email"))
                $rules["data.attributes.email"]= ["required", "email",
                    Rule::unique("alumnos", "email")
                        ->ignore($alumno)];
        }

        $datos_validados = $request->validate($rules);

        foreach ($datos_validados['data']['attributes'] as $campo =>$valor)
            $datos[$campo]=$valor;

        $alumno->update($datos);
        return new AlumnoResource($alumno);

    }
```
Para comprobar que funciona vamos a POSTMAN y hacemos una solicitud PATCH y PUT a nuestra URL indicando el artículo y el JSON con los datos a actualizar.
```bash
    http://localhost:8000/api/alumnos/2
```
Insertamos el JSON
```json
{
  "data": {
    "attributes": {
      "nombre": "juanjda",
      "direccion":"juanjos asdafasf",
      "email": "ju@a.com"
    }
  }
}
```
Y si nos lo ha aplicado bien, nos tendrá que devolver un código 200 OK, y nos mostrara el artículo cambiado.
```json
    {
    "data": {
        "id": "2",
        "type": "Alumnos",
        "attributes": {
            "nombre": "juanjda",
            "direccion": "juanjos asdafasf",
            "email": "ju@a.com"
        },
        "links": {
            "self": "http://localhost:8000/api/alumnos2"
        }
    },
    "jsonapi": {
        "version": "1.0"
    }
}

```

### Otros errores (Validación)
#### /app/Exceptions/Handler.php
```php
    protected function invalidJson($request, ValidationException $exception):JsonResponse
        {
            return response()->json([
                'errors' => collect($exception->errors())->map(function ($message, $field) use
                ($exception) {
                    return [
                        'status' => '422',
                        'title' => 'Validation Error',
                        'details' => $message[0],
                        'source' => [
                            'pointer' => '/data/attributes/' . $field
                        ]
                    ];
                })->values()
            ], $exception->status);
        }
```


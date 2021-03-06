# 6. Express

[Documentación PDF](https://github.com/adolfodelarosades/KEEPCODING-CURSO_JS-NODEJS-EXPRESS-MONGODB/blob/master/pdfs/Express.pdf)

## 43.- MVC y otros frameworks (3:32)

### Estructura

En Node.js podemos usar cualquier patrón para estructurar nuestro código.

El patrón MVC es comúnmente usado por muchos desarrolladores por su buen resultado.

### MVC

<img src="/images/mvc.png">

Es un patrón que nos permite dividir nuetra aplicación en tres apartados:

* **Modelo**: Gestiona la información y se la proporciona al Controlador.
* **Vista**: Es la parte visual de nuestra aplicación. En el caso de un API podría ser el JSON que estamos entregando
* **Controlador**: Decide lo que se va a mostrar o lo que se va a guardar en el Modelo.

*Por ejemplo al arrancar una App se cargaría un Controlador que pide datos a un Modelo por ejemplo una lista de usuarios, el Modelo se la daría al Controlador el cual lanzaria una Vista con la lista de usuarios. Si en la lista de usuarios el usuario pinchara en el botón Eliminar Usuario, la vista le avisaria al Controlador de que desea eliminar dicho usuario, el Controlador verificaría algunas cosas y si realmente se debe borrar el usuario el Controlador se lo indicaría al Modelo para que proceda a eliminarlo, el Modelo avisaría al Controlador de que el usuario a sido borrado y el controlador daría esa misma confirmación a la vista para que sea mostrado al usuario final.*

* El Controlador recoge datos del modelo y los da a la Vista para que los represente en su interfaz.
* La Vista recibe las acciones del usuario y da información al controlador que opcionalmente guardará lo necesario en el modelo.
* El Modelo avisará a el Controlador de que hay nuevas versiones de los datos, y este opcionalmente las entregará a la Vista.

### Web Frameworks

Existen múltiples frameworks web para node.js y surgen nuevos con frecuencia, por ejemplo:

* Express.js 
* Koa
* Hapi 
* Restify
* ...

Muchos de ellos extienden la funcionalidad de Express.js, siendo este el más usado.

## 44.- Introducción a Express (2:19)

[Express](http://expressjs.com/) es un framework web para Node.js

Podemos revisar toda la funcionalidad en su [API 5.x](http://expressjs.com/en/5x/api.html)
 
## 45.- Ejercicio: primera app express (9:08)

Dentro de la carpeta `cursonode`:

* Crear la carpeta `express_app_basica`.
* Entrar a la carpeta con `cd express_app_basica`.
* Escribir el comando `npm init` para que nos cree el fichero `package.json`.

<img src="/images/express01.png">

Con esto se nos crea el archivo `package.json`:
```js
{
  "name": "express_app_basica",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```
* Ahora vamos a instalar express con el comando `npm install express --save` para que incluya la dependencia en `package.json`.

<img src="/images/express02.png">

Una vez instalado el express el archivo `package.json` es modificado para incluir la dependencia de express:

```js
{
  "name": "express_app_basica",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.1"
  }
}
```
Además nos ha creado la carpeta `node_modules` con la carpeta `express` y todas sus dependencias:

<img src="/images/express03.png">

Y tambien se ha creado el archivo `package-lock.json` donde esta toda la configuración express del proyecto.

Dentro de la carpeta `express_app_basica`:

* Vamos a crear el archivo `index.js`.
* Incluir el siguiente código:
```js
"use strict";

var express = require('express');
var app = express();

app.get('/', function(req, res) {
    console.log('Petición a /');
    res.send('Hello world');
});

var server = app.listen(3000, function() {
    var port = server.address().port;
    console.log('*** App de ejemplo arrancada en el puerto: ', port);
});
```

Explicación del código:

`var express = require('express');` Requerimos express.
`var app = express();` Creamos una aplicación de express.
Creamos una petición `get`, donde indicamos la ruta que se invocara (`/`) y el callback que se ejecutara cuando se invoque, por un lado saca el mensaje `Petición a /` en la consola y por otro lado envia el mensaje `Hello world` como respusta de la petición:
```
app.get('/', function(req, res) {
    console.log('Petición a /');
    res.send('Hello world');
});
```
Podriamos responder una página html completa, un simple resultado, o hacer algo con la petición.

Nos permite crear un servidor donde indicamos el puerto (`3000`) y el callback que se ejecutará una vez iniciado el servidor, en este caso se enviará el mensaje `*** App de ejemplo arrancada en el puerto: 3000` en la consola:
```
var server = app.listen(3000, function() {
    var port = server.address().port;
    console.log('*** App de ejemplo arrancada en el puerto: ', port);
});
```
Con `var port = server.address().port;` recuperamos el puerto que ya sabemos que es el `3000`, es solo como ejemplo para saber el puerto donde esta arrancado el servidor.


* Ejecutamos el programa con `nodemon index.js`.

La salida es:

<img src="/images/express04.png">

Si cargamos nuestro navegador con `http://localhost:3000/` tenemos:

<img src="/images/express-navegador.png">

Y observamos que en la consola se muestra el mensaje que habiamos puesto al hacer una petión get:

<img src="/images/express05.png">

### Sistema de Logs Morgan


Podemos incluir un sistemas de logs en nuestro programa existen varios usaremos [Morgan](https://github.com/expressjs/morgan).

Lo primero es instalarlo con:

`npm i morgan --save`

Esto añade en el archivo `package.json` la dependencia:

```js
"dependencies": {
    "express": "^4.17.1",
    "morgan": "^1.9.1"
  }
```
Para utilizar morgan escribimos las siguientes líneas en `index.js`
```js
var morgan = require('morgan');
app.use(morgan('dev'));
```
Requerimos morgan y a continuación le decimos a express que lo utilice en modo desarrollo (`dev`). El modulo morgan nos da un Middleware.

Arrancamos nuestra aplicación con:
`nodemon`
No es necesario indicar `index.js` ya que lo busca por defecto.

Cuando arrancamos el servidor simplemente pinta el mensaje `*** App de ejemplo arrancada en el puerto:  3000` pero cuando cargamos la URL `http://localhost:3000/` en el navegador es cuando pinta el Log en la consola:

<img src="/images/express06.png">

## 46.- Express generator (6:42)

Express generator es una utilidad que va a generarnos una aplicación de Express con algunos componentes más y podemos usarla como punto de partida para iniciar una nueva aplicación.

### Usando Express Generator

Express Generator nos crea una estructura **base** para una aplicación.

`$ [sudo] npm install express-generator -g` Instalarla de manera global.

`$ express -h` Ver la lista de opciones que nos proporciona.

`$ express <nombreApp> [--ejs]` Crear una aplicacón básica de Express con cierto esqueleto.

`$ cd <nombreApp>` Cambiarnos a la carpeta del proyecto.

`$ npm install` Instalar las dependecias del proyecto.

Vamos a generar una aplicación con Express Generator. Lo primero que vamos a hacer instalar Espress Generator:

`$ [sudo] npm install express-generator -g`

Una vez instalado pulsamos:

`$ express -h`

<img src="/images/express-generator-install.png">

Dentro de la carpeta `cursonode`:

* Crear la carpeta `express_generator`
* Entrar a la carpeta `cd express_generator`
* Crear la aplicación `express generada`

<img src="/images/express-generada.png">
<img src="/images/express-generada-files.png">

* Cambiamos de directorio `cd generada`
* Instalamos dependencias `npm install`

<img src="/images/npm-install-generada.png">
<img src="/images/npm-install-generada-files.png">

* Lanzar la aplicación `npm start`
* Cargar en el navegador la URL `localhost:3000`

<img src="/images/express-navegador-02.png">
 
### Express Generator

Podemos establecer variables de entorno para variar la forma de arranque:

`$ npm start` Arrancar en desarrollo, puerto por defecto (3000) 

`$ NODE_ENV=production npm start` Arrancar en producción

*Las diferencias de lanzar en Desarrollo o Producción son los Logs que lanza la aplicación y la cantidad de información que proporciona cuando ocurre aqlgún error. Producción proporciona menos información para no develar posibles interioridades de nuestra aplicación.*

`$ DEBUG=nombreApp:* PORT=3001 NODE_ENV=production npm start` Arrancar con log debug activado, puerto 3001, entorno producción.

*Podemos personalizar el puerto, el * hace log de todos los modulos o podemos poner uno concretamente*

### Express Generator

Si queremos podríamos incluir esto en el comando `start` de `npm`, especificándolo en el `package.json`
```js
...
   "scripts": {
      "start": "DEBUG=nombreApp:* PORT=3002 nodemon ./bin/www"
}, ...
```
*Esta opción de arranque podemos incluirla en el archivo `package.json` que acepta varios scripts, damos un nombre y proporcionamos las distintas opciones de personalización de como queremos arrancar nuestra aplicación*
 
## 47.- Estructura y Rutas (9:34)

Las aplicaciones se pueden crear con un conjunto de plantillas, uno de los más sencillos es `ejs`. Para utilizar este motor de plantillas tenemos que decirselo a Express Generator, generame una aplicación con ese motor de plantillas.

Dentro de la carpeta `cursonode`:

* Crear la carpeta `express_generator_ejs`
* Entrar a la carpeta `cd express_generator_ejs`
* Crear la aplicación `express generada --ejs`

<img src="/images/express-generada-ejs.png">
<img src="/images/express-generada-ejs-files.png">

* Cambiamos de carpeta `cd generada`
* Instalamos dependencias `npm install`
* Lanzar la aplicación `npm start`
* Cargar en el navegador la URL `localhost:3000`

<img src="/images/express-navegador-02.png">
<img src="/images/express-navegador-03.png">
 
Si abrimos el archivo `package.json` vemos:
```js
{
  "name": "generada",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "start": "node ./bin/www"
  },
  "dependencies": {
    "cookie-parser": "~1.4.4",
    "debug": "~2.6.9",
    "ejs": "~2.6.1",
    "express": "~4.16.1",
    "http-errors": "~1.6.3",
    "morgan": "~1.9.1"
  }
}
```
Vemos que tenemos:
```js
"scripts": {
    "start": "node ./bin/www"
  },
```
Ejecuta un script llamado `start` con la instrucción `node ./bin/www`. Si vamos a la carpeta `./bin` tenemos el archivo `www` (Lanzador de nuestra aplicación) y si lo abrimos vemos todo su código:
```js
var app = require('../app');
var debug = require('debug')('generada:server');
var http = require('http');
...
```
Vemos que requiere el archivo `app.js` que es uno de los archivos de nuestra aplicación, donde tenemos la parte principal  de la aplicación:
```js
var createError = require('http-errors');
var express = require('express');
var path = require('path');
var cookieParser = require('cookie-parser');
var logger = require('morgan');

var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');

var app = express();

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');

app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', indexRouter);
app.use('/users', usersRouter);

// catch 404 and forward to error handler
app.use(function(req, res, next) {
  next(createError(404));
});

// error handler
app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;
```
Aquí:
* Requerimos express
* Configuramos express
* Añadimos los componentes que queremos usar como las `views`
* Que va a usar el motor de vistas `ejs`
* Como hacer logs
* Como parsear el body
* Indica algunas rutas que puede servir `/` y `users`
* Tiene un manejador 404
* Manejador de errores de desarrollo y producción

Podriamos abrir los Routes que requiere:

`index.js`:

```js
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});

module.exports = router;
```

`users.js`:

```js
var express = require('express');
var router = express.Router();

/* GET users listing. */
router.get('/', function(req, res, next) {
  res.send('respond with a resource');
});

module.exports = router;

```
En `index.js` :
* Requerimos express
* Creamos un router
* Creamos método `get` y asociamos la ruta
* Cuando hagamos la petición a `/` renderizara una vista llamada `index` con el título `Express` y en `users.js` simplemente envia el mensaje `respond with a resource`.

Si vamos a la carpeta `views` veremos las diferentes vistas que tenemos entre ellas una que se llame `index.ejs`:
```js
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1><%= title %></h1>
    <p>Welcome to <%= title %></p>
  </body>
</html>
```

**Todo esto que hemos visto a vista de pajaro, forma la estructura que se crea en una aplicación Express.**

### Rutas

Express define las rutas para tengan la siguiente estructura:

`app.METHOD(PATH, HANDLER)`

donde:
* app es la instancia de express
* METHOD es el método de la petición HTTP (GET, POST, ...) 
* PATH es la ruta de la petición
* HANDLER es la función que se ejecuta si la ruta coincide.
 
### Rutas

HTTP pone a nuestra disposición varios métodos, de los cuales generalmente hacemos distintos usos:

* **GET** para pedir datos, es idempotente (p.e. listas)
* **POST** para crear un recurso (p.e. crear un usuario)
* **PUT** para actualizar, es idempotente (p.e. guardar un usuario existente) 
* **DELETE** eliminar un recurso, es idempotente (p.e. eliminar un usuario)

**idempotente**: si lo ejecutas varias veces los resultados o el resultado del servidor no cambian.

### Rutas

Por lo tanto podríamos construir rutas como estas:
```js
app.get('/', function (req, res) { 
  res.send('Hello World!');
});

app.post('/', function (req, res) { 
  res.send('Guardado!');
});
```

### Orden de las rutas

El orden es importante!

En el orden que carguemos nuestras rutas a express es el orden en que las interpretará.

Si ponemos los estáticos después de nuestras rutas podremos *'sobre-escribir'* un fichero estático con una ruta, por ejemplo para comprobar si el usuario tiene permisos para descargarlo.

### Rutas

Express nos permite también usar `all` como comodín.

```js
app.all('/admin', function (req, res) { 
  console.log('Accediendo a sección admin ...'); 
  res.send('Admin zone');
});
```

Cualquier petición `get`, `post`, `put`, `delete` a `/admin` queremos hacer un tipo de acción no necesitamos definirlos todos vale con poner solo el `.all`.


### Rutas

Un uso tipico podría ser verificar unas credenciales, cualquier llamada a `/admin` verifica las credenciales y después la función `next`, que es un tercer parámetro y esta función cuando la llamemos significara que no hemos respondido sino que queremos que express pase la ejecución al siguiente handler que se haya definido para esa ruta y que ya respondera otro más adelante. 

Normalmente en las rutas de express podemos hacer dos cosas, en estos handlers o middleware podemos hacer dos cosas:

* Responder (caso anterior)
* Pasar el control al siguiente middleware (este caso)

```js
app.all('/admin', function (req, res, next) { //verificar credenciales
  next(); // pasa el control al siguiente handler
});
```
*next* pasa la ejecución al siguiente handler definido para esa ruta.

## 48.- Estáticos (1:18)

### Servir ficheros estáticos

Para servir ficheros estáticos como CSS, imágenes, ficheros javascript, etc, express generator nos ha creado una aplicación que ya tiene una entrada para esto, se especifica con un middleware llamado `express.static`:

```js
app.use(express.static(path.join(__dirname, 'public')));
```
*Le estamos diciendo que todo lo que haya en nuestra carpeta `public` de nuestro file system se va a servir en la raíz de nuestro site.*

Con esto serviremos lo que haya en la carpeta `public` como estáticos de la raíz de la ruta.

### Servir ficheros estáticos

Si queremos añadir otras carpetas de estáticos tenemos que especificar en que rutas colgarlos, podemos poner varias rutas.

```js
// la ruta virtual '/otros' servirá la carpeta '/otros'
app.use('/otros', express.static(path.join(__dirname, 'otros')));
app.use('/pdf', express.static(path.join(__dirname, 'pdf')));
```

## 49.- Recibiendo parámetros (5:23)

Veamos como recibir parámetros en nuestras rutas de express.

### Recibiendo parámetros

Habitualmente recibiremos parámetros en nuestros controladores de varias formas:

* En la ruta (`/users/5`): El 5 es un parámetro que podría ser el `id` del usuario a buscar.
* Con parámetros en query string (`/users?sort=name`)
* En el cuerpo de la petición (POST y PUT generalmente) (GET y DELETE no tienen body)
* También podemos recibirlos en la cabecera, pero esta zona solemos dejarla para información de contexto, como autenticación, formatos, etc.

### Recibiendo parámetros - en la ruta

Lo definimos en el argumento PATH de la ruta

```js
router.put('/ruta/:id', function(req, res) { console.log('params', req.params);
  var id = req.params.id;
});
```
La petición request `req` tiene una propiedad tipo objeto `params` que contiene todos los parámetros que se le pasen a la petición.

Un ejemplo de llamada podría ser:

**`PUT http://localhost:30003/apiv1/anuncios/55`**

*Podemos combinarlo con los otros*

### Recibiendo parámetros - en query string

Lo definimos en la query string

```js
router.put('/ruta', function(req, res) { 
  console.log('query-string', req.query); var id = req.query.id;
});
```
Aquí no hacemos nada especial en la definición de la ruta. La petición request `req` tiene una propiedad tipo objeto `query` que contiene todos los parámetros que se le pasen como query string a la petición. 

Un ejemplo de llamada podría ser:

**`PUT http://localhost:3000/apiv1/anuncios?id=66`**

*Podemos combinarlo con los otros*

### Recibiendo parámetros - en el body de la petición

Los recibimos en `req.body`. Esta forma no la podemos usar en GET ya que no usa body.

```js
router.put('/ruta', function(req, res) { 
  console.log('body', req.body); // body 
  var nombre = req.body.nombre;
});
```
Aquí tampoco hacemos nada especial en la definición de la ruta. La petición request `req` tiene una propiedad tipo objeto `body` que contiene todos los parámetros que se le pasen dentro del body a la petición. 

Un ejemplo de llamada podría ser:

**```PUT http://localhost:3000/apiv1/anuncios 
{nombre: 'Pepe'}```**

### Recibiendo parámetros

En el paso por ruta podemos usar **expresiones regulares** en los parámetros, incluir varios o hacerlos opcionales.

```js
router.put('/ruta/:id?', function...); // ? indica que es un parámetro opcional, podemos o no mandarlo 
// params { id: 'dato'}
router.put('/ruta/:id([0-9]+)', function...); // parámetro con regexp que indica que solo se manden números 
// params { id: '26'}
router.put('/ruta/:id([0-9]+)/piso/:piso(A|B|C)', function...); // varios 
// params { id: '26', piso: 'A' }
```
 El último ejemplo usa una expresión regular mñas compleja, que en caso de que el parámetro enviado coincida con ese patrón se ejecutara el midlware (función) definido.
 
## 50.- Respondiendo a peticiones (5:35)

Repasemos ahora las disintas formas de responder a las peticiones con los diferentes métodos existentes.

### Métodos de respuesta

Método | Descripción
-------|------------
`res.download()` |  Prompt a file to be downloaded.
`res.end()` | End the response process.
`res.json()` | Send a JSON response.
`res.jsonp()` | Send a JSON response with JSONP support.
`res.redirect()` | Redirect a request. 
`res.render()` | Render a view template.
`res.send()` | Send a response of various types. 
`res.sendFile` | Send a file as an octet stream. 
`res.sendStatus()` | Set the response status code and send its string representation as the response body.

Podemos ver su [documentación](https://expressjs.com/en/4x/api.html#res). Veamos los más usados...

### Métodos de respuesta - send

Para responder a una petición podemos usar el método genérico `res.send()`. 

El cuerpo de la respuesta puede ser un buffer, un string, un objeto o un array.
```js
res.send(new Buffer('whoop'));
res.send({ some: 'json' });
res.send('<p>some html</p>'); 
res.status(404).send('Sorry, we cannot find that!'); 
res.status(500).send({ error: 'something blew up' });
```
El `res.status(codigo).send('mensaje')` nos permite envíar un código de respuesta asociado al mensaje que se mande.

Express detecta el tipo de contenido y pone el **header Content-Type** adecuado. 

Si es un array o un objeto devuelve su representación en JSON.

### Métodos de respuesta - json

Podemos usar `res.json`, que ajusta un posible `null` o `undefined` para que salga bien en JSON.
```js
res.json(null)
res.json({ user: 'tobi' }) 
res.status(500).json({ error: 'message' })
```

### Métodos de respuesta - download

Transfiere el fichero especificado como un attachment. El browser debe solicitar al usuario que guarde el fichero.
```js
// el nombre del fichero es opcional
res.download('/report-12345.pdf', 'report.pdf');
```
`res.download` Recibe la ruta donde se encuentra el fichero y un nombre obcional con el que se descargara el fichero.

*Tipoco método que en el navegador va a abrir la ventana de guardar como para guardar ese fichero a descargar*.

### Métodos de respuesta - redirect

Podemos redireccionar al usuario ante una petición a otro sitio. 
Devuelve una redirección con el status code 302 por defecto (found). A menos que indiquemos otro como en el ejemplo.
```js
res.redirect('/foo/bar'); // relativa al root host name 
res.redirect('http://example.com'); // absoluta 
res.redirect(301, 'http://example.com'); // con status 
res.redirect('../login'); // relativa al path actual 
res.redirect('back'); // vuelve al referer
```

### Métodos de respuesta - render

Renderiza una vista y envia el HTML resultante. Acepta un parámetro opcional `locals` para dar variables locales a la vista.

```js
// render de la vista index
res.render('index');

// render de la vista user con el objeto locals
res.render('user', { name: 'Tobi' });
```
En el segundo ejemplo podríamos tener el objeto todas las propiedades del usuario y cuando cargue la vista `user` hacer que las muestre en el formulario.

### Métodos de respuesta - sendFile

Envía un fichero como si fuera un fichero estático, lo intentaría mostrar en el navegador.

Además de la ruta del fichero acepta un objeto de opciones y un callback para comprobar el resultado de la transmisión.

```js
var options = { 
  headers: {
    'x-timestamp': Date.now(),
    'x-sent': true 
  }
};

res.sendFile(fileName, options);
```

## 51.- Ejercicio: rutas parámetros y respuestas (11:16)

Abrir de nuestro proyecto **`express_generada_ejs`**.

* En la carpeta `routes` crear el archivo `clients.js`
* Insertar el siguiente código:
```js
"use strict";

var express = requiere('express');
var router = express.Router();

router.get('/', function(req, res, next){
   res.send('Respuesta a clients')
});

module.exports = router;
```
* Incluirlo en `app.js` con:

```js
...
var clientsRouter = require('./routes/clients');
...
app.use('/clients', clientsRouter);
```

* Probarlo ejecutando `nodemon` (Si ya estaba cargado es necesario reiniciar cuando se modifica `app.js`
* En el navegador cargar la URL `http://localhost:3000/clients`

<img src="/images/clients-navegador.png">

* Si intentamos pasar un parámetro con `http://localhost:3000/clients/5` tenemos:

<img src="/images/clients-navegador-error.png">

La ruta `http://localhost:3000/clients/5` no cuadra con ninguna de las rutas que hemos metido en express, por lo tanto express me dice que la ruta no se ha encontrado. 

* Vamos a definir la ruta con parámetros en `clients.js`:
```js
router.get('/:id', function(req, res, next) {
    res.send('Recibiendo parámetro id: ' + req.params.id);
});
```
* Intentamos nuevamente pasar un parámetro con `http://localhost:3000/clients/5` tenemos:

<img src="/images/clients-navegador-5.png">

* Un parámetro diferente con `http://localhost:3000/clients/7` tenemos:

<img src="/images/clients-navegador-7.png">

* Podríamos mostrar el parámetro en la consola:

```js
router.get('/:id', function(req, res, next) {
    console.log('Recibiendo parámetro id: ' + req.params.id);
    res.send('Recibiendo parámetro id: ' + req.params.id);
});
```

<img src="/images/clients-consola-7.png">

**Nota: Los `console.log()` que hagamos saldrán en la consola del servidor, no en la consola del navegador.**

* Introduzcamos una ruta para una petición POST en `clients.js`:

```js
router.post('/', function(req, res, next) {
    console.log('req.body ' + req.body);
    res.json({recibiendo: req.body.num});
});
```

* Usaremos **Postman** para probar esta opción poniendo:

<img src="/images/clients-postman-post.png">

<img src="/images/req-body-vacio.png">

Como no hemos mandado nada en el body nos regresa un JSON vacío.

** Mandemos `num` en el body:

<img src="/images/clients-postman-post-body.png">

<img src="/images/req-body-77.png">

Por último recibamos parámetro en la query string.

* Modificamos el método `get` para pintrar lo recibido en el query string:

```js
router.get('/', function(req, res, next) {
    console.log('Query String: ', req.query);
    console.log('Name: ', req.query.name);
    console.log('Lastname: ', req.query.lastname);
    res.send('Respuesta a clients');
});
```

* Cargar en el navegador el URL: `http://localhost:3000/clients?name=Leo&lastname=Messi`

<img src="/images/clients-navegador-query-string.png">

<img src="/images/consola-query-string.png">

### Código Final cliente.js

```js
"use strict";

var express = require('express');
var router = express.Router();

// No se mandan parámetros o se mandan como query string
router.get('/', function(req, res, next) {
    console.log('Query String: ', req.query);
    console.log('Name: ', req.query.name);
    console.log('Lastname: ', req.query.lastname);
    res.send('Respuesta a clients');
});

// Recibe parámetros como params
router.get('/:id', function(req, res, next) {
    console.log('Recibiendo parámetro id: ' + req.params.id);
    res.send('Recibiendo parámetro id: ' + req.params.id);
});

// Recibe parámetros enviados en el body
router.post('/', function(req, res, next) {
    console.log('req.body ', req.body);
    res.json({ recibiendo: req.body.num });
});

module.exports = router;
```

## 52.- Middlewares (7:57)

### Middlewares

Un Middleware es un handler que se activa ante unas determinadas peticiones o todas, antes de realizar la acción principal de una ruta.

Podemos poner tantos middlewares como nos hagan falta (para hacer logs, busquedas en BD, para autentícar, para responder a peticiones).

Una aplicación Express es esencialmente una serie de llamadas a middlewares, el último middleware responden a la petición.

### Middlewares

Tiene acceso al objeto de la petición (request), al objeto de la respuesta (response) y al siguiente middleware (next). Un middleware típico es:

```js
router.get('/', function(req, res, next) { }
```

**Si un middleware no quiere terminar una llamada debe llamar al siguiente next() para pasarle el control, de lo contrario la petición quedará sin respuesta, y el que la hizo (por ejemplo un browser) se quedará esperando hasta su tiempo límite de time-out.**

### Middlewares - tipos

Comúnmente establecemos middlewares de los siguientes tipos:

* **Application-level** —> todas las peticiones
* **Router-level** —> las peticiones a un router determinado 
* **Error-handling** —> controlan posibles errores de forma global 
* **Built-in** —> añaden funcionalidad estándar
* **Third-party** —> añaden funcionalidad de terceros

### Middlewares - Application-level

Se conecta al objeto instancia de la aplicación (app) con `app.use` o `app.METHOD`, por ejemplo:

```js
var app = express();

app.use(function (req, res, next) { 
  console.log('Time:', Date.now()); 
  next();
});
```

Los middlewares van colgados directamente en la app de Express, que atiende a todas las peticiones por que no estamos especificando un filtro de ruta, puede hacer cualquier cosa con `req`, `res` o `next`, en este caso para todas las peticiones escrive la fecha en que se lanza la petición y después llama a `next()` para que atienda esa petición. 

### Middlewares - Router-level

Se conecta a un router con `router.use` o `router.METHOD`, por ejemplo:

```js
var router = express.Router();

router.use(function (req, res, next) { 
  userModel.find(req.body.userId, function (user) {
    req.user = user; 
    next();
  }); 
});
```

En este caso estamos poniendo un middleware a un router que lo que hace es buscar un usuario por su id, lo que encuentra lo guarda en la petición `req` para que los middleware siguientes puedan usarlo.

### Middlewares - Error-handling

Los pondremos los últimos, tras todas nuestras rutas en el fichero `app.js`. 
Reciben un parámetro más `err` que se pone en primera posición.
Cuando uno de los middleware que se estan ejecutando pase a next un error, Express en lugar de dar el control al siguiente middleware que le toque, se saltara todos los middlewares que haya hasta llegar al middleware error y va a hacer lo que pongamos en su cuerpo. 

```js
app.use(function(err, req, res, next) { 
  console.error(err.stack); 
  res.status(500).send('Something broke!');
});

// en una ruta anterior podemos haber hecho next(err);
```

Podemos devolver lo que nos convenga, un JSON con un error, una página de error, un texto, etc.

### Middlewares - Built-in

Son middlewares estándar que proveen los mantenedores de Express.

Por ejemplo, `express.static`:

```js
app.use(express.static('public', options)); 
```

Se puede revisar una lista en su página de [github](https://github.com/senchalabs/connect?_ga=1.96605535.1731670228.1440846106%23middleware).

Es un repositorio de GitHub de **senchalabs** que son los que desarrollarón el módulo de Node que se llama **Conenect** usado por Express para gestionar las conexiones, donde se muestra un listado de los Midllware que se pueden considerar oficiales.

Hay una [lista de los más usados](http://expressjs.com/en/resources/middleware.html)

### Middlewares - Third-party (de terceros)

Son middlewares hechos por gente que los pública en [https://www.npmjs.com/](https://www.npmjs.com/) para que los demás los usemos, también nosotros podríamos desarrollar los nuestros y públicarlos.

Podemos instalarlos con npm y cargarlos como los anteriores.

`$ npm install cookie-parser`

```js
var cookieParser = require('cookie-parser');

// load the cookie parsing middleware
app.use(cookieParser());
```

Hay una [lista de los más usados](http://expressjs.com/en/resources/middleware.html)

## 53.- Ejercicio: un middleware sencillo (7:24)

Vamos a incluir un Middleware en nuestro archivo routes `clientes.js`:

* Añadir el middleware:

```js
...
router.use(function(req, res, next) {
    console.log('Middleware de router clientes');
});
...
```

* Ejecutar en el navegador la URL: `http://localhost:3000/clients`

Podemos ver en la consola del servidor el mensaje: `Middleware de router clientes`

<img src="images/consola-midellware.png">

Pero si observamos el navegador no termina de responder, se queda pensando. Si abrimos las herramientas del desarrollador y cargamos la pestaña **Network** vemos como nuestra petición se encuentra **pending** pendiente de responder. Esto pasa por que en nuestro middleware ni respondemos ni llamamos a next y esta a la espera que alguién responda cosa que no pasara como lo tenemos códificado. 

* Vamos a incluir `next()` en nuestro middleware:

```js
...
router.use(function(req, res, next) {
    console.log('Middleware de router clientes');
    next();
});
...
```

* Ejecutar nuevamente en el navegador la URL: `http://localhost:3000/clients`

Podemos ver en la consola del servidor el mensaje: `Middleware de router clientes`

Y el navegador termina de responder correctamente.

<img src="images/midellware-ok.png">

Este middleware solo funciona a nivel de cliente es decir con URLs que usen clients como:

* `http://localhost:3000/clients`
* `http://localhost:3000/clients/5`
* `http://localhost:3000/clients?name=John`
* etc.

A nivel de aplicación este middleware no funciona ya que lo definimos en el archivo router `clients.js`. Es decir que si cargamos la URL `http://localhost:3000` o `http://localhost:3000/users` el mensaje `Middleware de router clientes` no sale.

<img src="images/midellware-app-01.png">

Vamos a crear un middleware a nivel de aplicación en el archivo `app.js`.

* Incluir el siguiente código:

```js
app.use(function(req, res, next) {
    console.log('Middleware a nivel de app');
    next();
});
```

**NOTA: La definición del middleware debe estar antes de la definición de las rutas sino lo ignora**

* Si cargamos la URL `http://localhost:3000` o `http://localhost:3000/users` el mensaje `Middleware a nivel de app` ya sale. Incluso también sale para `http://localhost:3000/clients` junto con su propio mensaje:  

<img src="images/midellware-app-02.png">

Vamos a hacer que por lo que fuera en el middleware de aplicación no pudieramos continuar por que existe un error:

* Modificar el middleware para indicar que existe un error:

```js
app.use(function(req, res, next) {
    console.log('Middleware a nivel de app');
    next({status: 500, message: 'Imposible continuar ...'});
});
```

* Si cargamos la URL `http://localhost:3000`, `http://localhost:3000/users` o `http://localhost:3000/clients` tendremos:

<img src="images/midellware-500.png">

¿Por que lo pinta tan bonito? Cuando encuentra el error pasa lo siguiente.

* Salta todos los middleware que estan definidos después del que nos otros escribimos es decir todos estos:

```js
app.use('/', indexRouter);
app.use('/users', usersRouter);
app.use('/clients', clientsRouter);

// catch 404 and forward to error handler
app.use(function(req, res, next) {
    next(createError(404));
});
```

* Llega al middleware de error, el cual renderiza la página `error`:

```js
// error handler
app.use(function(err, req, res, next) {
    // set locals, only providing error in development
    res.locals.message = err.message;
    res.locals.error = req.app.get('env') === 'development' ? err : {};

    // render the error page
    res.status(err.status || 500);
    res.render('error');
});
```

* Si veo la vista de error `error.ejs`: 

```js
<h1><%= message %></h1>
<h2><%= error.status %></h2>
<pre><%= error.stack %></pre>
```

Veo que renderiza los valores que se le asignarón a nuestro objeto `res.locals` en el middleware de error, por eso nuestra página sale con ese diseño. El stack como no se lo pasamos no lo renderiza.
 
## 54.- Ejercicio: doble respuesta (3:27)

Para hacer este ejercicio vamos a

* Modificar el middleware en `app.js` para indicar que NO existe un error:

```js
app.use(function(req, res, next) {
    console.log('Middleware a nivel de app');
    next();
    //next({status: 500, message: 'Imposible continuar ...'});
});
```
* Al hacer la llamada a la URL `http://localhost:3000/clients` tenemos:

<img src="/images/midellware-clients.png">

Habíamos visto que en un middleware tenemos dos opciones:

* Pasamos a l siguiente middleware con `next()`
* O respondemos a la petición con `res.send('Algo...')``

Que pasaría si tenemos ambas cosas, modifiquemos el middleware en `clients.js`:

```js
router.use(function(req, res, next) {
    console.log('Middleware de router clientes');
    req.send('Respuesta');
    next();
});
```

<img src="/images/clients-2llamadas.png">

<img src="/images/error-2llamadas.png">

Por un lado en el navegador nos llega la respuesta hecha con `req.send('Respuesta');` pero por otro lado obtenemos un error en la consola.

El error es `Error [ERR_HTTP_HEADERS_SENT]: Cannot set headers after they are sent to the client` que nos dice `no se pueden establecer encabezados después de enviarlos al cliente`, esto es un claro indicador de que se esta respondiendo dos veces.

Como ya hemos visto ejecuta `req.send('Respuesta');` despues el `next()` que nos lleva al siguiente middleware:

```js
// No se mandan parámetros o se mandan como query string
router.get('/', function(req, res, next) {
    console.log('Query String: ', req.query);
    console.log('Name: ', req.query.name);
    console.log('Lastname: ', req.query.lastname);
    res.send('Respuesta a clients');
});
```

Ejecuta los `console.log`, pero cuando llega a `res.send('Respuesta a clients');` nos marca el error, ya que la respuesta la había realizado previamente.

El error se nos puede presentar por que nuestro código puede tener un error típico:

```js
router.use(function(req, res, next) {
    console.log('Middleware de router clientes');
    if(condicion){
       res.send('Respuesta');
    }
    next();
});
```

Para corregirlo tenemos dos opciones una es poner un `return`:

```js
router.use(function(req, res, next) {
    console.log('Middleware de router clientes');
    if(condicion){
       res.send('Respuesta');
       return;
    }
    next();
});
```

Al poner el `return` estaría finalizando el middeleware y ya no continuaria con el `next()` como en el caso de arriba, o bien se podría tener un `else`, pero en node no lo recomiendan mucho según el autor para evitar mucha identación, usan la estructura anterior, cuando se cumple una condición salen con un `return`.

## 55.- Vistas Templates (11:09)

Sistemas de plantillas (Templates) vamos a hablar de que son, como usarlos y como cambiarlos. 

### Templates

Además de servir HTML estático podemos usar sistemas de plantillas. Estas plantillas nos ayudarán a mezclar HTML o JS con datos que mandamos desde nuestros controladores.

Express por defecto monta **Jade**, podemos cambiarlo fácilmente, por ejemplo a **EJS** que es uno de los más usados.

### Templates

Hay muchos sistemas de templates en Javascript, pero los que cumplen con el estándar de Express están listados en:
[https://www.npmjs.com/package/consolidate#supported-template- engine](https://www.npmjs.com/package/consolidate#supported-template- engine)

### Templates

Para instalar un sistema de templates lo instalaremos con:

`npm install ejs --save`

y nos aseguramos de que nuestra aplicación lo usa con un par de settings:

* **views**, el directorio donde estarán nuestras plantillas, por ejemplo:

`app.set('views', './views')`

* **view engine**, el template engine a usar, por ejemplo: 

`app.set('view engine', 'jade')`

Express lo carga automáticamente y ya podremos usarlo.

### Templates - Jade

Ejemplo de **Jade**:

```jade
app.set('view engine', 'jade');

doctype html
html
  head
    title= title
    link(rel='stylesheet', href='/stylesheets/style.css')
  body
    p esto es un párrafo
```

Cuando **Jade** compila la plantilla anterior produce el siguiente HTML:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Express</title>
    <link rel="stylesheet" href="/stylesheets/style.css">
  </head>
  <body>
    <p>esto es un párrafo</p>
  </body>
</html>
```

### Templates - EJS

EJS añade su funcionalidad sobre HTML estándar.

Esto puede hacer que nos sea más fácil depurar errores o integrar y mantener una maquetación realizada por un maquetador especializado.

### Templates

Para proporcionar datos variables a las vistas le damos un objeto en la llamada render:

`res.render('index', { titulo: 'Anuncios' });`

Cuando indicamos la vista que queremos renderizar en este caso `ìndex`, añadimos un parámetro más, un objeto con todas las propiedades y valores que deseamos pasarle.

En la vista tendremos disponible el contenido de ese objeto.

`<title><%= titulo %></title>`
 
### Templates

Existen varias formas de renderizar los valores que son pasados a la vista.

* Para reemplazar datos usaremos esta notación:

`<title><%= titulo %></title>`
 
 Con el igual hago referencia a la variable para que pinte su contenido, dentro no puedo poner nada más.
 
### Templates - sin escapar

* El valor será escapado para evitar la inyección de código. Si queremos incluir HTML usaríamos `<%- %>`:

`<p><%- sinEscapar %></p>`

Con el signo - permite renderizar código HTML.
 
### Templates - include

Podemos incluir el contenido de otras plantillas con:

`<% include otra/plantilla %>`

views
  - index.js 
  - otra
    - plantilla.ejs

Esto es muy útil para hacerr páginas maestras, podemos hacer un include para el header y/o footer, etc.

### Templates - condiciones

Usar bloques de forma condicional es sencillo.

```html
<% if (condicion.estado) { %>
  <p><%= condicion.segundo %> es par</p>
<% } else { %>
  <p><%= condicion.segundo %> es impar</p>
<% } %>
```
 
 Estamos combinando código con HTML e impresión de variables.
 
### Templates - iterar
 
O por ejemplo iterar bucles.

```html
<% users.forEach(function(user){ %> 
  <p><%= user.name %></p>
<% }) %>
``` 

Si la vista recibe un array de usarios, los itera y por cada uno imprime su nombre en un parrafo.

### Templates - código

En resumen, entre los tags <% y %> (sin usar '=' o '-') podemos colocar cualquier estructura de código válida en Javascript.

Pero sin pasarnos. Esto es una vista, y meter código aquí significa que tenemos funcionalidad en las vistas.

Es recomendable tener toda o la mayor parte de la funcionalidad en los modelos y en los controladores.

### Templates

Podemos encontrar su documentación y más ejemplos en: [https://github.com/mde/ejs](https://github.com/mde/ejs)

## 56.- Ejercicio: vistas (9:19)

En nuestro proyecto tenemos la route `index.js` con el siguiente código:

```js
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
    res.render('index', { title: 'Express' });
});

module.exports = router;
```

Aquí vemos claramente como le estamos pasando un objeto con el valor para `title` a la vista `index.ejs`:

```html
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1><%= title %></h1>
    <p>Welcome to <%= title %></p>
  </body>
</html>
```

Vemos que es una plantilla EJS y que ocupa `<%= title %>` para renderizar el valor de `title` en varios sitios de la página.

* Cargar en el navegador la URL `localhost:3000`

<img src="/images/express-navegador-02.png">

Vamos añadir alguna propiedad más para pasarsela a la vista, en `index.js`:

```js
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
    res.render('index', {
        title: 'Express',
        description: '<p>Infraestructura web rápida, minimalista y flexible para Node.js</p>'
    });
});

module.exports = router;
```

En la vista `index.ejs` incluimos en código para que lo renderice:

```html
<!DOCTYPE html>
<html>
<head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
</head>
<body>
    <h1><%= title %></h1>
    <p>Welcome to <%= title %>
    </p>
    <%= description %>
    <%- description %>
</body>
</html>
```

<img src="/images/express-navegador-05.png">

Con ` <%= description %>` No esta inyectando el código lo esta pintando como tal, el código no se ejecuta aparece como un texto. Con `<%- description %>` si que inyectamos el código y lo renderiza sin problema.

Vamos a meter una condición en `index.js`: 

```js
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
    var segundo = (new Date()).getSeconds();

    res.render('index', {
        title: 'Express',
        description: '<p>Infraestructura web rápida, minimalista y flexible para Node.js</p>',
        condicion: {
            segundo: segundo,
            estado: segundo % 2 === 0
        }
    });
});

module.exports = router;
```

Para renderizarla métemos lo siguiente en la vista `index.ejs`:

```html
<!DOCTYPE html>
<html>
<head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
</head>
<body>
    <h1><%= title %></h1>
    <p>Welcome to <%= title %></p>
    <%= description %>
    <%- description %>
    <% if(condicion.estado) { %>
        <p><%= condicion.segundo %> es Par</p>
    <% } else { %>
        <p><%= condicion.segundo %> es Impar</p>
    <% } %>
</body>
</html>
```

<img src="/images/express-navegador-06.png">

<img src="/images/express-navegador-07.png">

Por último vamos a mandar un array de objetos en `ìndex.js` para usar un bucle:

Vamos a meter una condición en `index.js`: 

```js
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
    var segundo = (new Date()).getSeconds();

    res.render('index', {
        title: 'Express',
        description: '<p>Infraestructura web rápida, minimalista y flexible para Node.js</p>',
        condicion: {
            segundo: segundo,
            estado: segundo % 2 === 0
        },
        caracteristicas: [{ name: 'Aplicaciones web' }, { name: 'API' }, { name: 'Rendimiento' }, { name: 'LoopBack' }]
    });
});

module.exports = router;
```

Para renderizarlo métemos lo siguiente en la vista `index.ejs`:

```html
<!DOCTYPE html>
<html>
<head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
</head>
<body>
    <h1><%= title %></h1>
    <p>Welcome to <%= title %></p>
    <%= description %>
    <%- description %>
    <% if(condicion.estado) { %>
        <p><%= condicion.segundo %> es Par</p>
    <% } else { %>
        <p><%= condicion.segundo %> es Impar</p>
    <% } %>
    <h3>Características</h3>
    <ul>
       <% caracteristicas.forEach( function (car) { %>
          <li>
              <%= car.name %>
          </li>
       <% }) %>
    </ul> 
</body>
</html>
```

<img src="/images/express-navegador-08.png">

### Ejercicio - Listar módulos

Representar los resultados de la función versionModulos hecha previamente en una página, con los módulos de Express.
Los módulos cuya versión sea menor a 1.0 deberán salir en rojo. En que orden salen?

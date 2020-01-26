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
 
## 47.- Estructura y Rutas (9:34)
 
## 48.- Estáticos (1:18)
 
## 49.- Recibiendo parámetros (5:23)
 
## 50.- Respondiendo a peticiones (5:35)
 
## 51.- Ejercicio: rutas parámetros y respuestas (11:16)
 
## 52.- Middlewares (7:57)
 
## 53.- Ejercicio: un middleware sencillo (7:24)
 
## 54.- Ejercicio: doble respuesta (3:27)
 
## 55.- Vistas Templates (11:09)
 
## 56.- Ejercicio: vistas (9:19)

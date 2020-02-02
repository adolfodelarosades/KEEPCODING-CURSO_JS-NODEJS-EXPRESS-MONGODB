# 08 MongoDB

[Documentación](https://github.com/adolfodelarosades/KEEPCODING-CURSO_JS-NODEJS-EXPRESS-MONGODB/blob/master/pdfs/08_MongoDB.pdf)

## 60.- Introducción a MongoDB (4:40)

### Bases de datos - MongoDB

MongoDB en una base de datos no relacional sin esquemas, esto significa principalmente que:

* No tenemos JOIN, tendremos que hacerlo nosotros
* Cada registro podría tener una estructura distinta 
* Mínimo soporte a transacciones

A la hora de decidir que base de datos usar para una aplicación debemos pensar como vamos a organizar los datos para saber si nos conviene usar una base de datos relacional o no relacional.

### Bases de datos - MongoDB

Usar una base de datos como MongoDB puede darnos más rendimiento principalmente por alguna de estas razones:

* No tiene que gestionar transacciones
* No tiene que gestionar relaciones
* No es necesario convertir objetos a tablas y tablas a objetos ([Object- relation Impedance Mismatch](https://en.wikipedia.org/wiki/Object-relational_impedance_mismatch))
   
## 61.- Instalación de MongoDB (7:53)

* Ir a la página de [MongoDB](https://www.mongodb.com/es).
* Ir a la página de descargas para [MongoDB Community Server](https://www.mongodb.com/download-center/enterprise)
* Descargamos el archivo.
* El contenido lo metemos en un directorio
* Crear las carpetas `data\db`
* Creamos el archivo `start.sh` con el siguiente contenido:

```sh
bin/mongod --dbpath ./data/db
```
O podemos ejecutar eso desde el shell cada que queramos arrancar MongoDB.

* Hay que hacer que el archivo sea ejecutable con `chmod +x start.sh`
* Ejecutamos el archivo con `./start.sh`

<img src="/images/log-mongod.png">

Nos aparece un log de mongo en el cual nos dice que esta arrancando, la versión del servidor, y nos dice que esta esperando que alguien se conecte en el puero 27017 que es el puerto por defecto donde arranca MongoDB 

* Para parar el servidor de MongoDB simplemente presionamos CRTL+C en la ventana
* Si observamos la carpeta `data\db` veremos que ya contiene información
* Para arrancar el shell de mongo debemos ejecutar en ota terminal el comando `mongo`

### Otra forma de instalación de MongoDB mediante brew

Yo segui la siguiente lista de comandos para instalar MongoDB

<img src="/images/brew-install-mongodb.png">

## 62.- Uso básico MongoDB - Parte I (7:02)



### Bases de datos - MongoDB shell basics

Para acceder a la shell usaremos:

```sh
~/master/cursonode/mongodb-server/bin/mongo 
MongoDB shell version: 3.0.4
connecting to: test
>
```

O desde la carpeta donde lo tengamos instalado.

Desde aquí podremos podremos escribir comandos de MongoDB para:

* Añadir datos
* Modificar datos
* Consultar datos
* Eliminar datos
* Y mas...


### Bases de datos - MongoDB shell basics

Vamos a ver algunos comandos que podemos ejecutar:

Comando | Descripción
--------|------------
`show dbs` | Lista las bases de datos que tenemos en nuestro servidor.
`use <dbname>` | Usar la base de datos que indiquemos.
`show collections` | Mostrar las colecciones de la base de datos usada.
`show users` | Mostrar los usarios de la base de datos.
`db.agentes.find().pretty()` | Mostrar el contenido de la colección agentes en formato JSON.
`db.agentes.insert({name: "Brown", age: 37})` | Insertar un documento en la colección agente.
`db.agentes.remove({_id: ObjectId("55ead88991233838648570dd")})` | Eliminar un documento de la colección agente por su id
`db.agentes.update({_id: ObjectId("55eadb4191233838648570de")}, {$set: {age: 38}})` | Actualizar un documento de agentes -- cuidado con el $set! —-
`db.coleccion.drop()` | Borra toda la colección 
`db.agentes.createIndex({name:1, age:-1})` | Crea un indice por `name` y otro por `age`
`db.agentes.getIndexes()` | Nos indica que indices tiene la colección

Mas operaciones en la [referencia rápida a la shell de MongoDB](https://docs.mongodb.com/manual/reference/mongo-shell/)

## 63.- Uso básico MongoDB - Parte II (5:58)

<img src="/images/shell-01.png">

<img src="/images/shell-02.png">

<img src="/images/shell-03.png">

<img src="/images/shell-04.png">

## 64.- Filtros en MongoDB - Parte I (6:15)

Veamos otras formas de buscar documentos en nuestras colecciones.

### Bases de datos - MongoDB queries

Con el comando `find()` podemos consultar todos los documentos de una colección, adicionalmente podemos pasar un varios parametros para filtra la información que nos devuelve, los siguientes son algunos ejemplos:

Comando | Descripción
--------|------------
`db.agentes.find()` | Dame todos los documentos de la colección `agentes`
`db.agentes.find({ name : 'Smith'})` | Dame todos los documentos que tengan `name = 'Smith'` de `agentes`
`db.agentes.find({ _id : ObjectId("55eadb4191233838648570de")})` | Dame todos los documentos con ese `_id` de `agentes`
`db.agentes.find({ age: { $gt: 30}}) // $lt, $gte, $lte, ...` | Dame todos los documentos con `age > 30` de `agentes`
`db.agentes.find({ age: { $gt: 30, $lt: 40}});` | Dame todos los documentos con `age > 30 and age < 40` de `agentes`
`db.agentes.find({ name: { $in: [ 'Jones', 'Brown']}}) //$nin` | Dame todos los documentos con `name` que esten en el array
`db.agentes.find({ name: 'Smith', $or: [ { age: { $lt: 30}}, { age: 43 } ] })` | Dame todos los documentos que cumplan `'Smith' and ( age < 30 or age = 43)`
 
<img src="/images/shell-05.png">

## 65.- Filtros en MongoDB - Parte II (6:32)

### Bases de datos - MongoDB queries

Otros tipos de busquedas que podemos hacer es por subdocumentos.

```sh
// subdocuments
db.agentes.find({ 'producer.company': 'ACME'})

// arrays
db.agentes.find({ bytes: [ 5, 8, 9 ]}) // array exact 
db.agentes.find({ bytes: 5}) // array contain 
db.agentes.find({ 'bytes.0': 5}) // array position
```

[https://docs.mongodb.com/manual/reference/method/db.collection.find/index.html](https://docs.mongodb.com/manual/reference/method/db.collection.find/index.html)

[https://docs.mongodb.com/manual/tutorial/query-documents/](https://docs.mongodb.com/manual/tutorial/query-documents/)

<img src="/images/shell-06.png">

### Bases de datos - MongoDB queries

Ordenar (1 Ascendente | -1 Descendente):
```sh
db.agentes.find().sort({age: -1})`
```

Descartar resultados:
```sh
db.agentes.find().skip(1).limit(1) 
db.agentes.findOne({name: 'Brown'}) // igual a limit(1)
```

Contar:
```sh
db.agentes.find().count() // db.agentes.count()
```

<img src="/images/shell-07.png">
 
## 66.- Transacciones (2:52)

### Bases de datos - MongoDB transacción

Cuando hablamos de transacciones estamos hablando de formas de decirle a la base de datos de que agrupe operaciones de forma cuerente. Por ejemplo, buscar un cliente en el banco, verificar su saldo, decrementar su saldo en 100 euros, mostrar su saldo nuevo, todas estas operaciones se deberían ejecutar en una transacción. MongoDB cuenta con la siguiente instrucción: 

`findAndModify` es una operación atómica, lo que nos dará un pequeño respiro transaccional.

```sh
db.agentes.findAndModify({ 
   query: { name: "Brown"}, 
   update: { $inc: { age: 1}}
})
```

Lo busca y si lo encuentra lo modifica, no permitiendo que otro lo cambie antes de modificarlo.

## 67.- Full text search (3:01)

MongoDB tiene una potente capacidad de buscar texto. La busqueda de texto es algo que nos facilita mucho la vida cuando tenemos que buscar cadenas de texto en los distintos campos o atributos que tiene nuestro documento. 

### Bases de datos - MongoDB queries

**Full Text Search**

Crear índice por los campos de texto involucrados:

```sh
db.agentes.createIndex({title: 'text', lead: 'text', body: 'text'});
```

Para hacer la búsqueda usar:

```sh
db.agentes.find({$text:{$search:'smith jones'});
```

   
### Bases de datos - MongoDB queries

**Full Text Search**

Frase exacta:

```sh
db.agentes.find({$text:{$search:'smith jones "el elegido"'});
```

Excluir un término:

```sh
db.agentes.find({$text:{$search:'smith jones -mister'});
```

Más info:
[https://docs.mongodb.com/manual/text-search/](https://docs.mongodb.com/manual/text-search/)

[https://docs.mongodb.com/manual/tutorial/specify-language-for-text-index/](https://docs.mongodb.com/manual/tutorial/specify-language-for-text-index/)
 
## 68.- Ejercicio: uso desde Node.js con driver (8:44)

Ahora que conocemos un poco de MongoDB vamos a hacer una conexión a la base de datos desde NodeJS para ello utilizaremos el driver oficial de MongoDB aun que hay más drivers de la comunidad que se pueden utilizar. Esto ocurre con otros motores de base de datos.

La forma de hacer esta conexión es la siguiente:

```sh
$ npm install mongodb
```

```js
var client = require('mongodb').MongoClient;

client.connect('mongodb://localhost:27017/cursonode', 
function(err, db) {
  if (err) throw err; 
  db.collection('agentes').find({}).toArray(function(err, docs) {
    if (err) throw err; 
    console.dir(docs); 
    db.close();
  }); 
});
```

Vamos a implementar esto en nuestro código.

* En la carpeta `CURSONODE` crear la carpeta `mongodb_driver`
* Dentro de `mongodb_driver` crear el archivo `index.js`
* Desde la terminal entrar a la carpeta `mongodb_driver` con `cd mongodb_driver`
* Pulsamos el comando `npm init` (damos enter a todo), me creará el archivo `package.json`
* Instalamos MongoDB con `npm install mongodb --save`
* En `index.js` introducimos el siguiente código:

```js
"use strict";

var MongoClient = require('mongodb').MongoClient;

MongoClient.connect('mongodb://localhost:27017/', function(err, client) {

    const db = client.db('cursenode');

    if (err) {
        console.log(err);
        return process.exit();
    }
    console.log("Connected successfully to server");

    db.collection('agentes').find().toArray(function(err, docs) {
        if (err) {
            console.log(err);
            return process.exit();
        }
        console.log('DOCUMENTOS:', docs);
        client.close();
    });
});
```

<img src="/images/node-conexion-mongodb.png">

Para mas información [npm mongodb](https://www.npmjs.com/package/mongodb)


## 69.- Mongoose (4:35)

[Mongoose](https://mongoosejs.com/) es una herramienta que nos permite persistir objetos en MongoDB, recuperarlos y mantener esquemas de estos fácilmente.

Este tipo de herramientas suelen denominarse **ODM** (Object Document Mapper).
   
### Instalación de Mongoose

```sh
npm install mongoose --save
```

### Conectar a la Base de Datos con Mongoose

Conectar a la base de datos:

```js
var mongoose = require('mongoose'); 
var conn = mongoose.connection;

conn.on('error', console.error.bind(console, 'mongodb connection error:'));
conn.once('open', function() {
  console.info('Connected to mongodb.'); 
});
mongoose.connect('mongodb://localhost/diccionario');
```

Tenemos un objeto de conexión `conn` que recibe dos eventos de conexión `error` y `open`, `error` saltara cuando surga un error de conexión y el `open` saltara cuando se conecte a la base de datos. `on` y `once` nos dicen que estamos utilizando un `event emmiter`, `once` nos indica que solo salta el evento la primera vez, una sola vez, una vez colocados los `event emmiter` hacemos la conexión y ya saltara el evento que tenga que saltar.

### Utilizar Mongoose

Para utilizar Mongoose debemos definir un Modelo dentro de un Módulo, cargamos Mongoose, indicamos el esquema que tiene e indicarmos a Mongoose que cree un Modelo usando el esquema que definimos:

```js
var mongoose = require('mongoose');
var agenteSchema = mongoose.Schema({ 
    name: String,
    age: Number
});
mongoose.model('Agente', agenteSchema);
```

### ¿Qué podemos hacer con el Modelo

Podemos instanciar el modelo creado para crear nuevos objetos del tipo modelo. Este objeto creado por Mongoose tendrá unas propiedades y métodos que nos facilitan el persistirlo en la base de datos.

**Guardar un registro**:

```js
var agente = new Agente({name: 'Smith', age: 43});

agente.save(function (err, agenteCreado) {
   if (err) throw err;
   console.log('Agente ' + agenteCreado.name + ' creado');
});
```

En este caso se tiene un método `save` que cuando termina de guardar en la base de datos ejecutarara un callback, el cual recibe como parámetros un posible error `err` y el objeto ya persistido en la base de datos y podemos usarlos para mostrar información en la consola.
   
**Eliminar registros**:

```js
Agente.remove({ [filters] }, function(err) { 
  if (err) return cb(err);
  cb(null);
});
```

Aquí usamos un método de clase para eliminar.

Ver la documentación de [Mongoose](https://mongoosejs.com/) para más información.

## 70.- Ejercicio: modelos consultas - Parte I (9:55)

Para hacer este ejercicio utilizaremos el proyecto `express_generada_ejs/generada`:

* En la terminal entrar a la carpeta `express_generada_ejs/generada`  con `cd express_generada_ejs/generada`
* Arrancar la aplicación con `nodemon`
* Cargar el URL `http://localhost:3000/`

<img src="/images/aplicacion-3000">

Nuestra aplicación carga, vamos a hacer unos ajustes:

* Eliminar de la carpeta `routes` el archivo `users.js`
* Eliminar las referencias de `users` en `app.js`
* En `app.js` vamos a cambiar:
```js
...
var indexRouter = require('./routes/index');
var clientsRouter = require('./routes/clients');
...
app.use('/', indexRouter);
app.use('/clients', clientsRouter);
```

Por:

```js
app.use('/',        require('./routes/index'));
app.use('/clients', require('./routes/clients'));
```
 
Para ahorrarnos dos variables que no se usaban en ningún otro sitio.

* Comprobar que los URL `http://localhost:3000/` y `http://localhost:3000/clients` sigan funcionando y que `http://localhost:3000/users` ya no se cargue.

### Usar Mongoose en nuestro proyecto


Empecemos por instalar mongoose:

```sh
npm install mongoose --save
```

Se modifica nuestro archivo `package.json` para añadir la dependencia.

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
    "mongoose": "^5.8.11",
    "morgan": "~1.9.1"
  }
}
```

Una vez instalado continuemos con la conexión a la base de datos por medio de Mongoose:

* En la carpeta `generada` crear la carpeta `lib`
* En la carpeta `lib` crear el archivo `connectMongoose.js`
* Insertar el siguiente código:
```js
"use strict";

var mongoose = require('mongoose');

var db = mongoose.connection;

db.on('error', function(err) {
    console.log('ERROR: ', err);
});

db.once('open', function() {
    console.info('Conectado a MongoDB');
});

// Si se usa el puerto por default (27017) no es necesario ponerlo
mongoose.connect('mongodb://localhost/cursenode', { useNewUrlParser: true, useUnifiedTopology: true }); 
```
* Incluir la conexión en el archivo `app.js`, hay que observar que en `connectMongoose.js` no hemos exportado nada ya que cuando utilicemos los modelos no nos hara falta especificar la conexión en ningún momento. Incluyamos el siguiente código después de `var app = express();`:

```js
var app = express();

require('./lib/connectMongoose');
```

Con esta única línea que añadimos es suficiente para hacer la conexión a la base de datos mediante Mongoose, para comprobarlo, arrancar el servidor con `nodemon` tenemos que se dispara el evento `once('open'` por lo que sale el mensaje `Conectado a MongoDB`, con lo cual apenas se arranca el servidor ya estoy conectado a la base de datos:

<img src="/images/connect-mongoose.png">

Una vez hecha la conexión a la bas de datos vamos a crear una carpeta para los modelos.

* En la carpeta `generada` crear la carpeta `models`.
* En la carpeta `models` crear el archivo `Agente.js`.
* Incluir el siguiente código:

```js
"use strict";

var mongoose = require('mongoose');

// Crear Esquema 
var agenteSchema = mongoose.Schema({
    name: String,
    age: Number
});

// Crear Modelo
mongoose.model('Agente', agenteSchema);
```

El crear un Schema con los campos definidos nos va a permitir asegurarnos que todos los documentos que caen en esta colección tienen este esquema, Mongoose se encargara de ello, siempre y cuando los metamos utilizando Mongoose, por que si me voy a la base de datos y añado un documento que no tiene este esquema MongoDB me lo va a permitir, ¿Qué va a pasar cuando Mongoose intente recuperar ese documento? Lo recuperara, metera los datos que correspondan a las propiedades del esquema, pero el resto de datos no los gestionará por que no puede asociarlos. Tampoco exporto nada por que para usarlo basta poner `mongoose.model('Agente')`.

* Para que Mongoose conozca el modelo de Agente lo vamos a incluir en `app.js` :

```js
require('./lib/connectMongoose');
require('./models/Agente');
```

Una vez arrancado el servidor se conecta a la base de datos, y Mongoose conocerá los modelos que hemos definido y podemos utilizarlos en la aplicación.

### Creación del API

* Dentro de la carpeta `routes` crear la carpeta `apiv1`
* Dentro de `apiv1` vamos a crear el archivo `agentes.js`
* Introducir el siguiente código:

```js
"use strict";

var express = require('express');
var router = express.Router();
var mongoose = require('mongoose');
var Agente = mongoose.model('Agente');

router.get('/', function(req, res, next) {
    Agente.find().exec( function( err, list){
        if(err) {
            next(err);
            return;
        }
        res.json({ ok: true, list: list });
    });
});

module.exports = router;
```

* Cargar este router en nuestro `app.js`

```js
app.use('/apiv1/agentes', require('./routes/apiv1/agentes'));
```

* Cargar la URL `http://localhost:3000/apiv1/agentes`:

<img src="/images/api-agentes.png">

## 71.- Ejercicio: modelos consultas - Parte II (8:45)

Mongoose
Crear un método estático a un modelo:
agenteSchema.statics.deleteAll = function(cb) { Agente.remove({}, function(err) {
if (err) return cb(err);
cb(null); });
};
 © All rights reserved. www.keepcoding.io
   
Mongoose
Crear un método de instancia a un modelo:
agenteSchema.methods.findSimilarAges = function (cb) { return this.model('Agente').find({ age: this.age }, cb);
}
 © All rights reserved. www.keepcoding.io
   
Mongoose
Listando registros:
agenteSchema.statics.list = function(cb) { var query = Agente.find({}); query.sort('name');
query.skip(500);
query.limit(100);
query.select('name age');
return query.exec(function(err, rows) {
if (err) { return cb(err);}
return cb(null, rows); });
});
 © All rights reserved. www.keepcoding.io
 

 
## 72.- Ejercicio: modificaciones - Parte I (8:54)
 
## 73.- Ejercicio: modificaciones - Parte II (9:39)
 
## 74.- Mongoose métodos (1:49)
 
## 75. Ejercicio: metodos de modelo - Parte I (8:44)
 
## 76.- Ejercicio: metodos de modelo - Parte II (8:07)
 
## 76.1.- Para descargar


   
ENHORABUENA!
Express.js
    
© All rights reserved. www.keepcoding.io
  

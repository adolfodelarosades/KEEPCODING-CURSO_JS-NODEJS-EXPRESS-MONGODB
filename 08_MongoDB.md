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
`db.agentes.find()` | 1
`db.agentes.find({ name : 'Smith'})` | 2
`db.agentes.find({ _id : ObjectId("55eadb4191233838648570de")})` | 3
`db.agentes.find({ age: { $gt: 30}}) // $lt, $gte, $lte, ...` | 4
`db.agentes.find({ age: { $gt: 30, $lt: 40}});` | 5
`db.agentes.find({ name: { $in: [ 'Jones', 'Brown']}}) //$nin` | 6
`db.agentes.find({ name: 'Smith', $or: [ { age: { $lt: 30}}, { age: 43 } // 'Smith' and ( age < 30 or age = 43) ] })` | 7
   
### Bases de datos - MongoDB queries
// subdocuments
db.agentes.find({ 'producer.company': 'ACME'})
// arrays
db.agentes.find({ bytes: [ 5, 8, 9 ]}) // array exact db.agentes.find({ bytes: 5}) // array contain db.agentes.find({ 'bytes.0': 5}) // array position
http://docs.mongodb.org/manual/reference/method/db.collection.find/#db.collection.find http://docs.mongodb.org/manual/tutorial/query-documents/
   © All rights reserved. www.keepcoding.io
   
 
## 65.- Filtros en MongoDB - Parte II (6:32)
 
## 66.- Transacciones (2:52)
 
## 67.- Full text search (3:01)
 
## 68.- Ejercicio: uso desde Node.js con driver (8:44)
 
## 69.- Mongoose (4:35)
 
## 70.- Ejercicio: modelos consultas - Parte I (9:55)
 
## 71.- Ejercicio: modelos consultas - Parte II (8:45)
 
## 72.- Ejercicio: modificaciones - Parte I (8:54)
 
## 73.- Ejercicio: modificaciones - Parte II (9:39)
 
## 74.- Mongoose métodos (1:49)
 
## 75. Ejercicio: metodos de modelo - Parte I (8:44)
 
## 76.- Ejercicio: metodos de modelo - Parte II (8:07)
 
## 76.1.- Para descargar




 
 

   

 
   

   

Bases de datos - MongoDB queries
Ordenar:
db.agentes.find().sort({age: -1}) Descartar resultados:
db.agentes.find().skip(1).limit(1) db.agentes.findOne({name: 'Brown'}) // igual a limit(1)
Contar:
db.agentes.find().count() // db.agentes.count()
 © All rights reserved. www.keepcoding.io
   
Bases de datos - MongoDB queries
Full Text Search
Crear índice por los campos de texto involucrados:
db.agentes.createIndex({title: 'text', lead: 'text', body: 'text'});
Para hacer la búsqueda usar:
db.agentes.find({$text:{$search:'smith jones'});
 © All rights reserved. www.keepcoding.io
   
Bases de datos - MongoDB queries
Full Text Search
Frase exacta:
db.agentes.find({$text:{$search:'smith jones "el elegido"'}); Excluir un término:
db.agentes.find({$text:{$search:'smith jones -mister'});
Más info:
https://docs.mongodb.com/v3.2/text-search/ https://docs.mongodb.com/v3.2/tutorial/specify-language-for-text-index/
   © All rights reserved. www.keepcoding.io
   
Bases de datos - MongoDB transacción
findAndModify es una operación atómica, lo que nos dará un pequeño respiro transaccional.
db.agentes.findAndModify({ query: { name: "Brown"}, update: { $inc: { age: 1}}
})
Lo busca y si lo encuentra lo modifica, no permitiendo que otro lo cambie antes de modificarlo.
 © All rights reserved. www.keepcoding.io
   
Bases de datos - MongoDB
Ejemplo de uso MongoDB:
$ npm install mongodb
var client = require('mongodb').MongoClient;
client.connect('mongodb://localhost:27017/cursonode', function(err, db) {
if (err) throw err; db.collection('agentes').find({}).toArray(function(err, docs) {
if (err) throw err; console.dir(docs); db.close();
}); });
ejemplos/db/mongodb
 © All rights reserved. www.keepcoding.io
   
Mongoose
  © All rights reserved. www.keepcoding.io
   
Mongoose
Mongoose es una herramienta que nos permite persistir objetos en MongoDB, recuperarlos y mantener esquemas de estos fácilmente.
Este tipo de herramientas suelen denominarse ODM (Object Document Mapper).
  © All rights reserved. www.keepcoding.io
   
Mongoose
Instalación como siempre:
npm install mongoose --save
 © All rights reserved. www.keepcoding.io
   
Mongoose
Conectar a la base de datos:
var mongoose = require('mongoose'); var conn = mongoose.connection;
conn.on('error', console.error.bind(console, 'mongodb connection error:'));
conn.once('open', function() {
console.info('Connected to mongodb.'); });
mongoose.connect('mongodb://localhost/diccionario');?
  © All rights reserved. www.keepcoding.io
   
Mongoose
Crear un modelo:
var mongoose = require('mongoose');
var agenteSchema = mongoose.Schema({ name: String,
    age: Number
});
mongoose.model('Agente', agenteSchema);
 © All rights reserved. www.keepcoding.io
   
Mongoose
Guardar un registro:
var agente = new Agente({name: 'Smith', age: 43});
agente.save(function (err, agenteCreado) {
if (err) throw err;
console.log('Agente ' + agenteCreado.name + ' creado');
});
 © All rights reserved. www.keepcoding.io
   
Mongoose
Eliminar registros:
Agente.remove({ [filters] }, function(err) { if (err) return cb(err);
cb(null);
});
 © All rights reserved. www.keepcoding.io
   
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
   
ENHORABUENA!
Express.js
    
© All rights reserved. www.keepcoding.io
  

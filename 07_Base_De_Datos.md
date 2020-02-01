# Bases de Datos

## Bases de Datos

**Node.js**, a través de módulos de terceros, se puede conectar casi con cualquier base de datos del mercado, ver la [documentación](http://expressjs.com/es/guide/database-integration.html#integraci%C3%B3n-de-la-base-de-datos).

Basta con cargar el driver (módulo) adecuado y establecer la conexión.

```sh
$ npm install mysql

$ npm install mongoskin
```

## MySQL

Por ejemplo con MySQL:

```js
var mysql = require('mysql');
var connection = mysql.createConnection({
   host : 'localhost',
   user : 'usuario',
   password : 'pass',
   database : 'cursonode'
});

connection.connect(); // callback opcional

connection.query('SELECT * from agentes', function(err, rows, fields) {
   if (err) throw err;
   console.log(rows);
});
```

Después de haber instalado el driver `mysql` podemos utilizarlo en nuestro codigo:

* Requerimos `mysql`
* Establecemos una conección pasando un objeto con los valores necesarios (`host`, `user`, `password` y `database`) 
* Nos conectamos, podemos usar un callback por ejemplo para indicar un mensaje de conexión establecida correctamente
* Lanzamos queries, en el callback recuperamos los datos o algún posible error si ocurriera

### Aprender SQL

Para refrescar conceptos de SQL:

[https://www.sqlteaching.com/](https://www.sqlteaching.com/)

## ORMs

Un **ORM** (Object Relational Mapping / Mapeo Relacional con Objetos) se encarga principalmente de:

* Convertir objetos en consultas SQL para que puedan ser persistidos en una base de datos relacional. Por ejemplo si tengo un objeto cliente me lo puede traducir a un insert o update en sql.

* Traducir los resultados de una consulta SQL y generar objetos. Por ejemplo el resultado de un select de clientes me lo traduce en un array de objetos clientes.

Esto nos resultará útil si el diseño de nuestra aplicación es orientada a objetos (OOP).

Un ORM muy usado para bases de datos SQL es [Sequelize](https://sequelize.org/).

Sequelize tiene soporte àra MySQL, MariaDB, SQLite, PostgreSQL y MSSQL.

Otras buenas alternativas son [Knex](http://knexjs.org/) y [Bookshelf](https://bookshelfjs.org/)

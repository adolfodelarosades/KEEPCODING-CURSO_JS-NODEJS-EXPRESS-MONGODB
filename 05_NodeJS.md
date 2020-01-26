# 5. Node.js

## 36.- Process (2:07)

* `process.platform`
* `process.exit(int)`
* `process.on('exit', callback)`
* `process.nextTick(callback)`

El objeto global **process** tiene muchas propiedades que nos serán útiles, como **`process.platform`**, que nos dira en que sistema operativo o plataforma estamos, que en OS X nos dirá **'darwin'**, en linux **'linux'**, etc.

También tiene métodos útiles como **`process.exit(int)`** que forzara a nuestra aplicación a terminar este en el estado que este y devolvera al sistema operativo un **exit code** (código de retorno) se suele establecer a cero cuando queremos indicar que hemos terminado sin errores o a un número diferente de cero para indicar que ha habido algun error.

O eventos como **`process.on('exit', callback)`** donde process funciona como un *emisor de eventos* colocariamos un callback en estos eventos para realizar cosas que queremos que ocurran ante esos eventos, en el ejemplo tenemos el evento `exit`.

Tiene un método interesante, `process.nextTick`, que recibe un solo argumento, un callback.
```js
process.nextTick(function() {
  console.log('Siguiente vuelta del evento loop, whooouuu!');
});
```
Lo que hará es colocar la función al principio de la siguiente vuelta del event loop. Retrasa la ejecución de la función hasta la siguiente vuelta del event loop.

## 37.- Ejercicio: process (7:09)

Dentro de nuetra carpeta `cursonode`:

* Crear el archivo `process.js`
* Incluir el siguiente código:
```js
"use strict";

// Información de proceso
var info = {
    pid: process.pid
}

console.log('Entro en ', info);
```
* Ejecutar con `nodemon process`
La salida que tenemos es:

<img src="/images/process01.png">

Vemos el `{ pid: 34826 }` es el número de proceso que tiene el sistema operativo asociado a mi proceso. Si lo vuelvo a ejecutar obtendre un nuevo `pid`.

<img src="/images/process02.png">

Vamos a obtener algo más de información con `process`:
```js
"use strict";

// Información de proceso
var info = {
    pid: process.pid,
    version: process.version,
    platform: process.platform,
    title: process.title,
    argumentos: process.argv,
    execPath: process.execPath,
    carpeta: process.cwd()
}

console.log('Entro en ', info);
```

<img src="/images/process03.png">

`process.version` Nos indica la versión de Node que se esta ejecutando.
`process.platform` Nos indica en que sistema operativo se esta ejecutando Node
`process.title` Nos indica quien ejecuta el proceso
`process.argv`  Nos indica todos los argumentos que recibe el proceso
`process.execPath` Nos indica desde donde se ejecuta Node
`process.cwd()` Nos indica la carpeta donde se encuentra mi script

Vamos a probar el *Emisor de Eventos* que tiene Node:

```js
process.on('exit', function() {
    console.log('Adios. Antes de terminar me despido.');
});

console.log('Fin del Proceso');
```
La salida es:

```sh
Fin del Proceso
Adios. Antes de terminar me despido.
```
Lo que sucede aquí que se encuentra con un *Emisor de Eventos* de salida que enviará un mensaje cuando se salga de la aplicación, llega a la última instrucción que envia el mensaje `Fin del Proceso` y después se ejecuta el callback que estaba pendiente de ejecutar del *Emisor de Eventos* mandando el mensaje `Adios. Antes de terminar me despido.`.

Vamos a forzar un final de la aplicación:

```js
process.on('exit', function() {
    console.log('Adios. Antes de terminar me despido.');
});

console.log('Previo a forzar la salida');

process.exit(0); // Esto termina la ejecución

console.log('Posterior a forzar la salida');
```
La salida es:
```
Previo a forzar la salida
Adios. Antes de terminar me despido.
```

Como podemos observar todas las instrucciones posteriores a `process.exit(0);` ya no se ejecutaran.

### Código final
```js
"use strict";

// Información de proceso
var info = {
    pid: process.pid,
    version: process.version,
    platform: process.platform,
    title: process.title,
    argumentos: process.argv,
    execPath: process.execPath,
    carpeta: process.cwd()
}

console.log('Entro en ', info);

process.on('exit', function() {
    console.log('Adios. Antes de terminar me despido.');
});

console.log('Previo a forzar la salida');

process.exit(0); // Esto termina la ejecución

console.log('Posterior a forzar la salida');
```
La salida es:

<img src="/images/process04.png">

## 38.- Event loop (3:29)

Node usa un solo hilo. *Conceptualmente Node usa un solo hilo puede ser que internamente use alguno más, pero para nuestros ojos usará un solo hilo. Mientras este hilo de ejecución este ejecutando algo dejamos de antender a otro tipo de peticiones.*

Node tiene un bucle interno, que podemos llamar 'event loop' donde e cada vuelta ejecuta todo lo que tiene en esa 'fase', dejando los callbacks pendientes para otra vuelta. *Node internamente tiene un buccle interno, que podemos llamar 'event loop' donde va ejecutando código **sincrono**, ejecutando todo el código sincrono que puede hasta que lo termina de ejecutar, una vez que ha terminado comprueba si tiene algún callback pendiente de resolver, da otra vuelta al 'event loop' comprueba los callbacks pendientes, sino tiene un callback que haya terminado da otra vuelta en el 'event loop', no hay nigun callback que haya terminado da otra vuelta en el 'event loop', en el momento que haya un callback que haya terminado Node comprueba en esa vuelta del 'event loop' que si hay algo terminado y ejecuta ese código, cuando termine ese código da otra vuelta en el 'event loop' y sigue resolviendo calbacks así sucesivamente.*

La siguiente vuelta mira haber si a terminado algún callback y si es así ejecuta su handlers.

El equivalente de esta construcción:

```js
process.nextTick(function() {
   console.log('Siguiente vuelta del event loop, whooouuu!');
});
```
Es como decir a Node "Cuando vuelvas a comprobar los callbacks finalizados haz esto!"

### Non blocking

Si Node se quedara esperando hasta que termine una query o una petición a Facebook, acumularía demasiados eventos pendientes y dejaría de atender a las siguientes peticiones, ya que como dijimos **usa un solo hilo**.

*Por lo tanto nuestro código debería estar estructurado de tal manera que no bloque o bloque lo menos posible ese hilo, por tanto si tenemos que hacer trabajos pesados no los hagamos todos seguidos o a la vez por que dejariamos de atender otras peticiones. 
Por ejemplo si nosotros hicieramos las consultas a la BD o las escrituras o lecturas a disco de forma sincrona no usaramos funciones asíncronas, bloqueariamos ese hilo de ejecución mientras esta leyendo el fichero de disco sin poder hacer otras cosas, con lo cual se quedaría el hilo parado hasta que se resolviera esa lectura, eso no nos interesa preferimos que mientras esta haciendo la lectura en BD o en el File System pueda ir haciendo otras cosas y cuando termine que nos avice el sistema operativo y nuestr 'event loop' cazaría ese aviso y ejecutaría el callback que hubiesemos programado*

Por eso todos las llamadas a funciones que usan IO (por ejemplo escritura o lectura en disco, la red, bases de datos, etc) se hacen de forma asíncrona. Se aparcan para que nos avisen cuando terminen.

## 39.- EventEmitter (1:08)

El EventEmitter es un objeto muy potente de Node. Usando un EventEmitter podemos colgar eventos a un identificador. Por ejemplo:

```js
eventEmitter.on('llamar telefono', suenaTelefono);

function suenaTelefono() {
   ...
}
```
Ejecutara la función `suenaTelefono` cuando ocurra el evento nombrado con el identificador `llamar telefono`.

Y podemos emitir el identificador cuando queremos que sus eventos 'salten'
```js
eventEmitter.emit('llamar telefono');
```
Ejecutaría todos los callbacks que esten colgados de este identificador.

Incluso podemos darle argumentos al identificador para que se los dé a los manejadores.
```js
eventEmitter.emit('llamar telefono', 'madre');
```

Nuestro manejador recibirá el parámetro.
```js
var suenaTelefono = funtion(quien){
   ...
}
```

## 40.- Ejercicio: EventEmitter (6:31)
 
Dentro de nuetra carpeta `cursonode`:

* Crear el archivo `eventemitter.js.`
* Incluir el siguiente código:
```js
"use strict";

var events = require('events');
var myEmitter = new events.EventEmitter();

myEmitter.on('llamar telefono', sonarTelefono);
myEmitter.on('llamar telefono', vibrarTelefono);

function sonarTelefono() {
    console.log('Ring Ring Ring');
}

function vibrarTelefono() {
    console.log('Brrrr Brrrr Brrr');
}

myEmitter.emit('llamar telefono');
```
La salida es:

<img src="/images/emitter01.png">

Explicando el código:

`var events = require('events');` Requerimos la librería 'events'.
`var myEmitter = new events.EventEmitter();` Creamos un emisor de eventos llamado `myEmitter`
`myEmitter.on('llamar telefono', sonarTelefono);` Al identificador `llamar telefono` le colgamos la función `sonarTelefono`
`myEmitter.on('llamar telefono', vibrarTelefono);` A `llamar telefono` le colgamos una segúnda función `vibrarTelefono`
Definimos las dos funciones:
```js
function sonarTelefono() {
    console.log('Ring Ring Ring');
}

function vibrarTelefono() {
    console.log('Brrrr Brrrr Brrr');
}
```
`myEmitter.emit('llamar telefono');` Emitimos el evento `llamar telefono` lo que implicara que se ejecuten las funciones (callbacks) asociadas el evento, es decir `sonarTelefono` y  `vibrarTelefono` como vemos en la salida.

Añadiendo un parametro:
```js
"use strict";

var events = require('events');
var myEmitter = new events.EventEmitter();

myEmitter.on('llamar telefono', sonarTelefono);
myEmitter.on('llamar telefono', vibrarTelefono);

function sonarTelefono(quien) {
    if (quien == 'Madre')
        console.log('Tilin Tilin Tilin');
    else
        console.log('Ring Ring Ring');
}

function vibrarTelefono() {
    console.log('Brrrr Brrrr Brrr');
}

myEmitter.emit('llamar telefono', 'Madre');
myEmitter.emit('llamar telefono', 'Novia');
```
La salida es:

<img src="/images/emitter02.png">

Explicación del código:

`myEmitter.emit('llamar telefono', 'Madre');` Al emitir el evento `llamar telefono` añadimos el parámetro `Madre` el cual es recibido por las funciones asociadas, en este caso:
```js
function sonarTelefono(quien) {
    if (quien == 'Madre')
        console.log('Tilin Tilin Tilin');
    else
        console.log('Ring Ring Ring');
}
```
Lo que permite cambiar el tono cuando nos llame nuestra Madre y en cualquier otro caso dejar el  tono por default. La vibración será igual para todos los casos ya que en:
```js
function vibrarTelefono() {
    console.log('Brrrr Brrrr Brrr');
}
```
No estamos manipulando el parámetro.

### Buscar en la Documentación Oficial de Node

Si no sabemos como se llama la librería o queremos más información vamos a la [Documentación de Node](https://nodejs.org/es/docs/) entramos al [API](https://nodejs.org/dist/latest-v12.x/docs/api/) y en las opciones que salen seleccionamos [Events](https://nodejs.org/dist/latest-v12.x/docs/api/events.html) y dentro tenemos [Class:EventEmitter](https://nodejs.org/dist/latest-v12.x/docs/api/events.html#events_class_eventemitter) y podemos observar toda la información sobre como usar `EventEmitter`.

## 41.- Módulos (7:29)

Los módulos nos permiten estructurar nuestras aplicaciones y hacerlas crecer sin complicarnos demasiado.

Los módulos de Node.JS se basan en el estándar **CommonJS**.

* Quien quiere usar un módulo lo carga con **`require`**.
* La instrucción `require` es **síncrona**.
* Opcionalmente, los módulos usan **`exports`** para exportar cosas.
* Los módulos son singleton.

Node carga un módulo una sola vez. En el primer require.

**Los siguientes require al mismo módulo devolverán el mismo objeto que se exportó en la primera llamada**.

Esto nos vendrá muy bien con las conexiones a bases de datos, por ejemplo. *Cuando metemos el código de conexión a la BD en un módulo lo requerimos una vez, se establece la conexión, y las siguientes veces que hagamos un requiere de esa conexión, no se volvera a realizar una nueva conexión sino que reutilizaremos la conexión anterior*.

Ejemplo de un módulo básico:

```js

// modulo.js
console.log('Hola desde un módulo!');

// index.js
require('./module.js');
```
Con esto cargamos y ejecutamos el contenido del modulo `modulo.js` desde `index.js`.

Ejemplo que exporta algo:
```js
// suma.js
module.exports = function(a, b) {
   return a +b;
}

// index.js
var suma = require('./suma)';
```
En el archivo `suma.js` estamos creando un `module.exports` que tiene asociada una función anonima.
En el archivo `index.js` declaramos la variable `var suma = require('./suma)';`  con lo que el contenido de `suma` será igaul a la función anónima declarada en el módulo.


### Donde busca Node.js los módulos.

Los módulos son buscados en el siguiente orden hasta que lo encuentre.

1. Si empieza por `./` o `/` o `../` (algo que esta en el File System) va a la ruta calculada desde la situación del fichero actual.
2. Si es un módulo del core como `fs`, `events` o `path`
3. Módulos en la carpeta **node_modules** local
4. Módulos en la carpeta **node_modules** global

En nuestro código podemos llegar a tener situaciones como:

`require('../../../../../lib/files/modulo.js'`  (Ruta relativa)

**¿Cómo evitar esto? Asignando **NODE_PATH** a la ruta donde metemos nuestra librería en el arranque de nuestra app.

**NODE_PATH=lib** node index.js

`require("files/modulo.js"); // mejor así!`

Otra forma de hacer algo parecido es si usamos **npm** a partir de 2.0 podemos anotarlas en el `package.json`:
```js
{
   "name": "baz",
   "dependencies": {
      "bar": "file:../foo/bar"
   }
}
```

[Más info sobre Local Paths§](https://docs.npmjs.com/files/package.json#local-paths)

### ¿module.exports o exports?

* `exports` es un alias de `module.exports`.
* Node lo crea automáticamente
* Para exportar objetos se pueden usar los dos indistintamente

Si asignamos algo directamente a `exports` deja de ser un alias. Solo podemos usarlo con **`exports.loquesea`**

### npmjs.com

Principalmente podemos encontrar módulos de terceros en [npmjs.com](https://www.npmjs.com/). Podemos encontra módulos de procesar ficheros, de crear strings aleatoriamente, de críptografia, de gráficos, etc. Inclusive podemos publicar nuestro propio módulo si creemos que puede ser interesante para la comunidad.

## 42.- Ejercicio: haciendo módulos (3:49)

Dentro de nuetra carpeta `cursonode`:

* Crear la carpeta `modulos`
* Dentro de `modulos` crear el archivo `suma.js.`
* Incluir el siguiente código:
```js
```


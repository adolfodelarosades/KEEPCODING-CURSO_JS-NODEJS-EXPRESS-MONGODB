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

#### Código final
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
 
## 39.- EventEmitter (1:08)
 
## 40.- Ejercicio: EventEmitter (6:31)
 
## 41.- Módulos (7:29)
 
## 42.- Ejercicio: haciendo módulos (3:49)

# 1. Introducción a Node.js
 
## 1.- Introducción (1:49)

:+1:
 
## 2.- ¿Qué es Node.js? (11:42)

### ¿Qué es Node.js? 

* **Es un interprete de JavaScript**
* **Inicialmente para correr en servidores:** Actualmente ya se usa en muchas otras plataformas, por ejemplo para ejecutar programas como GRUNT, GULP, WEBPACK, etc. También podemos hacer aplicaciones de escritorio por ejemplo con ELECTRON
* **Asíncrono:** Determinadas cosas ocurren o terminan en un momento distinto del tiempo, de cuando las hemos lanzado.
* **Orientado a eventos**
* **Basado en el motor V8 de Google**

### ¿Orientado a evento?

* **En la programación secuencial es el programador quien decide el flujo.** El orden en que se ejecutan los pasos es totalmente secuencial.
* **En la programación orientada a eventos, el usuario o los programas clientes son quienes definen el flujo.**  Aquí no determinamos el orden en que van a ocurrir las cosas, es el usuario de la aplicación el que determinará que se va a ir ejecutando según las acciones que seleccione en la aplicación, (presionar botones, scroll, enlaces, etc.).
   
### Servidores de aplicaciones

* En otros lenjuajes se utiliza un *Servidor de Aplicaciones* donde cargamos la nuestra, como **Tomcat**, **IIS**, etc.
* También hay múltiples lenguajes que se usan como una extensión de un *Servidor Web* como **Apache**, **nginx**, etc., que es quien maneja las conexiones, como por ejemplo **PHP**.
* **En Node.js nuestra aplicación realiza directamente todas las funciones de un servidor Web o de Aplicacaciones**.

### Motor V8

* Motor JavaScript creado por Google para Chrome.
* Escrito en C++
* Multiplataforma (Win, Linux, Mac)
* Muy rápido

### Versiones de Node.js

* Versiones 4.x -> LTS (Long Term Service) **12.4.1 LTS**
* Versiones 5.x -> Estables, con las últimas novedades **13.7.0 Actual**
* A partir de la versión 4.0 se incorporan muchas de las nuevas características de ECMAScript versión 6.

**Node.js empezo por el año 2011**

### Límite de memoria

* Actualmente, por defecto tiene un límite de memoria de 512 MB en sistemas de 32 bits, **1GB en sistemas de 64 bits**.
* El límite se puede aumentar mediante el establecimiento de `--max_old_space_size` un máximo de `~ 1,024 (1GiB) (32 bits) y ~ 1,741 (1.7GiB) (64 bits)`, pero se recomienda dividir el proceso en varios workers si se llega a los límites.
   
### Ventajas de Node.js

Node.js tiene grandes ventajas reales en:

* Aplicaciones de red, como APIs, servicios en tiempo real, servidores de comunicaciones, etc.
* Aplicaciones cuyos clientes están hechos en JavaScript, pues compartimos código y estructuras entre el servidor y el cliente.
 
## 3.- Instalando Node.js (9:44)

### Distintas formas

Se puede hacer:

* Desde el instalador oficial [node.js.org](https://nodejs.org/es/)
* Con un instalador de paquetes
* Compilando manualmente

### Distintas formas - ¿Como decido?

Instalador: 

* Más sencillo de instalar

Homebrew (Instalador de paquetes de MacOSX) Chocolate (Windows)

* Se instala sin `sudo`
* Más fácil de desinstalar

### Instalador

* Ir a [node.js.org](https://nodejs.org/es/)
* Intall -> descargar
* Lanzar .pkg | .exe
* Siguiente, siguiente, ...

Solo para OSX y Windows

Instalará los ejecutables de **node** y **npm** en la ruta: `/usr/local/bin`

Pondrá ña carpeta `node_modules` global en: `/usr/local/lib`

Vínculo dinámico de **npm** en **bin**: `npm@ -> ../lib/node_modules/npm/bin/npm-cli.js`

A partir de aquí ya podremos ejecutar:

`node -v`: Para ver la versión de NodeJS.

`npm -g --depth=0`: Para ver la lista de paquetes instalados.

<img src="/images/node-version.png">

### A través del instalador - actualizar

Para actualizar se repite el proceso:

* Descargar el instalador de la versión elegida
* Instalar encima
* Comprobar la nueva versión

### A través del instalador - desinstalar

Se ha de hacer manualmente

Hay varios scripts que nos ayudan, por  ejemplo:

`https://gist.github.com/nicerobot/2697848`

**Nota importante:** Siempre que usemos un script de un tercero, **revisar su código y entender lo que hace!** antes de ejecutarlo. 

### A través de un instalador de paquetes

Pre-requisitos:

* XCode
* Homebrew [http://brew.sh/](http://brew.sh/)

Instalación:

1. Abre el Terminal
2. Escribe: `brew install node`

Trans la instalación comprobar las versiones: `node -v`

### A través de un instalador de paquetes - actualizar

Actualización:

1. Abre el Terminal
2. Escribe: `brew update`
3. Luego: `brew upgrade node`

Tras la actualización comprueba las versiones.

### A través de un instalador de paquetes - desinstalar

Desinstalación

1. Abre el Terminal
2. Escribe: `brew uninstall node`

### io.js junto con node.js

**io.js** se acaba de mergear recientemente con **node.js** para producir una única rama de desarrollo.

Si aún así qusiéramos tener instalado conjuntamente **node.js v0.12** con **node.js v4.0**, podemos usar por ejemplo [nvm](https://github.com/nvm-sh/nvm) para que nos ayude a gestionarlo.

### nvm

**nvm** Node Version Manager, nos permite tener varias versiones de NodeJS en nuestra máquina.

Instalar

* En sistemas linux usar `sudo npm install -g nvm`
* En windows usar `https://github.com/coreybutler/nvm-windows/releases`

```sh
nvm list

nvm installl <version>

nvm use <version>

nvm use system # en linux
```

Podemos tener hasta 7 versiones diferentes instaladas pero solo una activa.

## 4.- Ejercicio: un servidor básico (15:28)

Para crear nuestro servidor HTTP utilizaremos una librería del core de Node que se llama `http` que nos permite  no tener que programar los detalles de este protocolo, simplemente utilizaremos la librería que ya sabe hablar `http` con un cliente.

En nuestra carpeta de trabajo crear el archivo `servidor_basico.js`.

* Ir a `servidor_basico.js`
* Introducir el siguiente có
* Ir a `servidor_basico.js`digo:
```js
var http = require('http');

var server = http.createServer(function(request, response) {
    response.writeHead(200, {'Content-Type': 'text/plain'});
    response.end('Wake up, Neo...\n');
});

// Arrancamos el servidor
server.listen(1337, '127.0.0.1');
console.log('Servidor arrancado en 127.0.0.1:1337');
```

Vamos a explicar un poco el que es lo que estta haciendo este código:

`var http = require('http');`: Requerimos o importamos el módulo `http` de los modulos de Node, con esto ya tenemos el módulo disponible para usar.

`var server = http.createServer()`: Aquí le estamos diciendo a `http` que nos cree un servidor y que lo almacene en la variable `server`. 

`var server = http.createServer(function(request, response) {`: El servidor que nos va a crear recibirá peticiones que intentara resolver mediante la función que tiene como parámetro (callback) la cual a su vez tiene los parámetros `request` y `response`.

`response.writeHead(200, {'Content-Type': 'text/plain'});`: En el cuerpo de la función usaremos `response` para añadir nuestra respuesta, lo primero es indicar el código de retorno de nuestra respuesta y el tipo de respuesta que vamos a mandar, esto se hace en la cabecera.

`response.end('Wake up, Neo...\n');` Aquí indicamos el contenido de la respuesta.

Con esto ya tenemos preparado nuestro servidor que responde peticiones enviando el texto `Wake up, Neo...\n`. Cualquier petición que llegue responderá con un código 200, un texto plano que dice `Wake up, Neo...\n`.

`server.listen(1337, '127.0.0.1');` Lo siguiente que debemos hacer es arrancar el servidor, para lo cual debemos indicarle un puerto que no estemos usando en este caso el `1337` y también debemos indicarle a que ip debe engancharse ese servidor para arrancar, usaremos la `127.0.0.1` que significa `localhost`, nuestra maquina local.

`console.log('Servidor arrancado en 127.0.0.1:1337');`: Una vez arrancado el servidor queremos que salga un mensaje que lo indique.

Con todo esto ya tenemos listo nuestro servidor para arrancar.

### Ejecutar

Existen dos formas diferentes para poder ejecutarlo:

```sh
$ node index.js
```

Con **nodemon**:

```sh
$ npm install nodemon -g
$ nodemon
```

### Ejecutar nuestro ejemplo en Node

Para ejecutar nuestro ejemplo en **Node**:

* Abrir una Terminal.
* Ejecutar el comando: 
```sh
node servidor_basico.js
```
<img src="/images/levantar-servidor-terminal.png">

Para ejecutar nuestro ejemplo desde el navegador:

* Abrir la URL `127.0.0.1:1337` o `localhost:1337` en el navegador Chrome.

<img src="/images/levantar-servidor-navegador.png">
<img src="/images/levantar-servidor-localhost.png">

Podemos realizamos cambios para que me responda html:

```js
var http = require('http');

var server = http.createServer(function(request, response) {
    response.writeHead(200, { 'Content-Type': 'text/html; charset=UTF-8' });
    response.end('<h1>Wake up, Neo...</h1>');
});

// Arrancamos el servidor
server.listen(1337, '127.0.0.1');
console.log('Servidor arrancado en 127.0.0.1:1337');
```

Si quiero ver los cambios en el navegador aun que actualice el navegador no los veo, esto es por que es necesario **reiniciar el servidor**, una vez hecho esto veremos los cambios.

<img src="/images/levantar-servidor-html.png">

Por lo que **cada vez que yo haga algún cambio en el código tengo que parar y volver a arrancar el servidor**, para evitar hacer esto usaremos **nodemon** el cual monitoriza nuestro archivo de código fuente y cada que nosotros lo guardemos reiniciara el servidor.

### Intalar nodemon

Usar el comando: 

```sh
sudo npm install nodemon -g
```
<img src="/images/install-nodemon.png">

**npm** recoge del repositorio todas las dependencias que tiene este paquete para descargarlas e instalarlas.

### Ejecutar nuestro ejemplo con nodemon

Para ejecutar nuestro ejemplo con **nodemon**:

* Abrir una Terminal.
* Ejecutar el comando:
```sh
nodemon servidor_basico.js
```
<img src="/images/nodemon-execute.png">

Lo que nos indica **nodemon** con los mensajes que nos muestra es que esta monitorizando toda la carpeta por lo cual, cualquier cambio que se haga en los archivos dentro de la carpeta provocará que **nodemon** reinicie el servidor.

Por lo que si hacemos algún cambio en nuestro archivo `servidor_basico.js` y lo salvamos provocara que el servidor se reinicie inmediatamente.

<img src="/images/nodemon-reexecute.png">

Y si vamos al navegador y recargamos la página veremos los cambios.

<img src="/images/navegador-change.png">

## 5.- NPM (5:35)

**Node Package Manager** es un gestor de paquetes que nos ayuda a gestionar las dependencias de nuestro proyecto.

Entre otras cosas nos permite:

* Instalar librerías o programas de terceros
* Eliminarlas
* Mantenerlas actualizadas

Generalmente se instala conjuntamente con **NodeJS** de forma automática.

### NPM - package.json

Se apoya de un fichero llamado `package.json` para guardar el estado de las librerías.

Lo crearemos con el comando: `npm init`

El fichero `package.json` guarda información que necesita `npm` para saber que librerías tiene instaladas, sus versiones, 


[La documentación de este fichero](https://docs.npmjs.com/files/package.json)

Cuando ejecutemos el comando `npm init` se nos realizarán algunas preguntas, por ejemplo:

* Nombre de la aplicación
* La versión de nuestra aplicación
* Descripción de la aplicación
* Punto de entrada para ejecutarla
* Y más cosas ...

Finalmente todo se guardara en el archivo `package.json` que tendra una estructura JSON, con todos los datos que le dimos al pulsar el comando `npm init`.

#### Como se instalar una librería (ej. chance)

Para instalar cualquier librería adicional usaremos el comando:

```sh
npm install chance --save
```
Con este `--save` le estamos diciendo que apunte en el archivo `package.json` una dependencia de esa librería indicando la versión que usa:

```js
"dependencies": {
  "chance": "^0.8.0"
}
```

### NPM - global o local

Instalación local, en la carpeta del proyecto

`npm install <paquete> [--save]`

Instalación global (en `usr/local/lib/node_modules`)

`npm install -g <paquete>`

Si el paquete tiene ejecutables hará un vínculo a ellos en `/usr/local/bin`

### NPM - modificadores de versión

Cada librería lleva un identificador de versión
```js
"dependencies": {
  "chance": "^0.8.0"
}
```
NPM las parsea usando [Semver](https://github.com/npm/node-semver), aquí podemos buscar **Caret Range** para ver la información detallada de como debemos utilizar esta característica, así como tambieén el uso de otros modificadores para indicarle a `npm` como queremos que actualice nuestra librería cuando vaya a hacerlo.

Por defecto se establece un **Caret Range** (^):

* Ejemplos **"^1.2.3" "^0.2.5" "^0.0.4"
* Allows changes that do not modify the left-most non-zero digit in the [major, mino, patch] tuple.

Nos permite indicar como debe actualizar las versiones de nuestros paquetes.

## 6.- Ejercicio: npm y package.json (7:20)

Vamos a crear la carpeta `ejemplo_npm`

* Ir a la carpeta `cd ejemplo_npm`
* Pulsar el comando `npm init`

Nos preguntara varias cosas:

<img src="/images/npm-init.png">

Al contestar todas las preguntas y seleccionar `yes` dentro de nuestra carpeta `ejemplo_npm` nos creara el archivo `package.json` con la siguiente información:

```js
{
  "name": "ejemplo_npm",
  "version": "1.0.0",
  "description": "Esto es un ejemplo de un fichero package.json",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Adolfo de la Rosa",
  "license": "MIT"
}
```

### Instalar la librería chance

Para instalar la librería `change` y la almacene de forma local pulsamos el comando:

```sh
npm install chance --save
```
<img src="/images/install-chance.png">

La dependencia se ha descargado e instalado por lo que se ha modificado el archivo `package.json` para incluirla:
```js
{
  "name": "ejemplo_npm",
  "version": "1.0.0",
  "description": "Esto es un ejemplo de un fichero package.json",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Adolfo de la Rosa",
  "license": "MIT",
  "dependencies": {
    "chance": "^1.1.4"
  }
}
```
Si observamos la carpeta `ejemplo_npm` vemos que se ha incluido la carpeta `node_modules` que dentro contiene la carpeta `chance` que entre otras cosas contiene su propio archivo `package.json` y todo el código fuente necesario para utilizar esta librería.

<img src="/images/package-chance.png">

Si en lugar de instalar el paquete de forma local lo instalaramos de forma global no se instalaría en esta carpeta `node_modules` sino en la carpeta `/usr/local/bin` como lo hicimos con `nodemon`.

<img src="/images/nodemon-global.png">

Vemos que hay una entrada `nodemon` y que apunta a `../lib/node_modules/nodemon/bin/nodemon.js`

Por lo que si en mi `path`tengo incluida la carpeta `/usr/local/bin` yo puedo ejecutar el comando `nodemon` en cualquier momento y se ejecutara el archivo `../lib/node_modules/nodemon/bin/nodemon.js` que se instalo de manera global. Por eso funcionan las librerías que se instalan globalmente por que se estan incluyendo en el `path` y las puedo ejecutar desde cualquier ruta.

Podemos ver más detalles de todo lo que incluye el archivo `package.json`y mejorar nuestra librería en [La documentación de este fichero](https://docs.npmjs.com/files/package.json)


## 6.1.- Para Descargar

[PDF de la sección](https://github.com/adolfodelarosades/KEEPCODING-CURSO_JS-NODEJS-EXPRESS-MONGODB/blob/master/pdfs/Seccion01.pdf)

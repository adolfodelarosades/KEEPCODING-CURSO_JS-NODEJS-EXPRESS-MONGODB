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











 
## 5.- NPM (5:35)
 
## 6.- Ejercicio: npm y package.json (7:20)
 
## 6.1.- Para Descargar

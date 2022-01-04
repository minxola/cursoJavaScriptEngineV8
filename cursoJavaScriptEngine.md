# Curso de JavaScrip Engine (V8) y el navegador

**Diego de Granda**

## Historia de JavaScript

- JavaScript nace para dar funcionalidad a la web.
- Fue creado por Brendan Eich.
- Antes de crear JavaScript creó Mocha para NetScape. 
- JavaScript nace en 1995.
- De JavaScript Microsoft crea JScript
- El 1997 ECMA empieza a generar los estandares
- V8 nace en el 2008
- 2009 nace Node.js
- 2010 empiezan a surgir los frameworks
- En 2015 sale ES6

### ECMAScript

Es una especificación estandarizada por ECMA International. Fue creado para estandarizar JavaScript y para ayudar a fomentar múltiples implementaciones independientes.

## Como funciona el JavaScript Engine

JavaScript Engine es el motor de JavaScript y corre en el navegador. JavaScript Engine se encarga de interpretar o compilar el código JavaScript, transformándolo en lenguaje de máquina, el cual podrá ser entendido por la computadora. A este proceso se le conoce como **Just in time compiler** o compilación en tiempo real.

## V8: Motor de JavaScript de Chrome

V8, es el JavaScript Engine del navegador Chrome, es Open Source y fue creado por Google, escrito en C++.

No es el único motor de JavaScript que existe, muchos navegadores tienen implementado su propio JavaScript Engine.

V8, está siendo adoptado por diversos navegadores, debido a la rápida evolución, desarrollo y estabilidad de este motor. Chrome, Opera y Edge usan V8.

V8 nace para mejorar la experiencia de usuario en Google Maps. Actualmente V8 ayuda a crear muchas aplicaciones web.

Node.js también usa el V8 en el backend.

## Profundizando en el Engine

### Global Environment

Cuando nuestro código JavaScript corre en el Engine, se genera el **Global Environment** o **Entorno Global**.

Dentro del entorno global encontramos:

- El objeto global que en el navegador es `window`.
- Variable `this`, que depende del contexto, pero en el navegador es *window*
- Contexto de ejecución o **Excecution Context**.

El contexto de ejecución se realiza mediante la ejecución del código mediante un `stack de tareas`

- Parser del documento, donde encuentra las claves principales (keywords)

  > Parse, significa analizar y convertir un programa en un formato interno que un entorno de ejecución puede correr. Ejemplo, el motor de JavaScript dentro de los navegadores.

- AST (Abstract Syntax Tree), arbol de sintaxis abstracta. [AST Explorer](www.astexplorer.net)

- Interpreter, que transforma a bytecode, que es un código parecido a asembly, portatil y ejecutado por una virtual machine.

  > El interpreter intenta optimizar el código mediante el Profiler (Monitor), que lo compila y lo regresa como bytecode optimizado.
  >
  > Es en esta parte donde la maquina o el motor de JavaScript al tratar de optimizar se genera el **Hoisting**, que solo sucede con variables y funciones.

## Ejemplo de Objeto Global y Hoisting

### Objeto Global

Cuando abrimos el navegador, se genera un objeto global llamado `window`, este objeto global tiene todas las API que están listas para ejecutar nuestro código JavaScript.

En el navegador, la variable de referencia `this` es igual a `window`.

```js
window == this; //true
```

### Hoisting

El hoisting solo se genera en variables y funciones. Es cuando el Motor de JavaScript trata de interpretar u optimizar el código, generando así algunos errores. 

Para evitar estos errores por el hoisting es mejor primero declarar y asignar variables, así como definir las funciones, para luego al final ejecutarlas.

```js
//Hoisting en variables
console.log(name); //Undefined
var name = 'Diego';
```

En el ejemplo, JavaScript asume que se esta intentando llamar una variable, por lo que decide asignarle un espacio en memoria pero con el valor `undefined`.

```js
//Usando `const` no se genera hoisting
console.log(name); //name is not defined
const name = 'Diego';
```

Para el caso de las funciones, si ejecutamos primero la función y luego la definimos, JavaScript si hace hoisting, sin embargo en ese caso no se genera error.

## Memory Heap

Synchronous: Single Thread. JavaScript solo puede hacer una tarea a la vez.

### Memory Heap

Es el lugar donde se almacenan las variables, objetos y funciones.

Los objetos en JavaScript (objetos, arrays, funciones y básicamente todo lo que no sea un valor primitivo) se almacenan en la parte de memoria que de llama Memory Heap. Los valores primitivos son almacenados en el Call Stack, dentro del Scope (Contexto de Ejecución de la función que tenga acceso a esa variable). Acceder al Call Stack es mucho más rápido que al Heap. Además, en el Call Stack también se guardan las referencias, “como si fueran valores primitivos”. Cuando se asigna una variable a otra y esta apunta a un objeto, se copia la referencia, como si fuera un valor primitivo. Si el objeto tiene atributos como un número por ejemplo, este se guarda en la posición de memoria reservada para ese objeto. Los objetos también pueden tener más objetos dentro. En ese caso, dentro de la posición de memoria de ese objeto se va a guardar una referencia a otra posición de memoria

## Call Stack

Pila de tareas a realizar. JavaScript solo hace una tarea a la vez (Synchronous), en la parte baja está el *Objeto global* y por sobre esta se colocan las demás tareas, las cuales se van ejecutando desde la ultima a la primera, siendo el *Objeto Global* el que queda al final.

- JavaScript tiene un solo subproceso con un contexto de ejecución global, esto significa que JavaScript maneja una sola pila de llamadas(Call Stack) y un Memory Heap, que hace referencia a la parte de la memoria no estructurada donde se guardan los objetos y funciones. Las funciones del Call Stack se procesarán en el orden en que se llama, comúnmente conocido como Last-In, First-Out (LIFO).
- El motor de JavaScript utiliza una pila de llamadas(Call Stack) para administrar los contextos de ejecución: el contexto de ejecución global y los contextos de ejecución de funciones. La pila de llamadas funciona según el principio LIFO, es decir, el último en entrar es el primero en salir.
- Cuando ejecuta un script, el motor JavaScript crea un contexto de ejecución global(En la base de la pila reposa el Global Object) lo coloca en la parte superior de la pila de llamadas(Call Stack).
- Siempre que se llama a una función, el motor de JavaScript crea un contexto de ejecución de función para la función, lo coloca en la parte superior de la pila de llamadas(Call Stack) y comienza a ejecutar la función.
- Si una función llama a otra función, el motor de JavaScript crea un nuevo contexto de ejecución de función para la función que se está llamando y lo coloca en la parte superior de la pila de llamadas(Call Stack).
- Cuando se completa la función actual, el motor de JavaScript la saca de la pila de llamadas(Call Stack) y reanuda la ejecución donde la dejó en la última lista de código.
- El script se detendrá cuando la pila de llamadas esté vacía.

#### Desbordamiento de pila

La pila de llamadas tiene un tamaño fijo, dependiendo de la implementación del entorno de host, ya sea el navegador web o Node.js.

Si el número de contextos de ejecución supera el tamaño de la pila, se producirá un desbordamiento de la pila.

Por ejemplo, cuando ejecuta una función recursiva que no tiene condición de salida, resultará en un error de desbordamiento de pila:

## Garbage Collection

Los lenguajes de bajo nivel, como C, tienen primitivos de bajo nivel como `malloc() `y `free() `para la gestión de memoria. Por otro lado, para los valores en JavaScript se reserva memoria cuando"cosas" (objetos, strings, etc.) son creados y "automáticamente" liberados cuando ya no son utilizados. El proceso anterior es conocido como *Recolección de basura (garbage collection).* Su forma "automática" es fuente de confusión, y da la impresión a los desarrolladores de JavaScript (y de otros lenguajes de alto nivel) de poder ignorar el proceso de gestión de memoria. Esto es erróneo. 

[Ciclo de vida de memoria](https://developer.mozilla.org/es/docs/Web/JavaScript/Memory_Management)

Sin importar el lenguaje de programación, el ciclo de memoria es casi siempre parecido al siguiente:

1. Reservar la memoria necesaria
2. Utilizarla (lectura, escritura)
3. Liberar la memoria una vez ya no es necesaria.

El primer y el segundo paso son explícitos en todos los lenguajes. El último paso es explícito en lenguajes de bajo nivel, pero es mayormente implícito en lenguajes de alto nivel como JavaScript

**1. Reserva de memoria en JavaScript**

Para no agobiar al programador con reservas de memoria, JavaScript las realiza al mismo tiempo que la declaración de los valores.

En ocasiones, al llamar a una función se reserva memoria para un objeto.

**2. Usando valores**

Usar un valor es simplemente leerlo o escribirlo en memoria reservada. Esto puede ocurrir al leer o escribir el valor de una variable o de una propiedad de un objeto, inclusive pasando un argumento a una función.

**3. Liberar memoria cuando ya no es necesaria**

Los lenguajes de alto nivel incluyen una herramienta de software conocida como "recolector de basura" *(garbage collector),* cuya función es rastrear las reservas de memoria y su utilización, para así encontrar cuándo cierta parte de la memoria ya no es necesaria, y en su momento liberarla automáticamente.

**Recolección de basura (Garbage collection)**

La noción principal de los algoritmos de recolección se basan en la noción de *referencia*. Dentro del contexto de gestión de memoria, se dice que un objeto hace referencia a otro si el primero tiene acceso al segundo (ya sea de forma implícita o explícita). Por ejemplo, un objeto de JavaScript guarda una referencia a su prototipo (referencia implícita) y a cualquiera de los valores de sus propiedades (referencia explícita)

**Métodos de recolección de basura**

- Conteo de referencias
- Algoritmo Mark-and-sweep (marcado y barrido)

### [Algoritmo Mark-and-sweep (Marcado y barrido)](https://developer.mozilla.org/es/docs/Web/JavaScript/Memory_Management#algoritmo_mark-and-sweep_(marcado_y_barrido))

Este algoritmo reduce la definición de "un objeto ya no es necesitado" a "un objeto es inalcanzable"

Este algoritmo asume la noción de un grupo de objetos llamados *objetos raíz* (en JavaScript la raíz es el objeto global). Periódicamente el recolector empieza por estas raíces, encuentra todos los objetos que están referenciados por estas raíces, y luego todos los objetos referenciados de estos, etc. Empezando por las raíces, el recolector de esta forma encontrará todos los objetos que son *alcanzables* y recolectará los objetos inalcanzables.

Este algoritmo es mejor que el anterior ya que "un objeto tiene cero referencias" equivale al "objeto es inalcanzable". Esto no sucedía asi en el algoritmo anterior cuando se trataba de un ciclo.

Desde el 2012, todos los navegadores incluyen un recolector de basura basado en mark-and-sweep. Todas las mejoras realizadas en el campo de Recolección de basura en JavaScript (recolección generacional/incremental/concurrida/paralela) en los ultimos años son mejoras a la implementación del algoritmo, pero no mejoras sobre el algoritmo de recolección ni a la reducción de la definicion de cuando"un objeto ya no es necesario".

**En resumen**:

**Garbage collecction** es el proceso de rastrear los “desechos” y limpiar la memoria(Mark and Sweep) para evitar un **overstack**.
**Mark and sweep** es el proceso en el que marca(mark) los espacios de memoria no utilizados en el heap y los elimina(sweep).

## Stack Overflow

El **call stack** es la lista de tareas, donde en la base se encuentra el **Global Object**. Este call stack tiene un tamaño limitado ya sea por el servidor o el navegador.

Todas las tareas del código se van apilando sobre el global object, que luego de ejecutarse se van evacuando (garbage collection).

Puede ocurrir que una función desborde la pila de tareas, a esto se le conoce como `stack overflow`. La lista de tareas del call stack desborda su 'tamaño máximo'.

Cuando ocurre el Stack Overflow, anteriormente el navegador se cerraba, sin embargo, actualmente los navegadores detectan cuando una función puede desbordar la pila y detienen la ejecución de dicha función.

## JavaScript Runtime

El JavaScript Runtime Enviroment (JRE) es aquel que proporciona características adicionales a nuestra aplicación (clic con el mouse, información del navegador, solicitudes HTTP, etc) en el tiempo de ejecución, además es el responsable de hacer que JavaScript sea *Asíncrono* y *no bloqueante*.

<img src=".\jsRuntime.png" alt="JS Runtime" style="zoom:67%;" />

El JRE contiene los siguientes componentes:

- Motor JavaScript (Memory Heap, Call Stack)
- Web APIs (DOM, AJAX, Times, etc)
- Callback Queue (cola de devolución o cola de mensajes)
- Job Queue
- Event Loop (bucle de eventos)

## Asincronía

JavaScript por default corre una tarea a la vez –> Sincronismo
Memory Heap: Espacio donde se guardan funciones y variables.
Call Stack: Donde se apilan todas las tareas que tenemos que hacer con Javascript
Web API's (Ofrecidas por el navegador para manipular lo siguiente)

- DOM(document)
- AJAX(XMLHttpRequest)
- Timeout(setTimeout)

Call Back Queue: El orden en que se van a ejecutar a funciones
Al momento de usar asincronismo sacamos funciones del Call Stack que no serán ejecutadas por javascript y serán ejecutadas por el navegador (mediante Web API's), luego cuando el navegador tenga listas estas tareas delegadas, los colocará en el Call Back Queue según el orden que van llegando, luego el Event Loop, se encargará de enviarlas al Call Stack cuando este haya finalizado la pila de tareas.

**Ejemplos**

Al ejecutar en la consola del navegador lo siguiente:

```js
console.log("taco 1");
console.log("taco 2");
console.log("torta");
console.log("taco 3");
```

Se obtendría como resultado:

```js
//taco 1
//taco 2
//torta
//taco 3
```

En este caso no hay asincronismo, sino que todas las tareas se realizan una a una conforme aparecen en el código.

Sin embargo, al ejecutar este código en la consola del navegador, usando `setTimeout()` para en caso de la torta:

```js
console.log("taco 1");
console.log("taco 2");
setTimeout(()=>{
    console.log("torta");
}, 1000);
console.log("taco 3");
```

Se obtendría:

```js
//taco 1
//taco 2
//taco 3
//torta
```

Esto debido a que cuando el call stack llega a la función setTimeout, debido al tiempo asignado, esta es sacada de la pila y pasada al navegador para que la realice, continuando luego con la siguiente tarea de la pila. Una vez el navegador, evalúa la función setTimeout, lo envía al callback queue, quedando a la espera para que en console.log('torta') mediante el Event Loop sea pasado nuevamente a la pila de ejecución, donde será ejecutado siempre y cuando el call stack haya terminado de realizar sus tareas.

En el siguiente ejemplo, así el tiempo asignado sea 0 segundos, la función setTimeout, enviará dicha tarea al Callback Queue, hasta que las demás tareas del Call Stack se ejecuten, finalmente el Event Loop la pasará nuevamente al Call Stack para su ejecución.

```js
console.log("taco 1");
console.log("taco 2");
setTimeout(()=>{
    console.log("torta");
}, 0);
console.log("taco 3");
/*
Se obtendrá como resultado:
*/
//taco 1
//taco 2
//taco 3
//torta
```

Haciendo una analogía quedaría así para el caso de una taquería:

🌮 - **call stack** : *el taquero (órdenes rápidas)*
👨‍🍳 - **web APIs** : *la cocina*
🌯 - **callback queue** : *las órdenes preparadas*
💁‍♂️ - **event loop** : *el mesero*



**Never Stop Learning!**

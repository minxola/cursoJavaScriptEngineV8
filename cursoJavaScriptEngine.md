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












# Curso de JavaScrip Engine (V8) y el navegador

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

## JavaScript Engine

JavaScript Engine es el motor de JavaScript y corre en el navegador. JavaScript Engine se encarga de interpretar o compilar el código JavaScript, transformándolo en lenguaje de máquina, el cual podrá ser entendido por la computadora. A este proceso se le conoce como **Just in time compiler** o compilación en tiempo real.

## V8: Motor de JavaScript de Chrome

V8, es el JavaScript Engine del navegador Chrome, es Open Source y fue creado por Google, escrito en C++.

No es el único motor de JavaScript que existe, muchos navegadores tienen implementado su propio JavaScript Engine.

V8, está siendo adoptado por diversos navegadores, debido a la rápida evolución, desarrollo y estabilidad de este motor. Chrome, Opera y Edge usan V8.

V8 nace para mejorar la experiencia de usuario en Google Maps. Actualmente V8 ayuda a crear muchas aplicaciones web.

Node.js también usa el V8 en el backend.

### Global Environment

Cuando nuestro código JavaScript corre en el Engine, se genera el **Global Environment** o **Entorno Global**.

Dentro del entorno global encontramos:

- El objeto global que en el navegador es `window`.
- Variable `this`, que depende del contexto, pero en el navegador es *window*
- Contexto de ejecución o **Excecution Context**.

El contexto de ejecución se realiza mediante la ejecución del código mediante un stack de tareas.

- Parser del documento, donde encuentra las claves principales (kewords)

  > Parse, significa analizar y convertir un programa en un formato interno que un entorno de ejecución puede correr. Ejemplo, el motor de JavaScript dentro de los navegadores.

- AST (Abstract Syntax Tree), arbol de sintaxis abstracta. [AST Explorer](www.astexplorer.net)

- Interpreter, que transforma a bytecode, que es un código parecido a asembly, portatil y ejecutado por una virtual machine.

  > El interpreter intenta optimizar el código mediante el Profiler (Monitor), que lo compila y lo regresa como bytecode optimizado.
  >
  > Es en esta parte donde la maquina o el motor de JavaScript al tratar de optimizar se genera el **Hoisting**, que solo sucede con variables y funciones.

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

### Memory Heap

Es el lugar donde se almacenan las variables, objetos y funciones. 

### Call Stack

Pila de tareas a realizar. JavaScript solo hace una tarea a la vez (Synchronous), en la parte baja está el *Objeto global* y por sobre esta se colocan las demás tareas, las cuales se van ejecutando desde la ultima a la primera, siendo el *Objeto Global* el que queda al final.














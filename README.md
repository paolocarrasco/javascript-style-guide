# Airbnb JavaScript Style Guide() {

*Un enfoque altamente razonable para JavaScript*

> **Nota**: Esta guía asume que usas [Babel](https://babeljs.io), adicionalmente de tener instalado [babel-preset-airbnb](https://npmjs.com/babel-preset-airbnb) o su equivalente. También asume que tienes instalado shims/pollyfills en tu aplicación, con 
[airbnb-browser-shims](https://npmjs.com/airbnb-browser-shims) o su equivalente.

[![Downloads](https://img.shields.io/npm/dm/eslint-config-airbnb.svg)](https://www.npmjs.com/package/eslint-config-airbnb)
[![Downloads](https://img.shields.io/npm/dm/eslint-config-airbnb-base.svg)](https://www.npmjs.com/package/eslint-config-airbnb-base)
[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/airbnb/javascript?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

Otras Guías de Estilos
- [ES5](es5/) (Obsoleto)
- [React](https://github.com/agrcrobles/javascript/tree/master/react)
- [CSS-in-JavaScript](https://github.com/airbnb/javascript/tree/master/css-in-javascript/)
- [CSS & SASS](https://github.com/ismamz/css)
- [Ruby](https://github.com/airbnb/ruby)

## Tabla de Contenido

  1. [Tipos](#tipos)
  1. [Referencias](#referencias)
  1. [Objetos](#objetos)
  1. [Arreglos](#arreglos)
  1. [Destructuring](#destructuring)
  1. [Cadenas de Texto](#cadenas-de-texto)
  1. [Funciones](#funciones)
  1. [Notación de Funciones de Flecha](#notación-de-funciones-de-flecha)
  1. [Clases y Constructores](#clases-y-constructores)
  1. [Módulos](#módulos)
  1. [Iteradores y Generadores](#iteradores-y-generadores)
  1. [Propiedades](#propiedades)
  1. [Variables](#variables)
  1. [Hoisting](#hoisting)
  1. [Expresiones de comparación e igualdad](#expresiones-de-comparación-e-igualdad)
  1. [Bloques](#bloques)
  1. [Comentarios](#comentarios)
  1. [Espacios en blanco](#espacios-en-blanco)
  1. [Comas](#comas)
  1. [Puntos y Comas](#puntos-y-comas)
  1. [Casting de Tipos y Coerción](#casting-de-tipos-y-coerción)
  1. [Convenciones de nomenclatura](#convenciones-de-nomenclatura)
  1. [Funciones de Acceso](#funciones-de-acceso)
  1. [Eventos](#eventos)
  1. [jQuery](#jquery)
  1. [Compatibilidad con EcmaScript 5](#compatibilidad-con-ecmascript-5)
  1. [Estilos de EcmaScript6+ (ES2015+)](#estilos-de-ecmascript6-es-2015)
  1. [Pruebas](#pruebas)
  1. [Desempeño](#desempeño)
  1. [Recursos](#recursos)
  1. [En la cancha](#en-la-cancha)
  1. [Traducciones](#traducciones)
  1. [La guía de la Guía de Estilos JavaScript](#la-guía-de-la-guía-de-estilos-de-javascript)
  1. [Charla con nosotros sobre Javascript](#charla-con-nosotros-sobre-javascript)
  1. [Colaboradores](#colaboradores)
  1. [Licencia](#licencia)

## Tipos

  - **Primitivos**: Cuando accedes a un tipo primitivo, manejas directamente su valor

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **Complejo**: Cuando accedes a un tipo complejo, manejas la referencia a su valor.

    + `object`
    + `array`
    + `function`

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

## Referencias
  - Usa `const` para todas tus referencias; evita usar `var`.
  > ¿Por qué? Esto asegura que no reasignes tus referencias, lo
  que puede llevar a bugs y dificultad para comprender el código.

  ```javascript
  // mal
  var a = 1;
  var b = 2;

  // bien
  const a = 1;
  const b = 2;
  ```

  - Si vas a reasignar referencias, usa `let` en vez de `var`.
  > ¿Por qué? El bloque `let` es de alcance a nivel de bloque a
  diferencia del alcance a nivel de función de `var`.

  ```javascript
  // mal
  var count = 1;
  if (true) {
    count += 1;
  }

  // bien, usa el let
  let count = 1;
  if (true) {
    count += 1;
  }
  ```

  - Nota que tanto `let` como `const` tienen alcance a nivel de bloque.

  ```javascript
  // const y let solo existen en los bloques donde
  // estan definidos
  {
    let a = 1;
    const b = 1;
  }
  console.log(a); // ReferenceError
  console.log(b); // ReferenceError
  ```

## Objetos

  - Usa la sintaxis literal para la creación de un objeto.

    ```javascript
    // mal
    const item = new Object();

    // bien
    const item = {};
    ```

  - No uses [palabras reservadas](http://es5.github.io/#x7.6.1) para nombres de propiedades. No funciona en IE8 [Más información](https://github.com/airbnb/javascript/issues/61). No hay problema de usarlo en módulos de ES6 y en código de servidor.

    ```javascript
    // mal
    const superman = {
      default: { clark: 'kent' },
      private: true
    };

    // bien
    const superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - Usa sinónimos legibles en lugar de palabras reservadas.

    ```javascript
    // mal
    const superman = {
      class: 'alien'
    };

    // mal
    const superman = {
      klass: 'alien'
    };

    // bien
    const superman = {
      type: 'alien'
    };
    ```
**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

## Arreglos

  - Usa la sintaxis literal para la creación de arreglos

    ```javascript
    // mal
    const items = new Array();

    // bien
    const items = [];
    ```

  - Usa Array#push, en vez de asignación directa, para agregar elementos a un arreglo.

    ```javascript
    const someStack = [];

    // mal
    someStack[someStack.length] = 'abracadabra';

    // bien
    someStack.push('abracadabra');
    ```

  - Usa [spread de arrays](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Operadores/Spread_operator) para copiar arreglos.

    ```javascript
    const len = items.length;
    const itemsCopy = [];
    let i;

    // mal
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // bien
    const itemsCopy = [...items];
    ```

  - Para convertir un objeto ["array-like" (similar a un arreglo)](https://www.inkling.com/read/javascript-definitive-guide-david-flanagan-6th/chapter-7/array-like-objects) a un arreglo, usa Array#from.

    ```javascript
    const foo = document.querySelectorAll('.foo');
    const nodes = Array.from(foo);
    ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

## Destructuring

  - Usa object destructuring cuando accedas y uses múltiples propiedades de un objeto.

    > ¿Por qué? Destructuring te ahorra crear referencias temporales para esas propiedades.

    ```javascript
    // mal
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // bien
    function getFullName(user) {
      const { firstName, lastName } = user;
      return `${firstName} ${lastName}`;
    }

    // mejor
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```

  - Usa array destructuring.

    ```javascript
    const arr = [1, 2, 3, 4];

    // mal
    const first = arr[0];
    const second = arr[1];

    // bien
    const [first, second] = arr;
    ```

  - Usa object destructuring para múltiple valores de retorno, no array destructuring.

    > ¿Por qué? Puedes agregar nuevas propiedades en el tiempo o cambiar el orden de las cosas sin afectar la forma en que se llama.

    ```javascript
    // mal
    function processInput(input) {
      // then a miracle occurs
      return [left, right, top, bottom];
    }

    // el que llama necesita pensar en el orden de la data de retorno
    const [left, __, top] = processInput(input);

    // bien
    function processInput(input) {
      // then a miracle occurs
      return { left, right, top, bottom };
    }

    // el que llama elige solo la data que necesita
    const { left, top } = processInput(input);
    ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**


## Cadenas de Texto

  - Usa comillas simples `''` para las cadenas de texto

    ```javascript
    // mal
    const name = "Bob Parr";

    // bien
    const name = 'Bob Parr';
    ```

  - Las cadenas de texto con una longitud mayor a 100 caracteres deben ser escritas en múltiples líneas usando concatenación.

  > **Nota:** Cuando se usa sin criterio, las cadenas de texto largas pueden impactar en el desempeño. [jsPerf](http://jsperf.com/ya-string-concat) & [Discusión](https://github.com/airbnb/javascript/issues/40)

  ```javascript
  // mal
  var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

  // bien
  var errorMessage = 'This is a super long error that was thrown because\
  of Batman. When you stop to think about how Batman had anything to do \
  with this, you would get nowhere fast.';

  // bien
  var errorMessage = 'This is a super long error that was thrown because' +
    'of Batman. When you stop to think about how Batman had anything to do ' +
    'with this, you would get nowhere fast.';
  ```

  - Cuando se crean cadenas de texto de forma programática, usa template strings (cadena de plantillas) en vez de concatenación.

  > ¿Por qué? Los template strings te dan mayor legibilidad, sintaxis concisa con nuevas líneas apropiadas y capacidades de interpolación.

  ```javascript
  // mal
  function sayHi(name) {
    return 'How are you, ' + name + '?';
  }

  // mal
  function sayHi(name) {
    return ['How are you, ', name, '?'].join();
  }

  // bien
  function sayHi(name) {
    return `How are you, ${name}?`;
  }
  ```

  - Nunca uses `eval()` en una cadena de texto, abre una caja de Pandora de vulnerabilidades.

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**


## Funciones

  - Usa declaración de función en vez de expresiones de función.

  > ¿Por qué? Las declaraciones de función son nombradas, por lo que son más sencillas de identificar en las pilas de llamadas. Además todo el contenido de una declaración de función es *hoisted*, mientras que solo la referencia de una expresión de función es *hoisted*. Esta regla hace posible que siempre se usen [Arrow Functions](#notación-de-funciones-de-flecha) en vez de las funciones de expresión.

  ```javascript
   // mal
   const foo = function () {
   };

   // bien
   function foo() {
   }
   ```

  - Nunca declares una función en un bloque que no sea de función (if, while, etc). En vez de ello, asigna la función a una variable. Los navegadores te permitirán hacerlo pero todos ellos lo interpretarán de modo diferente, lo que es lamentable.

  > **Nota:** ECMA-262 define un bloque como una lista de sentencias. Una declaración de función no es una sentencia. [Lee la nota de ECMA-262 sobre este inconveniente](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

  ```javascript
  // mal
  if (currentUser) {
    function test() {
      console.log('Nope.');
    }
  }

  // bien
  let test;
  if (currentUser) {
    test = () => {
      console.log('Yup.');
    };
  }
  ```

  - Nunca nombres a un parámetro como `arguments`, esto tendrá precedencia sobre el objeto `arguments` que es brindado en cada ámbito de función.

    ```javascript
    // mal
    function nope(name, options, arguments) {
      // ...algo...
    }

    // bien
    function yup(name, options, args) {
      // ...algo...
    }
    ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

## Notación de Funciones de Flecha

  - Cuando debas usar funciones anónimas (como cuando pasas un callback inline), usa la notación de funciones de flecha.

    > ¿Por qué? Crea una versión de la función que ejecuta en el contexto de `this`, lo que usualmente es lo que deseas, además que tiene una sintaxis más concisa.

    > ¿Por qué no? Si tienes una función complicada, debes mover esa lógica fuera de su expresión de función nombrada.

    ```javascript
    // mal
    [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // bien
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  - Si el cuerpo de la función consiste en una sola sentencia retornando una [expresión](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) sin efectos colaterales, omite las llaves y usa el retorno implícito.
  De otro modo, mantén las llaves y usa una sentencia de retorno.

    > ¿Por qué? Un edulcorante sintáctico. Se lee bien cuando múltiples funciones están encadenadas entre sí.

    ```javascript
    // mal
    [1, 2, 3].map(number => {
      const nextNumber = number + 1;
      `A string containing the ${nextNumber}.`;
    });

    // bien
    [1, 2, 3].map(number => `A string containing the ${number}.`);

    // bien
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
    });

    // bien
    [1, 2, 3].map((number, index) => ({
      [index]: number,
    }));

    // Sin efectos colaterales para retorno implícito
    function foo(callback) {
      const val = callback();
      if (val === true) {
        // Do something if callback returns true
      }
    }

    let bool = false;

    // mal
    foo(() => bool = true);

    // bien
    foo(() => {
      bool = true;
    });
    ```

  - En caso que la expresión se expanda en varias líneas, envuélvela en paréntesis para una mejor legibilidad.

    > ¿Por qué? Se observa claramente dónde empieza y termina la función.

    ```javascript
    // mal
    ['get', 'post', 'put'].map(httpMethod => Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    );

    // bien
    ['get', 'post', 'put'].map(httpMethod => (
      Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    ));
    ```

  - Si tu función tiene un solo argumento y no usa llaves, omite los paréntesis. De otra forma, siempre incluye paréntesis alrededor de los argumentos por claridad y consistencia.
  Nota: es también aceptable siempre usar paréntesis, en cuyo caso usa la [opción de "always"](https://eslint.org/docs/rules/arrow-parens#always) para eslint o no incluyas [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam) para jscs.

    > ¿Por qué? Menos basura visual.

    ```javascript
    // mal
    [1, 2, 3].map((x) => x * x);

    // bien
    [1, 2, 3].map(x => x * x);

    // bien
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));

    // mal
    [1, 2, 3].map(x => {
      const y = x + 1;
      return x * y;
    });

    // bien
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  - Evita confundir la sintaxis de función de flecha (`=>`) con los operadores de comparación (`<=`, `>=`).

    ```javascript
    // mal
    const itemHeight = item => item.height > 256 ? item.largeSize : item.smallSize;

    // mal
    const itemHeight = (item) => item.height > 256 ? item.largeSize : item.smallSize;

    // bien
    const itemHeight = item => (item.height > 256 ? item.largeSize : item.smallSize);

    // bien
    const itemHeight = (item) => {
      const { height, largeSize, smallSize } = item;
      return height > 256 ? largeSize : smallSize;
    };
    ```

**[⬆ regresar a la Tabla de Contenido](#tabla-de-contenido)**

## Clases y Constructores

  - Siempre usa `class`. Evita manipular `prototype` directamente.

    > ¿Por qué? La sintaxis `class` es más concisa y fácil con la cual lidiar.

    ```javascript
    // mal
    function Queue(contents = []) {
      this._queue = [...contents];
    }
    Queue.prototype.pop = function () {
      const value = this._queue[0];
      this._queue.splice(0, 1);
      return value;
    }

    // bien
    class Queue {
      constructor(contents = []) {
        this._queue = [...contents];
      }
      pop() {
        const value = this._queue[0];
        this._queue.splice(0, 1);
        return value;
      }
    }
    ```

  - Métodos pueden retornar `this` para ayudar con el encadenamiento de métodos (*chaining*).

    ```javascript
    // mal
    Jedi.prototype.jump = function () {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function (height) {
      this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // bien
    class Jedi {
      jump() {
        this.jumping = true;
        return this;
      }

      setHeight(height) {
        this.height = height;
        return this;
      }
    }

    const luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```

  - Está bien escribir un método `toString()` personalizado, solo asegúrate que funcione correctamente y no cause efectos colaterales.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        this.name = options.name || 'no name';
      }

      getName() {
        return this.name;
      }

      toString() {
        return `Jedi - ${this.getName()}`;
      }
    }
    ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

## Módulos

  - Siempre usa módulos (`import`/`export`) antes que un sistema de módulos no estándar. Siempre puedes transpilar a tu sistema de módulos preferido.

    > ¿Por qué? Los módulos son el futuro, comencemos a usar el futuro en el presente.

    ```javascript
    // mal
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // mejor
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  - No uses imports con comodines (asterisco).

    > ¿Por qué? Esto te asegura de tener una única exportación por defecto.

    ```javascript
    // mal
    import * as AirbnbStyleGuide from './AirbnbStyleGuide';

    // bien
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    ```

  - Y no exportes directamente lo que traigas de un import.

    > ¿Por qué? A pesar que hacer las cosas en una línea es conciso, tener un modo claro de importar y un modo claro de exportar, hace las cosas consistentes.

    ```javascript
    // mal
    // filename es6.js
    export { es6 as default } from './AirbnbStyleGuide';

    // bien
    // filename es6.js
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  - Solo importa de una ruta en un mismo lugar.

    > ¿Por qué? Tener varias líneas que importan de una misma ruta hace al código difícil de mantener.

    ```javascript
    // mal
    import foo from 'foo';
    // … some other imports … //
    import { named1, named2 } from 'foo';

    // bien
    import foo, { named1, named2 } from 'foo';

    // bien
    import foo, {
      named1,
      named2,
    } from 'foo';
    ```

  - No exportes las asociaciones (bindings) mutables.

    > ¿Por qué? La mutación debe ser evitada en general, pero en particular cuando se exportan asociaciones (bindings) mutables. Mientras esta técnica puede ser necesaria para algunos casos especiales, en general solo referencias constantes deben ser exportadas.

    ```javascript
    // mal
    let foo = 3;
    export { foo };

    // bien
    const foo = 3;
    export { foo };
    ```

  - En módulos con una única exportación, prefiere la exportación por defecto sobre la exportación nombrada.

    > ¿Por qué? Para forzar a que más archivos solo exporten una sola cosa, lo que es mejor para la legibilidad y mantenibilidad.

    ```javascript
    // mal
    export function foo() {}

    // bien
    export default function foo() {}
    ```

  - Pon todos los `import`s encima de las sentencias de no importación.

    > ¿Por qué? Desde que los `import`s son elevados (hoisted), mantenerlos en el inicio previene comportamientos sorpresivos.

    ```javascript
    // mal
    import foo from 'foo';
    foo.init();

    import bar from 'bar';

    // bien
    import foo from 'foo';
    import bar from 'bar';

    foo.init();
    ```

  - Imports de multi-línea deben ser indentados como los arreglos multi-línea y literales de objeto.

    > ¿Por qué? Las llaves deben seguir las mismas reglas de indentación como en otros bloques de llaves en la guía de estilos, así como las comas finales.

    ```javascript
    // mal
    import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

    // bien
    import {
      longNameA,
      longNameB,
      longNameC,
      longNameD,
      longNameE,
    } from 'path';
    ```

  - No permitas la sintaxis de carga de Webpack en las sentencias de importación de módulos.

    > ¿Por qué? Debido a que usar la sintaxis de Webpack en los imports acopla el código a un ensamblador de módulos. Prefiere usar aquella sintaxis de carga en el archivo de `webpack.config.js`.

    ```javascript
    // mal
    import fooSass from 'css!sass!foo.scss';
    import barCss from 'style!css!bar.css';

    // bien
    import fooSass from 'foo.scss';
    import barCss from 'bar.css';
    ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**


## Propiedades

  - Usa la notación de punto `.` cuando accedas a las propiedades.

    ```javascript
    const luke = {
      jedi: true,
      age: 28
    };

    // mal
    const isJedi = luke['jedi'];

    // bien
    const isJedi = luke.jedi;
    ```

  - Usa la notación subscript `[]` cuando accedas a las propiedades con una variable.

    ```javascript
    const luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    const isJedi = getProp('jedi');
    ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

## Variables

  - Siempre usa `const` para declarar constantes o `let` para declarar variables. No hacerlo resultará en variables globales. Debemos evitar contaminar el espacio global (global namespace). El [Capitán Planeta](https://es.wikipedia.org/wiki/Capit%C3%A1n_Planeta_y_los_planetarios) nos advirtió de eso.

    ```javascript
    // mal
    superPower = new SuperPower();

    // bien
    const superPower = new SuperPower();

    o

    // bien
    let aPower;
    aPower = new SuperPower(); // esto puede cambiar a otro poder posteriormente
    ```

  - Usa una declaración `const` o `let` por variable.

    > ¿Por qué? Es más fácil agregar nuevas declaraciones de variables de este modo, y no tendrás que preocuparte por reemplazar `;` por `,` o introducir diffs de sólo puntuación .

    ```javascript
    // mal
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // mal
    // (compara con lo de arriba y encuentra el error)
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // bien
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  - Agrupa tus `const`s y luego agrupa tus `let`s.
    > ¿Por qué? Esto es útil cuando necesites asignar una variable luego dependiendo de una de las variables asignadas previamente.

   ```javascript
   // mal
   let i, len, dragonball,
       items = getItems(),
       goSportsTeam = true;

   // mal
   let i;
   const items = getItems();
   let dragonball;
   const goSportsTeam = true;
   let len;

   // bien
   const goSportsTeam = true;
   const items = getItems();
   let dragonball;
   let i;
   let length;
   ```

  - Asigna las variables cuando las necesites, pero ponlas en un lugar razonable.
    > ¿Por qué? `let` y `const` están a nivel de bloque, no a  nivel de función.

   ```javascript
   // mal - llamada a funcion innecesaria
   function checkName(hasName) {
     const name = getName();

     if (hasName === 'test') {
       return false;
     }

     if (name === 'test') {
       this.setName('');
       return false;
     }

     return name;
   }

   // bien
   function checkName(hasName) {
     if (hasName === 'test') {
       return false;
     }

     const name = getName();

     if (name === 'test') {
       this.setName('');
       return false;
     }

     return name;
   }
   ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**


## Hoisting

  - Las declaraciones de variables son movidas a la parte superior de su ámbito, sin embargo su asignación no.

    ```javascript
    // sabemos que esto no funcionara (asumiendo
    // que no hay una variable global notDefined)
    function example() {
      console.log(notDefined); // => lanza un ReferenceError
    }

    // crear una declaracion de variable luego
    // que referencies a la variable funcionara
    // por el hoisting. Nota: A la asignacion
    // del valor `true` no se le aplico hoisting.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // El interprete lleva la declaracion de la
    // variable a la parte superior de la funcion.
    // Eso significa que nuestro ejemplo
    // podria ser reescrito como:
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - Expresiones de función anónimas hacen hoisting de su nombre de variable, pero no de la asignación de la función.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - Expresiones de función nombradas hacen hoisting de su nombre de variable, pero no del nombre de la función ni del contenido de la función.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // lo mismo es cierto cuando el nombre
    // de la funcion es igual al nombre de
    // la variable.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - Las declaraciones de función hacen hoist de su nombre y del contenido de la función.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - Para más información lee [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) por [Ben Cherry](http://www.adequatelygood.com/)

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**



## Expresiones de comparación e igualdad

  - Usa `===` y `!==` en vez de `==` y `!=` respectivamente.
  - Las expresiones condicionales son evaluadas usando coerción con el método `ToBoolean` y siempre obedecen a estas reglas sencillas:

    + **Objects** son evaluados como **true** (se considera así al objeto vacío `{}` y arreglos sin contenido `[]`)
    + **Undefined** es evaluado como **false**
    + **Null** es evaluado como **false**
    + **Booleans** son evaluados como **el valor del booleano**
    + **Numbers** son evaluados como **false** si su valor es **+0**, **-0**, o **NaN**, de otro modo **true**
    + **Strings** son evaluados como **false** si es una cadena de texto vacía `''`, de otro modo son **true**

    ```javascript
    if ([0] && []) {
      // true
      // un arreglo es un objeto (incluso uno vacío), los objetos son evaluados como true
    }
    ```

  - Usa atajos.

    ```javascript
    // mal
    if (name !== '') {
      // ...cosas...
    }

    // bien
    if (name) {
      // ...cosas...
    }

    // mal
    if (collection.length > 0) {
      // ...cosas...
    }

    // bien
    if (collection.length) {
      // ...cosas...
    }
    ```

  - Para más información revisa [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) por Angus Croll

  - Usa llaves para crear bloques en cláusulas `case` y `default` que contengan
  declaraciones léxicas (e.g. `let`, `const`, `function` y `class`).

    > ¿Por qué? La declaración léxica es visible en todo el bloque `switch`
    pero solo se inicializa al ser asignado, lo que solo ocurre cuando el bloque
    `case` donde es declarado es alcanzado. Esto causa problemas cuando
    múltiples bloques `case` intentan definir la misma variable.


    ```javascript
    // mal
    switch (foo) {
      case 1:
        let x = 1;
        break;
      case 2:
        const y = 2;
        break;
      case 3:
        function f() {}
        break;
      default:
        class C {}
    }

    // bien
    switch (foo) {
      case 1: {
        let x = 1;
        break;
      }
      case 2: {
        const y = 2;
        break;
      }
      case 3: {
        function f() {}
        break;
      }
      case 4:
        bar();
        break;
      default: {
        class C {}
      }
    }
    ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**


## Bloques

  - Usa llaves con todos los bloques de múltiples líneas.

    ```javascript
    // mal
    if (test)
      return false;

    // bien
    if (test) return false;

    // bien
    if (test) {
      return false;
    }

    // mal
    function() { return false; }

    // bien
    function() {
      return false;
    }
    ```

  - Si estás usando bloques de muchas líneas con ```if``` y ```else```, pon el ```else``` en la misma línea que el ```if```.

    ```javascript
    // mal
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // bien
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**


## Comentarios

  - Usa `/** ... */` para comentarios de múltiples líneas. Incluye una descripción, especificación de tipos y valores para todos los parámetros y valores de retorno.

    ```javascript
    // mal
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // bien
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - Usa `//` para comentarios de una sola línea. Ubica los comentarios de una sola línea encima de la sentencia comentada. Deja una línea en blanco antes del comentario, a menos que sea la primera línea de un bloque.

    ```javascript
    // mal
    const active = true;  // is current tab

    // bien
    // is current tab
    const active = true;

    // mal
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }

    // bien
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }
    ```

  - Agregando a tus comentarios los prefijos `FIXME` o `TODO`, ayudará a otros desarrolladores a entender rápidamente si estás apuntando a un problema que precisa ser revisado o si estás sugiriendo una solución al problema que debería ser implementado. Estos son diferentes a comentarios regulares en el sentido que requieren alguna acción. Las acciones son `FIXME -- necesito resolver esto` o `TODO -- necesita implementarse`.

  - Usa `// FIXME:` para anotar problemas.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // FIXME: shouldn't use a global here
        total = 0;
      }
    }
    ```

  - Usa `// TODO:` para anotar soluciones a los problemas.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // TODO: total should be configurable by an options param
        this.total = 0;
      }
    }
    ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**


## Espacios en blanco

  - Usa indentaciones blandas (sin TAB)  establecidas en dos espacios.

    ```javascript
    // mal
    function foo() {
    ∙∙∙∙const name;
    }

    // mal
    function bar() {
    ∙const name;
    }

    // bien
    function baz() {
    ∙∙const name;
    }
    ```
  - Deja un espacio antes de la llave de apertura.

    ```javascript
    // mal
    function test(){
      console.log('test');
    }

    // bien
    function test() {
      console.log('test');
    }

    // mal
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // bien
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```

  - Deja un espacio antes del paréntesis de apertura en las sentencias de control (```if```, ```while```, etc.). No dejes espacios antes de la lista de argumentos en las invocaciones y declaraciones de funciones.
    ```javascript
    // mal
    if(isJedi) {
      fight ();
    }

    // bien
    if (isJedi) {
      fight();
    }

    // mal
    function fight () {
      console.log ('Swooosh!');
    }

    // bien
    function fight() {
      console.log('Swooosh!');
    }
    ```

  - Separa a los operadores con espacios.
    ```javascript
    // mal
    const x=y+5;

    // bien
    const x = y + 5;
    ```

  - Deja una línea en blanco al final del archivo.

    ```javascript
    // mal
    (function(global) {
      // ...algo...
    })(this);
    ```

    ```javascript
    // mal
    (function(global) {
      // ...algo...
    })(this);↵
    ↵

    ```

    ```javascript
    // bien
    (function(global) {
      // ...algo...
    })(this);↵

    ```

  - Usa indentación cuando uses métodos largos con 'chaining' (más de dos métodos encadenados). Emplea un punto adelante en cada nueva línea, lo que enfatiza que es un método llamado no una nueva sentencia.

    ```javascript
    // mal
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // mal
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // bien
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // mal
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // bien
    const leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .class('led', true)
        .attr('width',  (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

  - Deja una línea en blanco luego de los bloques y antes de la siguiente sentencia.

    ```javascript
    // mal
    if (foo) {
      return bar;
    }
    return baz;

    // bien
    if (foo) {
      return bar;
    }

    return baz;

    // mal
    const obj = {
      foo() {
      },
      bar() {
      }
    };
    return obj;

    // bien
    const obj = {
      foo() {
      },

      bar() {
      }
    };

    return obj;

    // mal
    const arr = [
      function foo() {
      },
      function bar() {
      },
    ];
    return arr;

    // bien
    const arr = [
      function foo() {
      },

      function bar() {
      },
    ];

    return arr;
    ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

## Comas

  - Comas al inicio de línea: **Nop.**

    ```javascript
    // mal
    const story = [
        once
      , upon
      , aTime
    ];

    // bien
    const story = [
      once,
      upon,
      aTime,
    ];

    // mal
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'strength'
    };

    // bien
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  - Coma adicional al final: **Sip.**
  > ¿Por qué? Esto lleva a diferenciales en git más claros. Además los transpiladores como Babel removerán la coma del final en el código transpilado lo que significa que no te tendrás que preocupar del [problema de la coma adicional al final](es5/README.md#commas) en navegadores antiguos.

  ```javascript
  // mal - git diff sin coma adicional al final
  const hero = {
       firstName: 'Florence',
  -    lastName: 'Nightingale'
  +    lastName: 'Nightingale',
  +    inventorOf: ['coxcomb chart', 'modern nursing']
  };

  // bien - git diff con coma adicional al final
  const hero = {
       firstName: 'Florence',
       lastName: 'Nightingale',
  +    inventorOf: ['coxcomb chart', 'modern nursing'],
  };

  // mal
  const hero = {
    firstName: 'Dana',
    lastName: 'Scully'
  };

  const heroes = [
    'Batman',
    'Superman'
  ];

  // bien
  const hero = {
    firstName: 'Dana',
    lastName: 'Scully',
  };

  const heroes = [
    'Batman',
    'Superman',
  ];
  ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**


## Puntos y Comas

  - **Sip.**

    ```javascript
    // mal
    (function () {
      const name = 'Skywalker'
      return name
    })()

    // bien
    (() => {
      const name = 'Skywalker';
      return name;
    }());

    // bien, pero arcaico (evita que la funcion se vuelva un argumento
    // cuando dos archivos con IIFEs sean concatenados)
    ;(() => {
      const name = 'Skywalker';
      return name;
    }());
    ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**


## Casting de Tipos y Coerción

  - Ejecuta coerción al inicio de una sentencia.
  - Strings:

    ```javascript
    //  => this.reviewScore = 9;

    // mal
    const totalScore = this.reviewScore + ''; // invoca a this.reviewScore.valueOf()

    // mal
    const totalScore = this.reviewScore.toString(); // no se garantiza que retorne una cadena de texto

    // bien
    const totalScore = String(this.reviewScore);
    ```

  - Números: Usa `Number` para el casting de tipo y `parseInt` siempre con la base numérica para el casting de textos.

    ```javascript
    const inputValue = '4';

    // mal
    const val = new Number(inputValue);

    // mal
    const val = +inputValue;

    // mal
    const val = inputValue >> 0;

    // mal
    const val = parseInt(inputValue);

    // bien
    const val = Number(inputValue);

    // bien
    const val = parseInt(inputValue, 10);
    ```

  - Si por alguna razón estás haciendo algo salvaje y `parseInt` es un cuello de botella por lo que necesitaste usar Bitshift por [razones de desempeño](http://jsperf.com/coercion-vs-casting/3), deja un comentario explicando la razón y resumen de lo que estás haciendo.

    ```javascript
    // bien
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    const val = inputValue >> 0;
    ```

  > **Nota:** Ten mucho cuidado al hacer operaciones de Bitshift. En Javascript los números son representados como [valores de 64-bit](http://es5.github.io/#x4.3.19), sin embargo las operaciones de Bitshift siempre retornan un entero de 32-bits ([fuente](http://es5.github.io/#x11.7)). Bitshift puede presentarnos un comportamiento inesperado para valores enteros mayores a 32 bits. [Discusión](https://github.com/airbnb/javascript/issues/109). El mayor entero con signo de 32 bits es 2,147,483,647:
  ```javascript
  2147483647 >> 0 //=> 2147483647
  2147483648 >> 0 //=> -2147483648
  2147483649 >> 0 //=> -2147483647
  ```

  - Booleans:

    ```javascript
    const age = 0;

    // mal
    const hasAge = new Boolean(age);

    // bien
    const hasAge = Boolean(age);

    // bien
    const hasAge = !!age;
    ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**


## Convenciones de nomenclatura

  - Evita nombres de una sola letra. Sé descriptivo con tus nombres.

    ```javascript
    // mal
    function q() {
      // ...algo...
    }

    // bien
    function query() {
      // ...algo...
    }
    ```

  - Usa camelCase cuando nombres tus objetos, funciones e instancias.

    ```javascript
    // mal
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    const o = {};
    function c() {}

    // bien
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  - Usa PascalCase cuando nombres constructores o clases.

    ```javascript
    // mal
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope'
    });

    // bien
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

  - No uses prefijos ni sufijos de guiones bajo.
  > ¿Por qué? JavaScript no tiene el concepto de privacidad en términos de propiedades o métodos. A pesar que un guión bajo como prefijo es una convención común para indicar que son "privados", la realidad es que estas propiedades son absolutamente públicas, y por ello, parte de tu contrato público de API. La convención del prefijo de guión bajo podría orientar a los desarrolladores a pensar erróneamente que un cambio a aquellos no será de impacto o que los tests no son necesarios.

  ```javascript
  // mal
  this.__firstName__ = 'Panda';
  this.firstName_ = 'Panda';
  this._firstName = 'Panda';


  // bien
  this.firstName = 'Panda';
  ```

  - Nunca guardes referencias a `this`. Usa funciones arrow o la función [#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)

    ```javascript
    // mal
    function() {
      const self = this;
      return function() {
        console.log(self);
      };
    }

    // mal
    function() {
      const that = this;
      return function() {
        console.log(that);
      };
    }

    // bien
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```

  - El nombre del archivo base debe corresponder exactamente con el nombre de su export por defecto.

  ```javascript
   // contenido archivo 1
   class CheckBox {
     // ...
   }
   export default CheckBox;

   // contenido archivo 2
   export default function fortyTwo() { return 42; }

   // contenido archivo 3
   export default function insideDirectory() {}

   // en algún otro archivo
   // mal
   import CheckBox from './checkBox'; // importacion/exportacion PascalCase, nombre de archivo camelCase
   import FortyTwo from './FortyTwo'; // importacion/nombre de archivo PascalCase, exportacion camelCase
   import InsideDirectory from './InsideDirectory'; // importacion/nombre de archivo PascalCase, exportacion camelCase

   // mal
   import CheckBox from './check_box'; // importacion/exportacion PascalCase, nombre de archivo snake_case
   import forty_two from './forty_two'; // importacion/nombre de archivo snake_case, exportacion camelCase
   import inside_directory from './inside_directory'; // importacion snake_case, exportacion camelCase
   import index from './inside_directory/index'; // requiere el archivo de index explicitamente
   import insideDirectory from './insideDirectory/index'; // requiere el archivo de index explicitamente

   // bien
   import CheckBox from './CheckBox'; // importacion/exportacion/nombre de archivo PascalCase
   import fortyTwo from './fortyTwo'; // importacion/exportacion/nombre de archivo camelCase
   import insideDirectory from './insideDirectory'; // importacion/exportacion/nombre directorio/archivo "index" implícito
   // ^ soporta tanto insideDirectory.js e insideDirectory/index.js

   ```

  - Usa camelCase cuando exportes por defecto una función. Tu nombre de archivo debe ser idéntico al nombre de tu función.

  ```javascript
  function makeStyleGuide() {
  }

  export default makeStyleGuide;
  ```

  - Usa camelCase cuando exportes un objeto constructor / clase / singleton / librería de función / esqueleto.
  ```javascript
  const AirbnbStyleGuide = {
    es6: {
    }
  };

  export default AirbnbStyleGuide;
  ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**


## Funciones de Acceso

  - Funciones de acceso para las propiedades no son requeridas.
  - No uses getters/setters de JavaScript ya que causan efectos colaterales no esperados y son difíciles de probar, mantener y razonar. En vez de ello, si creas funciones de acceso usa  ```getVal()``` y ```setVal('hello')```.

     ```javascript
     // Maintainable-JavaScript-Nicholas-C-Zakas
     class Dragon {
       get age() {
         // ...
       }

       set age(value) {
         // ...
       }
     }

     // bien
     class Dragon {
       getAge() {
         // ...
       }

       setAge(value) {
         // ...
       }
     }
     ```

  - Si la propiedad es un booleano, usa ```isVal()``` o ```hasVal()```.

    ```javascript
    // mal
    if (!dragon.age()) {
      return false;
    }

    // bien
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - Está bien crear funciones ```get()``` y ```set()```, pero sé consistente.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);
      }

      set(key, val) {
        this[key] = val;
      }

      get(key) {
        return this[key];
      }
    }
    ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**


## Eventos

  - Cuando envíes paquetes de datos a los eventos (ya sea con eventos del DOM o algo propietario como los eventos de Backbone), pasa un mapa en vez de un valor directo. Esto permitirá a un próximo colaborador a agregar más datos al paquete de datos sin que tenga que encontrar o actualizar un handler para cada evento. Por ejemplo, en vez de:

    ```js
    // mal
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', (e, listingId) => {
      // hacer algo con listingId
    });
    ```

    prefiere:

    ```js
    // bien
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', (e, data) => {
      // hacer algo con data.listingId
    });
    ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

## jQuery

  - Nombre las variables de objetos jQuery con un prefijo `$`.

    ```javascript
    // mal
    const sidebar = $('.sidebar');

    // bien
    const $sidebar = $('.sidebar');
    ```

  - Guarde en variables los lookups de jQuery que se necesiten posteriormente.

    ```javascript
    // mal
    function setSidebar() {
      $('.sidebar').hide();

      // ...algo...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // bien
    function setSidebar() {
      const $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...algo...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - Para consultas de elementos DOM usa el modo Cascada `$('.sidebar ul')` o parent > child `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - Usa `find` solo con consultas guardadas en variables previamente.

    ```javascript
    // mal
    $('ul', '.sidebar').hide();

    // mal
    $('.sidebar').find('ul').hide();

    // bien
    $('.sidebar ul').hide();

    // bien
    $('.sidebar > ul').hide();

    // bien
    $sidebar.find('ul');
    ```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**


## Compatibilidad con ECMAScript 5

  - Revisa la [tabla de compatibilidad](http://kangax.github.com/es5-compat-table/) de ES5 de [Kangax](https://twitter.com/kangax/).

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

## Estilos de EcmaScript6+ (ES 2015+)

    - A continuación, un conjunto de enlaces hacia los estilos para las nuevas características de ES6:

  1. [Notación Funciones de Flecha](#arrow-functions)
  1. [Clases](#classes--constructors)
  1. [Declaración abreviada para objeto](#es6-object-shorthand)
  1. [Declaración de objeto concisa](#es6-object-concise)
  1. [Propiedades computadas de objeto](#es6-computed-properties)
  1. [Plantillas de texto](#es6-template-literals)
  1. [Destructuring](#destructuring)
  1. [Parámetros por defecto](#es6-default-parameters)
  1. [Rest](#es6-rest)
  1. [Spreads de arreglos](#es6-array-spreads)
  1. [Let y Const](#references)
  1. [Iteradores y Generadores](#iterators-and-generators)
  1. [Módulos](#modules)

    - No uses [las propuestas de TC39](https://github.com/tc39/proposals) puesto que aún no han llegado a la tercera etapa.

      > ¿Por qué? [No están finalizadas](https://tc39.github.io/process-document/), y están sujetas a cambios o reescritas completamente. Vamos a usar JavaScript y las propuestas aún no son JavaScript.

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

## Pruebas

  - **Sip**.

    ```javascript
    function foo() {
      return true;
    }
    ```

  - **No, but seriously**:
   - Cualquiera que sea el framework de testing que emplees, ¡deberías escribir tests!
   - Esfuérzate por escribir funciones pequeñas y puras, además de minimizar las posibles mutaciones que pudiesen ocurrir.
   - Sé cuidados con los stubs y los mocks - pueden hacer tus tests más frágiles.
   - Usamos principalmente [`mocha`](https://www.npmjs.com/package/mocha) en Airbnb. [`tape`](https://www.npmjs.com/package/tape) es también usado ocasionalmente para módulos pequeños y separados.
   - 100% de cobertura de pruebas es una buena meta a perseguir, a pesar que no es siempre práctico conseguirlo.
   - Cuando corrijas una incidencia (bug), _escribe una prueba de regresión_. Una incidencia sin una prueba de regresión es casi seguro que volverá a ocurrir en el futuro.

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

## Desempeño

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - [Are Javascript functions like `map()`, `reduce()`, and `filter()` optimized for traversing arrays?](https://www.quora.com/JavaScript-programming-language-Are-Javascript-functions-like-map-reduce-and-filter-already-optimized-for-traversing-array/answer/Quildreen-Motta)
  - Loading...

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**


## Recursos

**Learning ES6**

  - [Draft ECMA 2015 (ES6) Spec](https://people.mozilla.org/~jorendorff/es6-draft.html)
  - [ExploringJS](http://exploringjs.com/)
  - [Tabla de compatibilidad de ES6](https://kangax.github.io/compat-table/es6/)
  - [Vistazo comprensivo de las nuevas características de ES6](http://es6-features.org/)

**Lee esto**

  - [Standard ECMA-262](http://www.ecma-international.org/ecma-262/6.0/index.html)

**Tools**

  Code Style Linters
  - [JSHint](http://www.jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/.jshintrc)
  - [JSCS](https://github.com/jscs-dev/node-jscs) - [Airbnb Style Preset](https://github.com/jscs-dev/node-jscs/blob/master/presets/airbnb.json)

**Otras guías de estilo**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml) (Guía de Estilo de Javascript de Google)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines) (Lineamientos de Estilo con el núcleo de jQuery)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/) (Idiomatic Javascript: Principios de Escritura Consistente)

**Otros estilos**

  - [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen (Nomenclatura en funciones anidadas)
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) (Callbacks condicionales)
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript) (Convenciones Populares de Programación con Javascript en Github)
  - [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman (Múltiples sentencias var en JavaScript, no superfluas)

**Lecturas más profundas**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll (Entendiendo los Closures de JavaScript)
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer (JavaScript Básico para el programador impaciente)
  - [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz (Podrías no necesitar jQuery)
  - [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban (Características de ES6)
  - [Frontend Guidelines](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock (Lineamientos para el Frontend)

**Libros**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford (JavaScript: Las Buenas Partes)
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov (Patrones JavaScript)
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz (Patrones de Diseño Avanzados en Javascript)
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders (Sitios Web de Alto Desempeño: Conocimiento Esencial para los Ingenieros de Capa de Presentación)
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas (JavaScript Mantenible)
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw (Aplicaciones Web JavaScript)
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig (Técnicas Avanzadas JavaScript)
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch (Increíble Node.js: JavaScript en todas partes)
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault (Secretos del JavaScript Ninja)
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg (JavaScript Humano)
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy (Superhéroe.js)
  - [JSBooks](http://jsbooks.revolunet.com/) - Julien Bouquillon
  - [Third Party JavaScript](http://manning.com/vinegar/) - Ben Vinegar and Anton Kovalyov (JavaScript de Terceros)
  - [Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript](http://amzn.com/0321812182) - David Herman (JavaScript Efectivo: 68 modos específicos para elevar el poder de JavaScript)
  - [Eloquent JavaScript](http://eloquentjavascript.net/) - Marijn Haverbeke (JavaScript Elocuente)
  - [You Don't Know JS: ES6 & Beyond](http://shop.oreilly.com/product/0636920033769.do) - Kyle Simpson (No sabes JS: ES6 y más allá)

**Blogs**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)

**Podcasts**

  - [JavaScript Air](https://javascriptair.com/)
  - [JavaScript Jabber](http://devchat.tv/js-jabber/)

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

## En la cancha

  Esta es una lista de las organizaciones que están usando esta guía de estilo. Envíanos un pull request o abre un issue y te agregaremos a la lista.

  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **American Insitutes for Research**: [AIRAST/javascript](https://github.com/AIRAST/javascript)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **DailyMotion**: [dailymotion/javascript](https://github.com/dailymotion/javascript)
  - **Evernote**: [evernote/javascript-style-guide](https://github.com/evernote/javascript-style-guide)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Gawker Media**: [gawkermedia/javascript](https://github.com/gawkermedia/javascript)
  - **GeneralElectric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
  - **reddit**: [reddit/styleguide/javascript](https://github.com/reddit/styleguide/tree/master/javascript)
  - **REI**: [reidev/js-style-guide](https://github.com/reidev/js-style-guide)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **Userify**: [userify/javascript](https://github.com/userify/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

## Traducciones

  Esta guía de estilo es también disponible en otros lenguajes:

  - ![us](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/United-States.png) **Inglés (original)**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Portugués Brasileño**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - ![bg](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bulgaria.png) **Búlgaro**: [borislavvv/javascript](https://github.com/borislavvv/javascript)
  - ![ca](https://raw.githubusercontent.com/fpmweb/javascript-style-guide/master/img/catala.png) **Catalán**: [fpmweb/javascript-style-guide](https://github.com/fpmweb/javascript-style-guide)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chino (Tradicional)**: [jigsawye/javascript](https://github.com/jigsawye/javascript)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chino (Simplificado)**: [sivan/javascript-style-guide](https://github.com/sivan/javascript-style-guide)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **Francés**: [nmussy/javascript-style-guide](https://github.com/nmussy/javascript-style-guide)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **Alemán**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italiano**: [sinkswim/javascript-style-guide](https://github.com/sinkswim/javascript-style-guide)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japonés**: [mitsuruog/javacript-style-guide](https://github.com/mitsuruog/javacript-style-guide)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Coreano**: [tipjs/javascript-style-guide](https://github.com/tipjs/javascript-style-guide)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polaco**: [mjurczyk/javascript](https://github.com/mjurczyk/javascript)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Ruso**: [uprock/javascript](https://github.com/uprock/javascript)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **Tailandés**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide)
  - ![vn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamita**: [giangpii/javascript-style-guide](https://github.com/giangpii/javascript-style-guide)
## La guía de la Guía de Estilos de Javascript

  - [Referencia](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## Charla con nosotros sobre Javascript
  - Encuéntranos en [gitter](https://gitter.im/airbnb/javascript).

## Colaboradores

  - [Vea a los colaboradores](https://github.com/airbnb/javascript/graphs/contributors)


## Licencia

(The MIT License)

Copyright (c) 2014-2016 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

## Enmiendas

Te recomendamos hacer fork de esta guía y cambiar las reglas para que se adecúen a la guía de estilos de tu equipo. Abajo podrás encontrar algunas enmiendas a la guía de estilos. Esto te permitirá actualizar periódicamente tu guía de estilos sin tener que lidiar con conflictos al hacer merge.

# };

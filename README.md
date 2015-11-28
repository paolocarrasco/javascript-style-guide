[![Gitter](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/airbnb/javascript?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

[# Airbnb JavaScript Style Guide() {](https://github.com/airbnb/javascript)

*Un enfoque altamente razonable para JavaScript*


## <a name='TOC'>Tabla de Contenido</a>

  1. [Tipos](#types)
  1. [Objetos](#objects)
  1. [Arreglos](#arrays)
  1. [Cadenas de Texto](#strings)
  1. [Funciones](#functions)
  1. [Propiedades](#properties)
  1. [Variables](#variables)
  1. [Hoisting](#hoisting)
  1. [Expresiones de comparación e igualdad](#conditionals)
  1. [Bloques](#blocks)
  1. [Comentarios](#comments)
  1. [Espacios en blanco](#whitespace)
  1. [Comas](#commas)
  1. [Puntos y Comas](#semicolons)
  1. [Casting de Tipos & Coerción](#type-coercion)
  1. [Convenciones de nomenclatura](#naming-conventions)
  1. [Funciones de Acceso](#accessors)
  1. [Constructores](#constructors)
  1. [Eventos](#events)
  1. [Módulos](#modules)
  1. [jQuery](#jquery)
  1. [Compatibilidad con ES5](#es5)
  1. [Pruebas](#testing)
  1. [Desempeño](#performance)
  1. [Recursos](#resources)
  1. [En la cancha](#in-the-wild)
  1. [Traducciones](#translation)
  1. [La guía de la Guía del Estilo JavaScript](#guide-guide)
  1. [Colaboradores](#contributors)
  1. [Charla con nosotros sobre Javascript](#chat-with-us-about-javascript)
  1. [Licencia](#license)

## <a name='types'>Tipos</a>

  - **Primitivos**: Cuando accesas a un tipo primitivo, manejas directamente su valor

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1;
    var bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **Complejo**: Cuando accesas a un tipo complejo, manejas la referencia a su valor.

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2];
    var bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**

## <a name='objects'>Objetos</a>

  - Usa la sintaxis literal para la creación de un objeto.

    ```javascript
    // mal
    var item = new Object();

    // bien
    var item = {};
    ```

  - No uses [palabras reservadas](http://es5.github.io/#x7.6.1) para nombres de propiedades. No funciona en IE8. [Más información](https://github.com/airbnb/javascript/issues/61)

    ```javascript
    // mal
    var superman = {
      default: { clark: 'kent' },
      private: true
    };

    // bien
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - Usa sinónimos legibles en lugar de palabras reservadas.

    ```javascript
    // mal
    var superman = {
      class: 'alien'
    };

    // mal
    var superman = {
      klass: 'alien'
    };

    // bien
    var superman = {
      type: 'alien'
    };
    ```
    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**

## <a name='arrays'>Arreglos</a>

  - Usa la sintaxis literal para la creación de arreglos

    ```javascript
    // mal
    var items = new Array();

    // bien
    var items = [];
    ```

  - Usa Array#push, en vez de asignación directa, para agregar elementos a un arreglo.

    ```javascript
    var someStack = [];

    // mal
    someStack[someStack.length] = 'abracadabra';

    // bien
    someStack.push('abracadabra');
    ```

  - Cuando necesites copiar un arreglo usa Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length;
    var itemsCopy = [];
    var i;

    // mal
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // bien
    itemsCopy = items.slice();
    ```

  - Para convertir un objeto ["array-like" (similar a un arreglo)](https://www.inkling.com/read/javascript-definitive-guide-david-flanagan-6th/chapter-7/array-like-objects) a un arreglo, usa Array#slice.

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**


## <a name='strings'>Cadenas de Texto</a>

  - Usa comillas simples `''` para las cadenas de texto

    ```javascript
    // mal
    var name = "Bob Parr";

    // bien
    var name = 'Bob Parr';

    // mal
    var fullName = "Bob " + this.lastName;

    // bien
    var fullName = 'Bob ' + this.lastName;
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

  - Cuando se crea programáticamente una cadena de texto, use Array#join en vez de concatenación. Sobretodo por IE: [jsPerf](http://jsperf.com/string-vs-array-concat/2).  

    ```javascript
    var items;
    var messages;
    var length;
    var i;

    messages = [{
      state: 'success',
      message: 'This one worked.'
    },{
      state: 'success',
      message: 'This one worked as well.'
    },{
      state: 'error',
      message: 'This one did not work.'
    }];

    length = messages.length;

    // mal
    function inbox(messages) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // bien
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        // usa asignacion directa aqui porque estamos micro-optimizando
        items[i] = '<li>' + messages[i].message + '</li>';
      }

      return '<ul>' + items.join('') + '</ul>';
    }
    ```

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**


## <a name='functions'>Funciones</a>

  - Expresiones de función:

    ```javascript
    // expresion de funcion anonima
    var anonymous = function() {
      return true;
    };

    // expresion de funcion nombrada
    var named = function named() {
      return true;
    };

    // expresion de funcion inmediatamente invocada (IIFE)
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
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
    var test;
    if (currentUser) {
      test = function test() {
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

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**



## <a name='properties'>Propiedades</a>

  - Usa la notación de punto `.` cuando accedas a las propiedades.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // mal
    var isJedi = luke['jedi'];

    // bien
    var isJedi = luke.jedi;
    ```

  - Usa la notación subscript `[]` cuando accedas a las propiedades con una variable.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**


## <a name='variables'>Variables</a>

  - Siempre usa `var` para declarar variables. No hacerlo resultará en variables globales. Debemos evitar contaminar el espacio global (global namespace). El [Capitán Planeta](https://es.wikipedia.org/wiki/Capit%C3%A1n_Planeta_y_los_planetarios) nos advirtió de eso.

    ```javascript
    // mal
    superPower = new SuperPower();

    // bien
    var superPower = new SuperPower();
    ```

  - Usa una declaración `var` por variable. Es más fácil agregar nuevas declaraciones de variables de este modo, y no tendrás que preocuparte por reemplazar `;` por `,` o introducir diffs de sólo puntuación .

    ```javascript
    // mal
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // mal
    // (compara con lo de arriba y encuentra el error)
    var items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // bien
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    ```

  - Declara a las variables sin asignación al final. Esto es útil cuando necesites asignar una variable luego dependiendo de una de las variables asignadas previamente.

    ```javascript
    // mal
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // mal
    var i;
    var items = getItems();
    var dragonball;
    var goSportsTeam = true;
    var len;

    // bien
    var items = getItems();
    var goSportsTeam = true;
    var dragonball;
    var length;
    var i;
    ```

  - Asigna las variables al inicio de su ámbito. Esto ayuda a evitar inconvenientes con la declaración de variables y temas relacionados a 'hoisting'.

    ```javascript
    // mal
    function() {
      test();
      console.log('doing stuff..');

      //..otras cosas..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // bien
    function() {
      var name = getName();

      test();
      console.log('doing stuff..');

      //..otras cosas..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // mal - llamada a funcion innecesaria
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      this.setFirstName(name);

      return true;
    }

    // bien
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();
      this.setFirstName(name);

      return true;
    }
    ```

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**


## <a name='hoisting'>Hoisting</a>

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

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**



## <a name='conditionals'>Expresiones de comparación e igualdad</a>

  - Usa `===` y `!==` en vez de `==` y `!=` respectivamente.
  - Expresiones condicionales son evaluadas usando coerción con el método `ToBoolean` y siempre obedecen a estas reglas sencillas:

    + **Objects** son evaluados como **true** (también considera así al objeto vacío ```{}``` y arreglos sin contenido ```[]```)
    + **Undefined** es evaluado como **false**
    + **Null** es evaluado como **false**
    + **Booleans** son evaluados como **el valor del booleano**
    + **Numbers** son evaluados como **false** si **+0**, **-0**, o **NaN**, de otro modo **true**
    + **Strings** son evaluados como **false** si es una cadena de texto vacía `''`, de otro modo son **true**

    ```javascript
    if ([0]) {
      // true
      // un arreglo es un objeto, los objetos son evaluados como true
    }
    ```

  - Usa atajos.

    ```javascript
    // mal
    if (name !== '') {
      // ...stuff...
    }

    // bien
    if (name) {
      // ...stuff...
    }

    // mal
    if (collection.length > 0) {
      // ...stuff...
    }

    // bien
    if (collection.length) {
      // ...stuff...
    }
    ```

  - Para más información revisa [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) por Angus Croll

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**


## <a name='blocks'>Bloques</a>

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

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**


## <a name='comments'>Comentarios</a>

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

  - Usa `//` para comentarios de una sola línea. Ubica los comentarios de una sola línea encima de la sentencia comentada. Deja una línea en blanco antes del comentario.

    ```javascript
    // mal
    var active = true;  // is current tab

    // bien
    // is current tab
    var active = true;

    // mal
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }

    // bien
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

  - Agregando a tus comentarios los prefijos `FIXME` o `TODO`, ayudará a otros desarrolladores a entender rápidamente si estás apuntando a un problema que precisa ser revisado o si estás sugiriendo una solución al problema que debería ser implementado. Estos son diferentes a comentarios regulares en el sentido que requieren alguna acción. Las acciones son `FIXME -- necesito resolver esto` o `TODO -- necesita implementarse`.

  - Usa `// FIXME:` para anotar problemas.

    ```javascript
    function Calculator() {

      // FIXME: shouldn't use a global here
      total = 0;

      return this;
    }
    ```

  - Usa `// TODO:` para anotar soluciones a los problemas.

    ```javascript
    function Calculator() {

      // TODO: total should be configurable by an options param
      this.total = 0;

      return this;
    }
  ```

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**


## <a name='whitespace'>Espacios en blanco</a>

  - Usa indentaciones blandas (sin TAB)  establecidas en dos espacios.

    ```javascript
    // mal
    function() {
    ∙∙∙∙var name;
    }

    // mal
    function() {
    ∙var name;
    }

    // bien
    function() {
    ∙∙var name;
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
    var x=y+5;

    // bien
    var x = y + 5;
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

  - Usa indentación cuando uses métodos largos con 'chaining'. Emplea un punto adelante en cada nueva línea, lo que enfatiza que es un método llamado no una nueva sentencia.

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
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // bien
    var leds = stage.selectAll('.led')
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
    // bad
    if (foo) {
      return bar;
    }
    return baz;

    // good
    if (foo) {
      return bar;
    }

    return baz;

    // bad
    var obj = {
      foo: function() {
      },
      bar: function() {
      }
    };
    return obj;

    // good
    var obj = {
      foo: function() {
      },

      bar: function() {
      }
    };

    return obj;
    ```

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**

## <a name='commas'>Comas</a>

  - Comas al inicio de línea: **Nop.**

    ```javascript
    // mal
    var story = [
        once
      , upon
      , aTime
    ];

    // bien
    var story = [
      once,
      upon,
      aTime
    ];

    // mal
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // bien
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

  - Coma adicional al final: **Nop.** Esto puede provocar problemas en IE6/7 o IE9 si está en quirksmode. Además, en algunas implementaciones de ES3 se puede aumentar la longitud del arreglo si se tiene una coma adicional al final. Esto fue clarificado en ES5 ([fuente](http://es5.github.io/#D)):

  > La Edición 5 aclara el hecho de que dejar una coma al final de un ArrayInitialiser (inicialización de un arreglo) no aumenta la longitud del arreglo. Esto no es un cambio semántico a la Edición 3 pero algunas implementaciones tal vez malinterpretaron esto.

    ```javascript
    // mal
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // bien
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**


## <a name='semicolons'>Puntos y Comas</a>

  - **Sip.**

    ```javascript
    // mal
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // bien
    (function() {
      var name = 'Skywalker';
      return name;
    })();

    // super bien (evita que la funcion se vuelva un argumento
    // cuando dos archivos con IIFEs sean concatenados)
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**


## <a name='type-coercion'>Casting de Tipos & Coerción</a>

  - Ejecuta coerción al inicio de una sentencia.
  - Strings:

    ```javascript
    //  => this.reviewScore = 9;

    // mal
    var totalScore = this.reviewScore + '';

    // bien
    var totalScore = '' + this.reviewScore;

    // mal
    var totalScore = '' + this.reviewScore + ' total score';

    // bien
    var totalScore = this.reviewScore + ' total score';
    ```

  - Usa `parseInt` para números y siempre con la base numérica para el casting de tipo.

    ```javascript
    var inputValue = '4';

    // mal
    var val = new Number(inputValue);

    // mal
    var val = +inputValue;

    // mal
    var val = inputValue >> 0;

    // mal
    var val = parseInt(inputValue);

    // bien
    var val = Number(inputValue);

    // bien
    var val = parseInt(inputValue, 10);
    ```

  - Si por alguna razón estás haciendo algo salvaje y `parseInt` es un cuello de botella por lo que necesitaste usar Bitshift por [razones de desempeño](http://jsperf.com/coercion-vs-casting/3), deja un comentario explicando qué y porqué lo estás haciendo.

    ```javascript
    // bien
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    var val = inputValue >> 0;
    ```

  > **Nota:** Ten mucho cuidado al hacer operaciones de Bitshift. En Javascript los números son representados como [valores de 64-bit](http://es5.github.io/#x4.3.19), sin embargo las operaciones de Bitshift siempre retornan un entero de 32-bits ([fuente](http://es5.github.io/#x11.7)). Bitshift puede presentarnos un comportamiento inesperado para valores enteros mayores a 32 bits. [Discusión](https://github.com/airbnb/javascript/issues/109). El mayor entero con signo de 32 bits es 2,147,483,647:
  ```javascript
  2147483647 >> 0 //=> 2147483647
  2147483648 >> 0 //=> -2147483648
  2147483649 >> 0 //=> -2147483647
  ```

  - Booleans:

    ```javascript
    var age = 0;

    // mal
    var hasAge = new Boolean(age);

    // bien
    var hasAge = Boolean(age);

    // bien
    var hasAge = !!age;
    ```

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**


## <a name='naming-conventions'>Convenciones de nomenclatura</a>

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
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    var o = {};
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

    var bad = new user({
      name: 'nope'
    });

    // bien
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  - Usa un guión bajo `_` adelante de la variable cuando nombres propiedades privadas.

    ```javascript
    // mal
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // bien
    this._firstName = 'Panda';
    ```

  - Cuando guardes una referencia a `this` usa `_this`.

    ```javascript
    // mal
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }

    // mal
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // bien
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }
    ```

  - Nombra tus funciones. Esto será de ayuda cuando hagas seguimiento de la pila de llamadas (e.g. en caso de errores).

    ```javascript
    // mal
    var log = function(msg) {
      console.log(msg);
    };

    // bien
    var log = function log(msg) {
      console.log(msg);
    };
    ```

  > **Nota:**  En IE8 e inferiores se tienen algunas inconveniencias con las expresiones de función nombradas. Mira http://kangax.github.io/nfe/ para más información.

  - Si tu archivo exporta una sola clase, el nombre de tu archivo debe ser exactamente el nombre de tu clase.

    ```javascript
    // contenido del archivo
    class CheckBox {
      // ...
    }
    module.exports = CheckBox;

    // en algun otro archivo
    // mal
    var CheckBox = require('./checkBox');

    // mal
    var CheckBox = require('./check_box');

    // bien
    var CheckBox = require('./CheckBox');
    ```

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**


## <a name='accessors'>Funciones de Acceso</a>

  - Funciones de acceso para las propiedades no son requeridas.
  - Si creas funciones de acceso usa  ```getVal()``` y ```setVal('hello')```.

    ```javascript
    // mal
    dragon.age();

    // bien
    dragon.getAge();

    // mal
    dragon.age(25);

    // bien
    dragon.setAge(25);
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
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function(key) {
      return this[key];
    };
    ```

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**


## <a name='constructors'>Constructores</a>

  - Asigna métodos al objeto prototype, en vez de sobreescribir prototype con un nuevo objeto. La sobreescritura de prototype hace la herencia imposible: ¡reseteando prototype sobreescribirás la base!

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // mal
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // bien
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - Métodos pueden retornar `this` para ayudar con el encadenamiento de métodos (chaining).

    ```javascript
    // mal
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20) // => undefined

    // bien
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - Está bien escribir un método toString() personalizado, solo asegúrate que funcione correctamente y no cause efectos colaterales.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**


## <a name='events'>Eventos</a>

  - Cuando envíes paquetes de datos a los eventos (ya sea con eventos del DOM o algo propietario como los eventos de Backbone), pasa un mapa en vez de un valor directo. Esto permitirá a un próximo colaborador a agregar más datos al paquete de datos sin que tenga que encontrar o actualizar un handler para cada evento. Por ejemplo, en vez de:

    ```js
    // mal
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // hacer algo con listingId
    });
    ```

    prefiere:

    ```js
    // bien
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // hacer algo con data.listingId
    });
    ```

  **[[⬆ regresar a la Tabla de Contenido]](#TOC)**


## <a name='modules'>Módulos</a>

  - El módulo debe empezar con un `!`. Esto asegura que si un módulo mal formado olvide incluir al final un punto y coma, no hayan errores en producción cuando los scripts sean concatenados. [Explicación](https://github.com/airbnb/javascript/issues/44#issuecomment-13063933)
  - El archivo debe ser nombrado con camelCase, residir en un fólder con el mismo nombre, y corresponder al nombre de la función a exportar.
  - Agrega un método `noConflict()` que reestablezca el módulo exportado a la versión anterior y retorne este módulo (para ser asignado a una variable).
  - Siempre declara `'use strict';` al inicio de cada módulo.

    ```javascript
    // fancyInput/fancyInput.js

    !function(global) {
      'use strict';

      var previousFancyInput = global.FancyInput;

      function FancyInput(options) {
        this.options = options || {};
      }

      FancyInput.noConflict = function noConflict() {
        global.FancyInput = previousFancyInput;
        return FancyInput;
      };

      global.FancyInput = FancyInput;
    }(this);
    ```

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**


## <a name='jquery'>jQuery</a>

  - Nombre las variables de objetos jQuery con un prefijo `$`.

    ```javascript
    // mal
    var sidebar = $('.sidebar');

    // bien
    var $sidebar = $('.sidebar');
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
      var $sidebar = $('.sidebar');
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

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**


## <a name='es5'>Compatibilidad con ECMAScript 5</a>

  - Revisa la [tabla de compatibilidad](http://kangax.github.com/es5-compat-table/) de ES5 de [Kangax](https://twitter.com/kangax/).

  **[[⬆]](#TOC)**


## <a name='testing'>Pruebas</a>

  - **Sip**.

    ```javascript
    function() {
      return true;
    }
    ```

    **[[⬆ regresar a la Tabla de Contenido]](#TOC)**


## <a name='performance'>Desempeño</a>

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...

  **[[⬆ regresar a la Tabla de Contenido]](#TOC)**


## <a name='resources'>Recursos</a>


**Lee esto**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**Tools**

  Code Style Linters
  - [JSHint](http://www.jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/jshintrc)
  - [JSCS](https://github.com/jscs-dev/node-jscs) - [Airbnb Style Preset](https://github.com/jscs-dev/node-jscs/blob/master/presets/airbnb.json)

**Otras guías de estilo**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml) (Guía de Estilo de Javascript de Google)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines) (Lineamientos de Estilo con el núcleo de jQuery)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/) (Idiomatic Javascript: Principios de Escritura Consistente)
  - [JavaScript Standard Style](https://github.com/feross/standard) (Estilo estándar de JavaScript)

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
  - [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS) - Kyle Simpson (No sabes JS)

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

  - [JavaScript Jabber](http://devchat.tv/js-jabber/)

  **[[⬆ regresar a la Tabla de Contenido]](#TOC)**

## <a name='in-the-wild'>En la cancha</a>

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

## <a name='translation'>Traducciones</a>

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

## <a name='guide-guide'>La guía de la Guía de Estilos de Javascript</a>

  - [Referencia](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## <a name='chat-with-us-about-javascript'>Charla con nosotros sobre Javascript</a>
  - Encuéntranos en [gitter](https://gitter.im/airbnb/javascript).

## <a name='authors'>Colaboradores</a>

  - [Vea a los colaboradores](https://github.com/airbnb/javascript/graphs/contributors)


## <a name='license'>Licencia</a>

(The MIT License)

Copyright (c) 2012 Airbnb

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

**[[⬆ regresar a la Tabla de Contenido]](#TOC)**

# };

[![Gitter](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/airbnb/javascript?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

# Airbnb JavaScript Guida allo Stile() {

*Un approccio più o meno ragionevole a Javascript*


## Tavola dei contenuti

  1. [Tipi](#tipi)
  1. [Oggetti](#oggetti)
  1. [Arrays](#arrays)
  1. [Stringhe](#stringhe)
  1. [Funzioni](#funzioni)
  1. [Proprietà](#proprietà)
  1. [Variabili](#variabili)
  1. [Hoisting](#hoisting)
  1. [Operatori di Confronto ed Ugualianza](#operatori-di-confronto-ed-ugualianza)
  1. [Blocchi](#blocchi)
  1. [Commenti](#commenti)
  1. [Spazi bianchi](#spazi-bianchi)
  1. [Virgole](#virgole)
  1. [Punti e Virgole](#punti-e-virgole)
  1. [Conversioni di Tipo e Coercizioni](#conversioni-di-tipo-e-coercizioni)
  1. [Convenzioni sui Nomi](#convenzioni-sui-nomi)
  1. [Accessors](#accessors)
  1. [Costruttori](#costruttori)
  1. [Eventi](#eventi)
  1. [Moduli](#moduli)
  1. [jQuery](#jquery)
  1. [Compatibilità ECMAScript 5](#compatibilità-ecmascript-5)
  1. [Testing](#testing)
  1. [Performance](#performance)
  1. [Risorse](#risorse)
  1. [In Piena Libertà](#in-piena-libertà)
  1. [Traduzioni](#traduzioni)
  1. [Guida sulla Guida allo Stile di Javascript](#guida-sulla-guida-allo-stile-di-javascript)
  1. [Chatta con Noi di Javascript](#chatta-con-noi-di-javascript)
  1. [Collaboratori](#collaboratori)
  1. [Licensa](#licensa)

## Tipi

  - **Primitivi**: Quando accedi ad un tipo primitivo lavori direttamente sul suo valore.

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
  - **Complessi**: Quando accedi un tipo complesso lavori su un riferimento al suo valore.

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2];
    var bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ torna in cima](#tavola-dei-contenuti)**

## Oggetti

  - Usa la sintassi letterale per la creazione di oggetti.

    ```javascript
    // errato
    var oggetto = new Object();

    // corretto
    var oggetto = {};
    ```

  - Non usare [parole riservate](http://es5.github.io/#x7.6.1) come chiavi. Non funzionerà su IE8. [Maggiori informazioni](https://github.com/airbnb/javascript/issues/61).

    ```javascript
    // errato
    var superman = {
      default: { clark: 'kent' },
      private: true
    };

    // corretto
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - Usa sinonimi leggibili al posto di parole riservate.

    ```javascript
    // errato
    var superman = {
      class: 'alieno'
    };

    // errato
    var superman = {
      klass: 'alieno'
    };

    // corretto
    var superman = {
      type: 'alieno'
    };
    ```

**[⬆ torna in cima](#tavola-dei-contenuti)**

## Arrays

  - Usa la sintassi letterale per la creazione di array.

    ```javascript
    // errato
    var oggetti = new Array();

    // corretto
    var oggetti = [];
    ```

  - Usa Array#push invece usare l'assegnamento diretto per aggiungere oggetti ad un array.

    ```javascript
    var someStack = [];


    // errato
    someStack[someStack.length] = 'abracadabra';

    // corretto
    someStack.push('abracadabra');
    ```

  - Quando devi copiare un array usa Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = oggetti.length;
    var itemsCopy = [];
    var i;

    // errato
    for (i = 0; i < len; i++) {
      itemsCopy[i] = oggetti[i];
    }

    // corretto
    itemsCopy = oggetti.slice();
    ```

  - Per convertire un oggetti simil-array, usa Array#slice.

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

**[⬆ torna in cima](#tavola-dei-contenuti)**


## Stringhe

  - Usa singoli apici `''` per le stringhe.

    ```javascript
    // errato
    var nome = "Bob Parr";

    // corretto
    var nome = 'Bob Parr';

    // errato
    var nomeCompleto = "Bob " + this.cognome;

    // corretto
    var nomeCompleto = 'Bob ' + this.cognome;
    ```

  - Stringhe più lunghe di 80 caratteri devono essere scritte su più righe usando la concatenazione.
  - Nota: Se usato troppo, stringhe lunghe con concatenazione possono intaccare la performance. [jsPerf](http://jsperf.com/ya-string-concat) e [Discussione](https://github.com/airbnb/javascript/issues/40).

    ```javascript
    // errato
    var errorMessage = 'Questo è un errore molto lungo che è stato lanciato a causa di Batman. Quando smetti di pensare al fatto che Batman abbia potuto avere a che fare con questo, non arriverai da nessuna parte.';

    // errato
    var errorMessage = 'Questo e un errore molto lungo che e stato lanciato a causa\
        di Batman. Quando smetti di pensare al fatto che Batman abbia potuto\
        avere a che fare con questo, non arriverai da nessuna\
        parte.';

    // corretto
    var errorMessage = 'Questo e un errore molto lungo che e stato lanciato a causa ' +
      'di Batman. Quando smetti di pensare al fatto che Batman abbia potuto ' +
      'avere a che fare con questo, non arriverai da nessuna parte.';
    ```

  - Quando devi costruire una stringa programmaticamente, usa Array#join invece della concatenazione. Soprattutto per IE: [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var oggetti;
    var messaggi;
    var lunghezza;
    var i;

    messaggi = [{
      stato: 'successo',
      messaggio: 'Questo ha funzionato.'
    }, {
      stato: 'successo',
      messaggio: 'Anche questo ha funzionato.'
    }, {
      stato: 'errore',
      messaggio: 'Questo non ha funzionato.'
    }];

    lunghezza = messaggi.length;

    // errato
    function inbox(messaggi) {
      oggetti = '<ul>';

      for (i = 0; i < lunghezza; i++) {
        oggetti += '<li>' + messaggi[i].messaggio + '</li>';
      }

      return oggetti + '</ul>';
    }

    // corretto
    function inbox(messaggi) {
      oggetti = [];

      for (i = 0; i < lunghezza; i++) {
        oggetti[i] = '<li>' + messaggi[i].messaggio + '</li>';
      }

      return '<ul>' + oggetti.join('') + '</ul>';
    }
    ```

**[⬆ torna in cima](#tavola-dei-contenuti)**


## Funzioni

  - Espressione delle funzioni:

    ```javascript
    // espressione di una funzione anonima
    var anonima = function() {
      return true;
    };

    // epressione di una funziona con nome
    var conNome = function conNome() {
      return true;
    };

    // espressione di funzioni invocate immediatamente (immediately-invoked function expression (IIFE))
    (function() {
      console.log('Benvenuto in Internet. Perfavore, seguimi.');
    })();
    ```

  - Non dichiarare mai una funzione in un blocco non-funzione (if, while, ecc). Invece, assegna la funzione ad una variabile. I browsers ti permetteranno di farlo, ma ciascuno lo interpreterà diversamente.
  - **Nota:** ECMA-262 definisce un `block` (blocco) come una lista di statements. Una dichiarazione di una funzione non è uno statement. [Leggi la nota di ECMA-262 su questa questione](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // errato
    if (utenteCorrente) {
      function test() {
        console.log('No.');
      }
    }

    // corretto
    var test;
    if (utenteCorrente) {
      test = function test() {
        console.log('Si.');
      };
    }
    ```

  - Non nominare mai un parametro `arguments`. Questo avrà la precedenza sull'oggetto `arguments` che viene dato in ogni ambito di visibilità (scope) di una funzione.

    ```javascript
    // errato
    function no(name, options, arguments) {
      // ...codice...
    }

    // corretto
    function si(name, options, args) {
      // ...codice...
    }
    ```

**[⬆ torna in cima](#tavola-dei-contenuti)**



## Proprietà

  - Usa la notazione puntata per accedere alle proprietà

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // errato
    var isJedi = luke['jedi'];

    // corretto
    var isJedi = luke.jedi;
    ```

  - Usa la notazione subscript `[]` per accedere a proprietà con una variabile.

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

**[⬆ torna in cima](#tavola-dei-contenuti)**


## Variabili

  - Usa sempre `var` per dichiarare variabili. Altrimenti saranno variabili globali. Bisogna evitare di inquinare lo spazio di nomi globale. Il Capitan Pianeta ci ha avvisati di cio'.

    ```javascript
    // errato
    superPotere = new SuperPotere();

    // corretto
    var superPotere = new SuperPotere();
    ```

  - Usa una dichiarazione `var` per ogni variabile.
    È più facile aggiungere dichiarazioni di variabili in questa maniera, e non devi mai
    preoccuparti di scambiare una `;` con una `,` o intrudurre diffs
    legate solamente alla punteggiatura.

    ```javascript
    // errato
    var oggetti = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // errato
    // (confronta con quanto sopra e cerca di individuare lo sbaglio)
    var oggetti = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // corretto
    var oggetti = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    ```

  - Dichiara variabili senza assegnamento per ultime. Questo è vantaggioso quando in futuro avrai bisogno di assegnare una variabile rispetto ad una variabile assegnata in precedenza. 

    ```javascript
    // errato
    var i, len, dragonball,
        oggetti = getItems(),
        goSportsTeam = true;

    // errato
    var i;
    var oggetti = getItems();
    var dragonball;
    var goSportsTeam = true;
    var len;

    // corretto
    var oggetti = getItems();
    var goSportsTeam = true;
    var dragonball;
    var length;
    var i;
    ```

  - Assegna variabili in cima al loro scope. Questo permette di evitare problemi relativi a dichiarazione di variabili ed assegnamenti.

    ```javascript
    // errato
    function() {
      test();
      console.log('sto lavorando..');

      //..altro codice..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // corretto
    function() {
      var name = getName();

      test();
      console.log('sto lavorando..');

      //..altro codice..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // errato
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // corretto
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();

      return true;
    }
    ```

**[⬆ torna in cima](#tavola-dei-contenuti)**


## Hoisting

  - Le dichiarazioni di variabili vengono sollevate (hoisting) in cima al loro scope, ma non il loro assegnamento.

    ```javascript
    // sappiamo che questo non funziona (assumendo che
    // non sia stata definita la variabile globale notDefined)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // creare una dichiarazione di variabile dopo che hai fatto
    // riferimento alla variabile funzionerà a causa del
    // variable hoisting. Nota: l'assegnamento
    // del valore `true` non è sollevato.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // L'interprete usa hoisting sulla dichiarazione della 
    // variabile in cima allo scope,
    // il che significa il nostro esempio puo' essere riscritto come:
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - Le espressioni di funzioni anonime sollevano il nome della loro variabile, ma non l'assegnamento della funzione.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - Espressioni di funzioni con nomi sollevano il nome della variabile, ma non il nome della funzione o il suo corpo.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // lo stesso è vero quando il nome della funzione
    // è lo stesso del nome della variabile.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - Le dichiarazioni di funzioni sollevano il loro nome ed il corpo della funzione.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - Per maggiori informazioni vedi [JavaScript Scoping & Hoisting](http://www.adequatelycorretto.com/2010/2/JavaScript-Scoping-and-Hoisting) di [Ben Cherry](http://www.adequatelycorretto.com/).

**[⬆ torna in cima](#tavola-dei-contenuti)**



## Operatori di Confronto ed Ugualianza

  - Usa `===` e `!==` anzichè `==` e `!=`.
  - Gli operatori di confronti vengono valutati usando la coercizione con il metodo `ToBoolean` e seguono sempre queste semplici regole:
 
    + **Objects** valgono come **true**
    + **Undefined** valgono come **false**
    + **Null** valgono come **false**
    + **Booleans** valgono come **il valore del booleano**
    + **Numbers** valgono come **false** se **+0, -0, o NaN**, altrimenti **true**
    + **Strings** valgono come **false** se è una stringa vuota `''`, altrimenti **true**

    ```javascript
    if ([0]) {
      // true
      // Un array è un oggetto, gli oggetti valgono come true
    }
    ```

  - Usa scorciatoie.

    ```javascript
    // errato
    if (name !== '') {
      // ...stuff...
    }

    // corretto
    if (name) {
      // ...stuff...
    }

    // errato
    if (collection.length > 0) {
      // ...stuff...
    }

    // corretto
    if (collection.length) {
      // ...stuff...
    }
    ```

  - Per maggiori informazioni vedi [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) di Angus Croll.

**[⬆ torna in cima](#tavola-dei-contenuti)**


## Blocchi

  - Usa parentesi graffe per tutti i blocchi multi-riga.

    ```javascript
    // errato
    if (test)
      return false;

    // corretto
    if (test) return false;

    // corretto
    if (test) {
      return false;
    }

    // errato
    function() { return false; }

    // corretto
    function() {
      return false;
    }
    ```

  - Se stai usando blocchi multi-riga con `if` e `else`, metti `else` sulla stessa riga della parentesi graffa che chiude il blocco `if`.

    ```javascript
    // errato
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // corretto
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```


**[⬆ torna in cima](#tavola-dei-contenuti)**


## Commenti

  - Usa `/** ... */` per commenti multi-riga. Includi una descrizione, specifica i tipi ed i valori per tutti i parametri ed i valori di ritorno.

    ```javascript
    // errato
    // make() ritorna un nuovo elemento
    // basato sul tag name passato
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...codice...

      return element;
    }

    // corretto
    /**
     * make() ritorna un nuovo elemento
     * basato sul tag name passato
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...codice...

      return element;
    }
    ```

  - Usa `//` per commenti da una riga. Poni commenti di questo tipo su una nuova riga sopra il soggetto del commento. Poni una riga vuota prima del commento.

    ```javascript
    // errato
    var active = true;  // is current tab

    // corretto
    // is current tab
    var active = true;

    // errato
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }

    // corretto
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

  - Anteporre i tuoi commenti con `FIXME` o `TODO` aiuta altri sviluppatori a capire rapidamente se stai esponendo un problema che deve essere risolto,
  o se stai suggerendo una soluzione al problema che deve essere implementato. Questi sono diversi da commenti normali perchè sono azionabili.
  Le azioni sono `FIXME -- devo risolvere questo` o `TODO -- va implementato questo`.

  - Usa `// FIXME:` per annotare problemi.

    ```javascript
    function Calculator() {

      // FIXME: non dovrei usare una variabile globale qui
      total = 0;

      return this;
    }
    ```

  - Usa `// TODO:` per annotare soluzioni ad un problema.

    ```javascript
    function Calcolatrice() {

      // TODO: totale dovrebbe essere configurabile da un parametro opzioni
      this.totale = 0;

      return this;
    }
  ```

**[⬆ torna in cima](#tavola-dei-contenuti)**


## Spazi bianchi

  - Usa una indentatura (tabbing) settata a 2 spazi.

    ```javascript
    // errato
    function() {
    ∙∙∙∙var name;
    }

    // errato
    function() {
    ∙var name;
    }

    // corretto
    function() {
    ∙∙var name;
    }
    ```

  - Poni 1 spazio prima della graffa principale.

    ```javascript
    // errato
    function test(){
      console.log('test');
    }

    // corretto
    function test() {
      console.log('test');
    }

    // errato
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // corretto
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```

  - Poni 1 spazio prima della parentesi di apertura degli statement di controllo (`if`, `while` etc.). Non porre alcun spazio prima della lista degli argomenti nelle chiamate di funzioni e nelle dichiarazioni.

    ```javascript
    // errato
    if(isJedi) {
      fight ();
    }

    // corretto
    if (isJedi) {
      fight();
    }

    // errato
    function fight () {
      console.log ('Swooosh!');
    }

    // corretto
    function fight() {
      console.log('Swooosh!');
    }
    ```

  - Separa gli operatori con spazi.

    ```javascript
    // errato
    var x=y+5;

    // corretto
    var x = y + 5;
    ```

  - Finisci i file con un singolo carattere di nuova riga.

    ```javascript
    // errato
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // errato
    (function(global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // corretto
    (function(global) {
      // ...stuff...
    })(this);↵
    ```

  - Usa l'indentatura quando devi fare lunghe catene di metodi. Usa un punto iniziale, il quale
  enfatizza il fatto che la linea è una chiamata ad un metodo, e non un nuovo statement.

    ```javascript
    // errato
    $('#oggetti').find('.selected').highlight().end().find('.open').updateCount();

    // errato
    $('#oggetti').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // corretto
    $('#oggetti')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // errato
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // corretto
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width',  (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

  - Lascia una riga vuota dopo blocchi e prima di un nuovo statement.

    ```javascript
    // errato
    if (foo) {
      return bar;
    }
    return baz;

    // corretto
    if (foo) {
      return bar;
    }

    return baz;

    // errato
    var obj = {
      foo: function() {
      },
      bar: function() {
      }
    };
    return obj;

    // corretto
    var obj = {
      foo: function() {
      },

      bar: function() {
      }
    };

    return obj;
    ```


**[⬆ torna in cima](#tavola-dei-contenuti)**

## Virgole

  - Virgole principali: **No.**

    ```javascript
    // errato
    var story = [
        once
      , upon
      , aTime
    ];

    // corretto
    var story = [
      once,
      upon,
      aTime
    ];

    // errato
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // corretto
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

  - Virgole successive addizionali: **No.** Questo puo' causare problemi con IE6/7 e IE9 se è in modalita' stranezze. Inoltre, in alcune implementazioni di ES3 viene aggiunta lunghezza ad un array se ci sono virgole successive addizionali. Questo è stato chiarificato in ES5 ([source](http://es5.github.io/#D)):

  > Edition 5 chiarifica il fatto che una virgola successiva in fondo ad un ArrayInitialiser non aggiunge nulla alla lunghezza dell'array. Questo non è un cambiamento semantico dalla Edition 3 ma alcune implementazioni passate hanno potenzialmente frainteso cio'

    ```javascript
    // errato
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // corretto
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```

**[⬆ torna in cima](#tavola-dei-contenuti)**


## Punti e virgole

  - **Già.**

    ```javascript
    // errato
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // corretto
    (function() {
      var name = 'Skywalker';
      return name;
    })();

    // corretto (non permette alla funzione di diventare un argomento quando due file con IIFEs vengono concatenati)
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    [Per saperne di più](http://stackoverflow.com/a/7365214/1712802).

**[⬆ torna in cima](#tavola-dei-contenuti)**


## Conversioni di Tipo e Coercizione

  - Effettua la coercizione di tipo all'inizio di uno statement.
  - Strings:

    ```javascript
    //  => this.reviewScore = 9;

    // errato
    var totalScore = this.reviewScore + '';

    // corretto
    var totalScore = '' + this.reviewScore;

    // errato
    var totalScore = '' + this.reviewScore + ' total score';

    // corretto
    var totalScore = this.reviewScore + ' total score';
    ```

  - Usa `parseInt` per Numbers e sempre con una radice per la conversione di tipo.

    ```javascript
    var inputValue = '4';

    // errato
    var val = new Number(inputValue);

    // errato
    var val = +inputValue;

    // errato
    var val = inputValue >> 0;

    // errato
    var val = parseInt(inputValue);

    // corretto
    var val = Number(inputValue);

    // corretto
    var val = parseInt(inputValue, 10);
    ```

  - Se per qualsiasi ragione stai facendo qualcosa di strano e `parseInt` è il tuo collo di bottiglia allora devi usare Bitshift per [motivi di performance](http://jsperf.com/coercion-vs-casting/3), lascia un commento spiegando come mai e cosa stai facendo.

    ```javascript
    // corretto
    /**
     * parseInt è la ragione per la quale il mio codice era lento.
     * Effettuando Bitshifting alla String per forzarla a diventare un
     * Number lo ha reso molto più veloce.
     */
    var val = inputValue >> 0;
    ```

  - **Nota:** Fai attenzione quando usi operazioni di bitshift. I Numbers sono rappresentati come [valori da 64-bit](http://es5.github.io/#x4.3.19), ma le operazioni di Bitshift ritornano sempre un intero da 32-bit ([source](http://es5.github.io/#x11.7)). Il Bitshift puo' portare a comportamenti anomali per valori interi più grandi di 32-bit. [Discussion](https://github.com/airbnb/javascript/issues/109). L'intero con segno a più grande è 2,147,483,647:

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - Booleans:

    ```javascript
    var age = 0;

    // errato
    var hasAge = new Boolean(age);

    // corretto
    var hasAge = Boolean(age);

    // corretto
    var hasAge = !!age;
    ```

**[⬆ torna in cima](#tavola-dei-contenuti)**


## Convenzioni sui Nomi

  - Evita nomi con una singola lettera. Sii descrittivo quando dai nomi.

    ```javascript
    // errato
    function q() {
      // ...stuff...
    }

    // corretto
    function query() {
      // ..stuff..
    }
    ```

  - Usa la notazioneACammello (camelCase) quando dai nomi ad oggetti, funzioni ed istanze.

    ```javascript
    // errato
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c() {}
    var u = new user({
      name: 'Bob Parr'
    });

    // corretto
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - Usa PascalCase quando dai nomi a costruttori o a classi.

    ```javascript
    // errato
    function user(options) {
      this.name = options.name;
    }

    var errato = new user({
      name: 'nope'
    });

    // corretto
    function User(options) {
      this.name = options.name;
    }

    var corretto = new User({
      name: 'yup'
    });
    ```

  - Usa un underscore `_` principale quando dai nomi a proprietà private.

    ```javascript
    // errato
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // corretto
    this._firstName = 'Panda';
    ```

  - Quando salvi un riferimento a `this` usa `_this`.

    ```javascript
    // errato
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }

    // errato
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // corretto
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }
    ```

  - Dai un nome alle tue funzioni. Questo è di aiuto per le stack traces.

    ```javascript
    // errato
    var log = function(msg) {
      console.log(msg);
    };

    // corretto
    var log = function log(msg) {
      console.log(msg);
    };
    ```
  - **Nota:** Da IE8 in giù si manifestano stranezze con espressioni di funzioni a cui è stato dato un nome. vedi [http://kangax.github.io/nfe/](http://kangax.github.io/nfe/) per maggiori informazioni.

  - Se il tuo file esporta una singola classe, il nome del file dovrebbe essere esattamente quello del nome della classe.

    ```javascript
    // file contents
    class CheckBox {
      // ...
    }
    module.exports = CheckBox;

    // in some other file
    // errato
    var CheckBox = require('./checkBox');

    // errato
    var CheckBox = require('./check_box');

    // corretto
    var CheckBox = require('./CheckBox');
    ```

**[⬆ torna in cima](#tavola-dei-contenuti)**


## Accessors

  - Funzioni accessor per proprietà non sono richieste.
  - Se usi funzioni accessor usa getVal() e setVal('hello').

    ```javascript
    // errato
    dragon.age();

    // corretto
    dragon.getAge();

    // errato
    dragon.age(25);

    // corretto
    dragon.setAge(25);
    ```

  - Se la proprietà è un booleano, usa isVal() o hasVal().

    ```javascript
    // errato
    if (!dragon.age()) {
      return false;
    }

    // corretto
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - Va bene creare funzioni get() e set(), ma sii consistente.

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

**[⬆ torna in cima](#tavola-dei-contenuti)**


## Costruttori

  - Assegna metodi all'oggetto prototype, invece di sovrascrivere il prototipo con un nuovo oggetto. Sovrascrivere il prototipo rende l'ereditarietà
  impossibile: resettando il prototipo sovrascriverai la base!

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // errato
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // corretto
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - Metodi possono ritornare `this` per aiutare il concatenamento di metodi.

    ```javascript
    // errato
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // corretto
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


  - Va bene scrivere un metodo toString() personalizzato, basta essere sicuri che funzioni correttamente e che non causi effetti indesiderati.

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

**[⬆ torna in cima](#tavola-dei-contenuti)**


## Eventi

  - Quando alleghi data payloads ad eveti (non importa se eventi DOM o qualcosa di più prorietario come eventi Backbone), passa un hash invece di una valore semplice. Questo permette ad un collaboratore futuro di aggiungere più data al payload dell'evento senza trovare ed aggiornare ogni handler per l'evento. Ad esempio, invece di

    ```js
    // errato
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    prefer:

    ```js
    // corretto
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

  **[⬆ torna in cima](#tavola-dei-contenuti)**


## Moduli

  -Il modulo dovrebbe iniziare con un `!`. Questo assicura che se un modulo malformato dimentica di includere un punto e virgola finale allora non ci sono errori di produzione quando gli script vengono concatenati. [Spiegazione](https://github.com/airbnb/javascript/issues/44#issuecomment-13063933)
  - Il file deve essere nominato concamelCase, essere in una cartella con lo stesso nome, and match the name of the single export.
  - Aggiungi un metodo chiamato `noConflict()` che setta il modulo esportato alla versione precedente e ritorna quello in questione.
  - Dichiara sempre `'use strict';` in cima al modulo.

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

**[⬆ torna in cima](#tavola-dei-contenuti)**


## jQuery

  - Anteponi le variabili di oggetti jQuery con `$`.

    ```javascript
    // errato
    var sidebar = $('.sidebar');

    // corretto
    var $sidebar = $('.sidebar');
    ```

  - Cache jQuery lookups.

    ```javascript
    // errato
    function setSidebar() {
      $('.sidebar').hide();

      // ...codice...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // corretto
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...codice...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - Per query del DOM usa a cascata `$('.sidebar ul')` o parent > child `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - Usa `find` con scoped query di oggetti jQuery.

    ```javascript
    // errato
    $('ul', '.sidebar').hide();

    // errato
    $('.sidebar').find('ul').hide();

    // corretto
    $('.sidebar ul').hide();

    // corretto
    $('.sidebar > ul').hide();

    // corretto
    $sidebar.find('ul').hide();
    ```

**[⬆ torna in cima](#tavola-dei-contenuti)**


## Compatibilità ECMAScript 5

  - Fai riferimento a ES5 [tavola delle compatibilità](http://kangax.github.com/es5-compat-table/) di [Kangax](https://twitter.com/kangax/).

**[⬆ torna in cima](#tavola-dei-contenuti)**


## Testing

  - **Già.**

    ```javascript
    function() {
      return true;
    }
    ```

**[⬆ torna in cima](#tavola-dei-contenuti)**


## Performance

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Caricamento...

**[⬆ torna in cima](#tavola-dei-contenuti)**


## Risorse


**Leggi Questo**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**Strumenti**

  - Code Style Linters
    + [JSHint](http://www.jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/jshintrc)
    + [JSCS](https://github.com/jscs-dev/node-jscs) - [Airbnb Style Preset](https://github.com/jscs-dev/node-jscs/blob/master/presets/airbnb.json)

**Altre Guide allo Stile**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)
  - [JavaScript Standard Style](https://github.com/feross/standard)

**Altri Stili**

  - [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) - Ross Allen
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
  - [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**Letture Ulteriori**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
  - [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
  - [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban
  - [Frontend Guidelines](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock

**Libri**

  - [JavaScript: The corretto Parts](http://www.amazon.com/JavaScript-corretto-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/) - Julien Bouquillon
  - [Third Party JavaScript](http://manning.com/vinegar/) - Ben Vinegar and Anton Kovalyov
  - [Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript](http://amzn.com/0321812182) - David Herman
  - [Eloquent JavaScript](http://eloquentjavascript.net) - Marijn Haverbeke
  - [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS) - Kyle Simpson

**Blog**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately corretto](http://www.adequatelycorretto.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)

**Podcast**

  - [JavaScript Jabber](http://devchat.tv/js-jabber/)


**[⬆ torna in cima](#tavola-dei-contenuti)**

## In Piena Libertà

  Questa è una lista di organizzazioni che fanno uso di questa guida allo stile. Mandaci una pull request o apri una issue e ti aggiungeremo alla lista.

  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Adult Swim**: [adult-swim/javascript](https://github.com/adult-swim/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **American Insitutes for Research**: [AIRAST/javascript](https://github.com/AIRAST/javascript)
  - **Apartmint**: [apartmint/javascript](https://github.com/apartmint/javascript)
  - **Avalara**: [avalara/javascript](https://github.com/avalara/javascript)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **DailyMotion**: [dailymotion/javascript](https://github.com/dailymotion/javascript)
  - **Digitpaint** [digitpaint/javascript](https://github.com/digitpaint/javascript)
  - **Evernote**: [evernote/javascript-style-guide](https://github.com/evernote/javascript-style-guide)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Gawker Media**: [gawkermedia/javascript](https://github.com/gawkermedia/javascript)
  - **GeneralElectric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **correttoData**: [correttodata/gdc-js-style](https://github.com/correttodata/gdc-js-style)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript)
  - **InfoJobs**: [InfoJobs/JavaScript-Style-Guide](https://github.com/InfoJobs/JavaScript-Style-Guide)
  - **Intent Media**: [intentmedia/javascript](https://github.com/intentmedia/javascript)
  - **Jam3**: [Jam3/Javascript-Code-Conventions](https://github.com/Jam3/Javascript-Code-Conventions)
  - **Kinetica Solutions**: [kinetica/javascript](https://github.com/kinetica/javascript)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **Money Advice Service**: [moneyadviceservice/javascript](https://github.com/moneyadviceservice/javascript)
  - **Muber**: [muber/javascript](https://github.com/muber/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
  - **Nimbl3**: [nimbl3/javascript](https://github.com/nimbl3/javascript)
  - **Nordic Venture Family**: [CodeDistillery/javascript](https://github.com/CodeDistillery/javascript)
  - **Orion Health**: [orionhealth/javascript](https://github.com/orionhealth/javascript)
  - **Peerby**: [Peerby/javascript](https://github.com/Peerby/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **reddit**: [reddit/styleguide/javascript](https://github.com/reddit/styleguide/tree/master/javascript)
  - **REI**: [reidev/js-style-guide](https://github.com/reidev/js-style-guide)
  - **Ripple**: [ripple/javascript-style-guide](https://github.com/ripple/javascript-style-guide)
  - **SeekingAlpha**: [seekingalpha/javascript-style-guide](https://github.com/seekingalpha/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **StudentSphere**: [studentsphere/javascript](https://github.com/studentsphere/javascript)
  - **Target**: [target/javascript](https://github.com/target/javascript)
  - **TheLadders**: [TheLadders/javascript](https://github.com/TheLadders/javascript)
  - **T4R Technology**: [T4R-Technology/javascript](https://github.com/T4R-Technology/javascript)
  - **Userify**: [userify/javascript](https://github.com/userify/javascript)
  - **VoxFeed**: [VoxFeed/javascript-style-guide](https://github.com/VoxFeed/javascript-style-guide)
  - **Weggo**: [Weggo/javascript](https://github.com/Weggo/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

## Traduzioni

  Questa guida allo stile è disponibile anche in altre lingue:

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Portoghese Braziliano**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - ![bg](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bulgaria.png) **Bulgaro**: [borislavvv/javascript](https://github.com/borislavvv/javascript)
  - ![ca](https://raw.githubusercontent.com/fpmweb/javascript-style-guide/master/img/catala.png) **Catalano**: [fpmweb/javascript-style-guide](https://github.com/fpmweb/javascript-style-guide)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Cinese(Tradizionale)**: [jigsawye/javascript](https://github.com/jigsawye/javascript)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Cinese(Semplificato)**: [adamlu/javascript-style-guide](https://github.com/adamlu/javascript-style-guide)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **Francese**: [nmussy/javascript-style-guide](https://github.com/nmussy/javascript-style-guide)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **Tedesco**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italiano**: [sinkswim/javascript-style-guide](https://github.com/sinkswim/javascript-style-guide)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Giapponese**: [mitsuruog/javacript-style-guide](https://github.com/mitsuruog/javacript-style-guide)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Coreano**: [tipjs/javascript-style-guide](https://github.com/tipjs/javascript-style-guide)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polacco**: [mjurczyk/javascript](https://github.com/mjurczyk/javascript)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russo**: [uprock/javascript](https://github.com/uprock/javascript)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spagnolo**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **Tailandese**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide)

## Guida sulla Guida allo Stile di Javascript

  - [Riferimento](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## Chatta con noi di Javascript

  - Trovaci su [gitter](https://gitter.im/airbnb/javascript).

## Collaboratori

  - [Visualizza i collaboratori](https://github.com/airbnb/javascript/graphs/contributors)


## Licensa

(The MIT License)

Copyright (c) 2014 Airbnb

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

**[⬆ torna in cima](#tavola-dei-contenuti)**

# };

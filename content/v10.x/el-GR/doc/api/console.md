# Κονσόλα

<!--introduced_in=v0.10.13-->

> Σταθερότητα: 2 - Σταθερό

Η ενότητα `console` παρέχει μια απλή κονσόλα αποσφαλμάτωσης που είναι παρόμοια με τον μηχανισμό κονσόλας της JavaScript που παρέχεται από τους περιηγητές.

Η ενότητα εξάγει δύο συγκεκριμένα μέρη:

* Μια κλάση `Console` με μεθόδους όπως τα `console.log()`, `console.error()` και `console.warn()` που μπορούν να χρησιμοποιηθούν για να γράψουν σε οποιαδήποτε ροή της Node.js.
* Ένα καθολικό στιγμιότυπο του `console` που έχει ρυθμιστεί για να γράφει στα [`process.stdout`][] και [`process.stderr`][]. Το καθολικό `console` μπορεί να χρησιμοποιείται και χωρίς να γίνει κλήση του `require('console')`.

***Προσοχή***: Οι μέθοδοι αντικειμένων της καθολικής κονσόλας δεν είναι ούτε συνεπώς σε συγχρονισμό όπως τα API των περιηγητών που παρομοιάζουν, ούτε είναι συνεπώς ασύγχρονα όπως όλες οι άλλες ροές της Node.js. Δείτε την [σημείωση για το I/O των διαδικασιών](process.html#process_a_note_on_process_i_o) για περισσότερες πληροφορίες.

Παράδειγμα χρήσης του καθολικού `console`:

```js
console.log('hello world');
// Τυπώνει: hello world, στο stdout
console.log('hello %s', 'world');
// Τυπώνει: hello world, στο stdout
console.error(new Error('Whoops, something bad happened'));
// Τυπώνει: [Error: Whoops, something bad happened], στο stderr

const name = 'Will Robinson';
console.warn(`Danger ${name}! Danger!`);
// Τυπώνει: Danger Will Robinson! Danger!, στο stderr
```

Παράδειγμα χρησιμοποιώντας την κλάση `Console`:

```js
const out = getStreamSomehow();
const err = getStreamSomehow();
const myConsole = new console.Console(out, err);

myConsole.log('hello world');
// Τυπώνει: hello world, στο out
myConsole.log('hello %s', 'world');
// Τυπώνει: hello world, στο out
myConsole.error(new Error('Whoops, something bad happened'));
// Τυπώνει [Error: Whoops, something bad happened], στο err

const name = 'Will Robinson';
myConsole.warn(`Danger ${name}! Danger!`);
// Τυπώνει: Danger Will Robinson! Danger!, στο err
```

## Class: Console

<!-- YAML
changes:

  - version: v8.0.0
    pr-url: https://github.com/nodejs/node/pull/9744
    description: Errors that occur while writing to the underlying streams
                 will now be ignored by default.
-->

<!--type=class-->

Η κλάση `Console` μπορεί να χρησιμοποιηθεί για τη δημιουργία ενός απλού καταγραφέα με ρυθμιζόμενες ροές εξόδου, και μπορεί να χρησιμοποιηθεί είτε το `require('console').Console` ή το `console.Console` (ή οι αδόμητες αντιστοιχίες τους) για να αποκτηθεί πρόσβαση σε αυτόν:

```js
const { Console } = require('console');
```

```js
const { Console } = console;
```

### new Console(stdout\[, stderr\]\[, ignoreErrors\])

### new Console(options)

<!-- YAML
changes:

  - version: v8.0.0
    pr-url: https://github.com/nodejs/node/pull/9744
    description: The `ignoreErrors` option was introduced.
  - version: v10.0.0
    pr-url: https://github.com/nodejs/node/pull/19372
    description: The `Console` constructor now supports an `options` argument,
                 and the `colorMode` option was introduced.
-->

* `options` {Object} 
  * `stdout` {stream.Writable}
  * `stderr` {stream.Writable}
  * `ignoreErrors` {boolean} Αγνοεί τα σφάλματα όταν γίνεται εγγραφή στις υποκείμενες ροές. **Προεπιλογή:** `true`.
  * `colorMode` {boolean|string} Ορίζει την υποστήριξη χρωμάτων για αυτό το στιγμιότυπο `Console`. Αν οριστεί ως `true`, επιτρέπει τον χρωματισμό κατά την επιθεώρηση τιμών, ενώ αν οριστεί ως `'auto'` ορίσει την υποστήριξη χρώματος στην τιμή της ιδιότητας `isTTY` και η τιμή του επιστρέφεται από το `getColorDepth()` στην αντίστοιχη ροή του. **Προεπιλογή:** `'auto'`.

Δημιουργεί ένα νέο `Console` με μια ή δύο εγγράψιμες ροές. Το `stdout` είναι μια εγγράψιμη ροή που τυπώνει την έξοδο καταγραφής ή πληροφοριών. Το `stderr` χρησιμοποιείται για έξοδο προειδοποιήσεων ή σφαλμάτων. Αν δεν παρέχεται έξοδος `stderr`, τότε χρησιμοποιείται η έξοδος `stdout` και για το `stderr`.

```js
const output = fs.createWriteStream('./stdout.log');
const errorOutput = fs.createWriteStream('./stderr.log');
// απλός προσαρμοσμένος καταγραφέας
const logger = new Console({ stdout: output, stderr: errorOutput });
// χρήση σαν κονσόλα
const count = 5;
logger.log('count: %d', count);
// στο stdout.log: count 5
```

Το καθολικό `console` είναι ένα ειδικό στιγμιότυπο `Console` του οποίου η έξοδος γίνεται στο [`process.stdout`][] και στο [`process.stderr`][]. Είναι ισοδύναμο της παρακάτω κλήσης:

```js
new Console({ stdout: process.stdout, stderr: process.stderr });
```

### console.assert(value[, ...message])

<!-- YAML
added: v0.1.101
changes:

  - version: v10.0.0
    pr-url: https://github.com/nodejs/node/pull/17706
    description: The implementation is now spec compliant and does not throw
                 anymore.
-->

* `value` {any} Οποιαδήποτε τιμή που είναι πιθανώς αληθής.
* `...message` {any} Όλες οι παράμετροι, εκτός από το `value`, χρησιμοποιούνται ως μήνυμα σφάλματος.

Ένας απλός έλεγχος ισχυρισμού για το αν το `value` είναι αληθές. Αν δεν είναι, καταγράφεται ως `Assertion failed`. Αν παρέχεται, το σφάλμα `message` μορφοποιείται με τη χρήση του [`util.format()`][] μεταφέροντας όλες τις παραμέτρους του μηνύματος. Η έξοδος χρησιμοποιείται ως το μήνυμα σφάλματος.

```js
console.assert(true, 'does nothing');
// OK
console.assert(false, 'Whoops %s work', 'didn\'t');
// Assertion failed: Whoops didn't work
```

Η κλήση του `console.assert()` με έναν λάθος ισχυρισμό, θα προκαλέσει την εκτύπωση του `message` στην κονσόλα, χωρίς να διακοπεί η εκτέλεση του κώδικα που ακολουθεί.

### console.clear()

<!-- YAML
added: v8.3.0
-->

Όταν το `stdout` είναι ένα TTY, η κλήση του `console.clear()` θα προσπαθήσει να εκκαθαρίσει ολόκληρο το TTY. Όταν το `stdout` δεν είναι ένα TTY, αυτή η μέθοδος δεν κάνει τίποτα.

Η ακριβής λειτουργία του `console.clear()` μπορεί να διαφοροποιείται μεταξύ λειτουργικών συστημάτων και τύπων κονσόλας. Για τις περισσότερες διανομές Linux, το `console.clear()` λειτουργεί παρόμοια με την κλήση της εντολής κελύφους `clear`. Στα Windows, το `console.clear()` θα εκκαθαρίσει μόνο την έξοδο της τρέχουσας προβολής τερματικού του Node.js.

### console.count([label='default'])

<!-- YAML
added: v8.3.0
-->

* `label` {string} Η ετικέτα προβολής για τον μετρητή. **Προεπιλογή:** `'default'`.

Διατηρεί έναν εσωτερικό μετρητή ειδικά για το `label` και τυπώνει στο `stdout` πόσες φορές κλήθηκε το `console.count()` με το συγκεκριμένο `label`.

<!-- eslint-skip -->

```js
> console.count()
default: 1
undefined
> console.count('default')
default: 2
undefined
> console.count('abc')
abc: 1
undefined
> console.count('xyz')
xyz: 1
undefined
> console.count('abc')
abc: 2
undefined
> console.count()
default: 3
undefined
>
```

### console.countReset([label='default'])

<!-- YAML
added: v8.3.0
-->

* `label` {string} Η ετικέτα προβολής για τον μετρητή. **Προεπιλογή:** `'default'`.

Μηδενίζει τον εσωτερικό μετρητή του `label`.

<!-- eslint-skip -->

```js
> console.count('abc');
abc: 1
undefined
> console.countReset('abc');
undefined
> console.count('abc');
abc: 1
undefined
>
```

### console.debug(data[, ...args])

<!-- YAML
added: v8.0.0
changes:

  - version: v9.3.0
    pr-url: https://github.com/nodejs/node/pull/17033
    description: "`console.debug` is now an alias for `console.log`."
-->

* `data` {any}
* `...args` {any}

Η συνάρτηση `console.debug()` είναι ένα ψευδώνυμο για το [`console.log()`][].

### console.dir(obj[, options])

<!-- YAML
added: v0.1.101
-->

* `obj` {any}
* `options` {Object} 
  * `showHidden` {boolean} Αν είναι `true`, τότε θα εμφανιστούν και οι ιδιότητες συμβόλων και μη-καταμέτρησης του αντικειμένου. **Προεπιλογή:** `false`.
  * `depth` {number} Ενημερώνει το [`util.inspect()`][] πόσες φορές να ανατρέξει κατά την μορφοποίηση του αντικειμένου. Αυτό είναι χρήσιμο για την επιθεώρηση μεγάλων και μπερδεμένων αντικειμένων. Για να γίνεται επ'αορίστου, χρησιμοποιήστε την τιμή `null`. **Προεπιλογή:** `2`.
  * `colors` {boolean} Αν είναι `true`, τότε η έξοδος θα χρησιμοποιεί κωδικούς χρωμάτων ANSI. Τα χρώματα είναι παραμετροποιήσιμα· δείτε την ενότητα [παραμετροποίηση χρωμάτων `util.inspect()`][]. **Προεπιλογή:** `false`.

Χρησιμοποιεί το [`util.inspect()`][] στο `obj` και τυπώνει το string του αποτελέσματος στο `stdout`. Αυτή η συνάρτηση αγνοεί οποιαδήποτε προσαρμοσμένη συνάρτηση `inspect()` έχει οριστεί στο `obj`.

### console.dirxml(...data)

<!-- YAML
added: v8.0.0
changes:

  - version: v9.3.0
    pr-url: https://github.com/nodejs/node/pull/17152
    description: "`console.dirxml` now calls `console.log` for its arguments."
-->

* `...data` {any}

Αυτή η μέθοδος καλεί το `console.log()` και μεταδίδει τις παραμέτρους που έλαβε. Παρακαλώ σημειώστε ότι αυτή η μέθοδος δεν παράγει κάποια μορφοποίηση XML.

### console.error(\[data\]\[, ...args\])

<!-- YAML
added: v0.1.100
-->

* `data` {any}
* `...args` {any}

Τυπώνει στο `stderr` με χαρακτήρα αλλαγής γραμμής. Μπορούν να μεταδοθούν πολλαπλές παράμετροι, με την πρώτη να χρησιμοποιείται ως το κυρίως μήνυμα, και οι υπόλοιπες ως αντικαταστάτες, όπως στο printf(3) (όλες οι παράμετροι μεταφέρονται στο [`util.format()`][]).

```js
const code = 5;
console.error('error #%d', code);
// Τυπώνει: error #5, to stderr
console.error('error', code);
// Τυπώνει: error 5, to stderr
```

Αν δε βρεθούν στοιχεία μορφοποίησης (π.χ. `%d`) στο πρώτο string, τότε καλείται το [`util.inspect()`][] σε κάθε παράμετρο, και οι τιμές των αποτελεσμάτων συνενώνονται. Δείτε το [`util.format()`][] για περισσότερες πληροφορίες.

### console.group([...label])

<!-- YAML
added: v8.5.0
-->

* `...label` {any}

Αυξάνει την εσοχή των γραμμών που ακολουθούν, κατά δύο διαστήματα.

Αν παρέχονται ένα ή περισσότερα `label`, τότε αυτά τυπώνονται πρώτα χωρίς κάποια εσοχή.

### console.groupCollapsed()

<!-- YAML
  added: v8.5.0
-->

Ψευδώνυμο του [`console.group()`][].

### console.groupEnd()

<!-- YAML
added: v8.5.0
-->

Μειώνει την εσοχή των γραμμών που ακολουθούν, κατά δύο διαστήματα.

### console.info(\[data\]\[, ...args\])

<!-- YAML
added: v0.1.100
-->

* `data` {any}
* `...args` {any}

Η συνάρτηση `console.info()` είναι ένα ψευδώνυμο για την συνάρτηση [`console.log()`][].

### console.log(\[data\]\[, ...args\])

<!-- YAML
added: v0.1.100
-->

* `data` {any}
* `...args` {any}

Τυπώνει στο `stdout` με χαρακτήρα αλλαγής γραμμής. Μπορούν να μεταδοθούν πολλαπλές παράμετροι, με την πρώτη να χρησιμοποιείται ως το κυρίως μήνυμα, και οι υπόλοιπες ως αντικαταστάτες, όπως στο printf(3) (όλες οι παράμετροι μεταφέρονται στο [`util.format()`][]).

```js
const count = 5;
console.log('count: %d', count);
// Τυπώνει: count: 5, στο stdout
console.log('count:', count);
// Τυπώνει: count: 5, στο stdout
```

Δείτε το [`util.format()`][] για περισσότερες πληροφορίες.

### console.table(tabularData[, properties])

<!-- YAML
added: v10.0.0
-->

* `tabularData` {any}
* `properties` {string[]} Alternate properties for constructing the table.

Try to construct a table with the columns of the properties of `tabularData` (or use `properties`) and rows of `tabularData` and log it. Falls back to just logging the argument if it can’t be parsed as tabular.

```js
// These can't be parsed as tabular data
console.table(Symbol());
// Symbol()

console.table(undefined);
// undefined

console.table([{ a: 1, b: 'Y' }, { a: 'Z', b: 2 }]);
// ┌─────────┬─────┬─────┐
// │ (index) │  a  │  b  │
// ├─────────┼─────┼─────┤
// │    0    │  1  │ 'Y' │
// │    1    │ 'Z' │  2  │
// └─────────┴─────┴─────┘

console.table([{ a: 1, b: 'Y' }, { a: 'Z', b: 2 }], ['a']);
// ┌─────────┬─────┐
// │ (index) │  a  │
// ├─────────┼─────┤
// │    0    │  1  │
// │    1    │ 'Z' │
// └─────────┴─────┘
```

### console.time(label)

<!-- YAML
added: v0.1.104
-->

* `label` {string} **Default:** `'default'`

Starts a timer that can be used to compute the duration of an operation. Timers are identified by a unique `label`. Use the same `label` when calling [`console.timeEnd()`][] to stop the timer and output the elapsed time in milliseconds to `stdout`. Timer durations are accurate to the sub-millisecond.

### console.timeEnd(label)

<!-- YAML
added: v0.1.104
changes:

  - version: v6.0.0
    pr-url: https://github.com/nodejs/node/pull/5901
    description: This method no longer supports multiple calls that don’t map
                 to individual `console.time()` calls; see below for details.
-->

* `label` {string} **Default:** `'default'`

Stops a timer that was previously started by calling [`console.time()`][] and prints the result to `stdout`:

```js
console.time('100-elements');
for (let i = 0; i < 100; i++) {}
console.timeEnd('100-elements');
// prints 100-elements: 225.438ms
```

### console.trace(\[message\]\[, ...args\])

<!-- YAML
added: v0.1.104
-->

* `message` {any}
* `...args` {any}

Prints to `stderr` the string `'Trace: '`, followed by the [`util.format()`][] formatted message and stack trace to the current position in the code.

```js
console.trace('Show me');
// Prints: (stack trace will vary based on where trace is called)
//  Trace: Show me
//    at repl:2:9
//    at REPLServer.defaultEval (repl.js:248:27)
//    at bound (domain.js:287:14)
//    at REPLServer.runBound [as eval] (domain.js:300:12)
//    at REPLServer.<anonymous> (repl.js:412:12)
//    at emitOne (events.js:82:20)
//    at REPLServer.emit (events.js:169:7)
//    at REPLServer.Interface._onLine (readline.js:210:10)
//    at REPLServer.Interface._line (readline.js:549:8)
//    at REPLServer.Interface._ttyWrite (readline.js:826:14)
```

### console.warn(\[data\]\[, ...args\])

<!-- YAML
added: v0.1.100
-->

* `data` {any}
* `...args` {any}

The `console.warn()` function is an alias for [`console.error()`][].

## Inspector only methods

The following methods are exposed by the V8 engine in the general API but do not display anything unless used in conjunction with the [inspector](debugger.html) (`--inspect` flag).

### console.markTimeline(label)

<!-- YAML
added: v8.0.0
-->

* `label` {string} **Default:** `'default'`

This method does not display anything unless used in the inspector. The `console.markTimeline()` method is the deprecated form of [`console.timeStamp()`][].

### console.profile([label])

<!-- YAML
added: v8.0.0
-->

* `label` {string}

This method does not display anything unless used in the inspector. The `console.profile()` method starts a JavaScript CPU profile with an optional label until [`console.profileEnd()`][] is called. The profile is then added to the **Profile** panel of the inspector.

```js
console.profile('MyLabel');
// Some code
console.profileEnd();
// Adds the profile 'MyLabel' to the Profiles panel of the inspector.
```

### console.profileEnd()

<!-- YAML
added: v8.0.0
-->

This method does not display anything unless used in the inspector. Stops the current JavaScript CPU profiling session if one has been started and prints the report to the **Profiles** panel of the inspector. See [`console.profile()`][] for an example.

### console.timeStamp([label])

<!-- YAML
added: v8.0.0
-->

* `label` {string}

This method does not display anything unless used in the inspector. The `console.timeStamp()` method adds an event with the label `'label'` to the **Timeline** panel of the inspector.

### console.timeline([label])

<!-- YAML
added: v8.0.0
-->

* `label` {string} **Default:** `'default'`

This method does not display anything unless used in the inspector. The `console.timeline()` method is the deprecated form of [`console.time()`][].

### console.timelineEnd([label])

<!-- YAML
added: v8.0.0
-->

* `label` {string} **Default:** `'default'`

This method does not display anything unless used in the inspector. The `console.timelineEnd()` method is the deprecated form of [`console.timeEnd()`][].
# Functions

अक्सर हमें स्क्रिप्ट के कई स्थानों पर इसी तरह की कार्रवाई करने की आवश्यकता होती है।

उदाहरण के लिए, जब कोई आगंतुक लॉग इन करता है, लॉग आउट करता है और शायद कहीं और एक अच्छा दिखने वाला संदेश दिखाना चाहता है।

कार्य कार्यक्रम के मुख्य "बिल्डिंग ब्लॉक्स" हैं। वे बिना दोहराव के कोड को कई बार कॉल करने की अनुमति देते हैं।

हम पहले से ही अंतर्निहित कार्यों के उदाहरण देख चुके हैं, जैसे `alert(message)`, `prompt(message, default)` तथा `confirm(question)`. लेकिन हम अपने स्वयं के कार्य भी बना सकते हैं।

## Function Declaration

एक फ़ंक्शन बनाने के लिए हम  *function declaration*. का उपयोग कर सकते हैं

यह इस तरह दिखेगा:

```js
function showMessage() {
  alert( 'Hello everyone!' );
}
```

`function` कीवर्ड पहले जाता है, फिर *फ़ंक्शन का नाम* जाता है, फिर कोष्ठकों के बीच *पैरामीटर* की एक सूची (उपरोक्त उदाहरण में अल्पविराम से अलग, खाली) और अंत में फ़ंक्शन का कोड, जिसका नाम " फंक्शन बॉडी", घुंघराले ब्रेसिज़ के बीच।

```js
function name(parameters) {
  ...body...
}
```
हमारे नए फ़ंक्शन को इसके नाम से बुलाया जा सकता है: `showMessage()`.

उदाहरण के लिए:

```js run
function showMessage() {
  alert( 'Hello everyone!' );
}

*!*
showMessage();
showMessage();
*/!*
```

कॉल `showMessage()` फ़ंक्शन के कोड को निष्पादित करता है। यहां हम संदेश को दो बार देखेंगे।

यह उदाहरण स्पष्ट रूप से कार्यों के मुख्य उद्देश्यों में से एक को प्रदर्शित करता है: कोड दोहराव से बचने के लिए।

यदि हमें कभी भी संदेश या उसके दिखाए जाने के तरीके को बदलने की आवश्यकता होती है, तो यह कोड को एक ही स्थान पर संशोधित करने के लिए पर्याप्त है: वह फ़ंक्शन जो इसे आउटपुट करता है।
## Local variables

किसी फ़ंक्शन के अंदर घोषित एक चर केवल उस फ़ंक्शन के अंदर ही दिखाई देता है।

उदाहरण के लिए:

```js run
function showMessage() {
*!*
  let message = "Hello, I'm JavaScript!"; // local variable
*/!*

  alert( message );
}

showMessage(); // Hello, I'm JavaScript!

alert( message ); // <-- Error! The variable is local to the function
```

## Outer variables

एक फ़ंक्शन बाहरी चर को भी एक्सेस कर सकता है, उदाहरण के लिए:

```js run no-beautify
let *!*userName*/!* = 'John';

function showMessage() {
  let message = 'Hello, ' + *!*userName*/!*;
  alert(message);
}

showMessage(); // Hello, John
```

फ़ंक्शन के पास बाहरी चर तक पूर्ण पहुंच है। यह इसे संशोधित भी कर सकता है।

उदाहरण के लिए:

```js run
let *!*userName*/!* = 'John';

function showMessage() {
  *!*userName*/!* = "Bob"; // (1) changed the outer variable

  let message = 'Hello, ' + *!*userName*/!*;
  alert(message);
}

alert( userName ); // *!*John*/!* before the function call

showMessage();

alert( userName ); // *!*Bob*/!*, the value was modified by the function
```

बाहरी चर का उपयोग केवल तभी किया जाता है जब कोई स्थानीय न हो।

यदि फ़ंक्शन के अंदर एक ही नामित चर घोषित किया जाता है तो यह बाहरी को *छाया* देता है। उदाहरण के लिए, नीचे दिए गए कोड में फ़ंक्शन स्थानीय `उपयोगकर्ता नाम` का उपयोग करता है। बाहरी को अनदेखा किया जाता है:

```js run
let userName = 'John';

function showMessage() {
*!*
  let userName = "Bob"; // declare a local variable
*/!*

  let message = 'Hello, ' + userName; // *!*Bob*/!*
  alert(message);
}

// the function will create and use its own userName
showMessage();

alert( userName ); // *!*John*/!*, unchanged, the function did not access the outer variable
```

```smart header="Global variables"
Variables declared outside of any function, such as the outer `userName` in the code above, are called *global*.

Global variables are visible from any function (unless shadowed by locals).

It's a good practice to minimize the use of global variables. Modern code has few or no globals. Most variables reside in their functions. Sometimes though, they can be useful to store project-level data.
```

## Parameters

हम पैरामीटर (जिसे *फ़ंक्शन तर्क* भी कहा जाता है) का उपयोग करके फ़ंक्शन के लिए मनमाना डेटा पास कर सकते हैं।

नीचे दिए गए उदाहरण में, फ़ंक्शन के दो पैरामीटर हैं: `from` तथा `text`.

```js run
function showMessage(*!*from, text*/!*) { // arguments: from, text
  alert(from + ': ' + text);
}

*!*
showMessage('Ann', 'Hello!'); // Ann: Hello! (*)
showMessage('Ann', "What's up?"); // Ann: What's up? (**)
*/!*
```

जब फ़ंक्शन को लाइनों में कहा जाता है `(*)` तथ `(**)`, दिए गए मान स्थानीय चर में कॉपी किए जाते हैं `from` तथा `text`. फिर फ़ंक्शन उनका उपयोग करता है।

यहाँ एक और उदाहरण है: हमारे पास एक चर है `from` और इसे फ़ंक्शन में पास करें। कृपया ध्यान दें: फ़ंक्शन बदलता है `from`, लेकिन परिवर्तन बाहर नहीं देखा जाता है, क्योंकि एक फ़ंक्शन को हमेशा मूल्य की एक प्रति मिलती है:


```js run
function showMessage(from, text) {

*!*
  from = '*' + from + '*'; // make "from" look nicer
*/!*

  alert( from + ': ' + text );
}

let from = "Ann";

showMessage(from, "Hello"); // *Ann*: Hello

// the value of "from" is the same, the function modified a local copy
alert( from ); // Ann
```

## Default values

If a parameter is not provided, then its value becomes `undefined`.

For instance, the aforementioned function `showMessage(from, text)` can be called with a single argument:

```js
showMessage("Ann");
```

That's not an error. Such a call would output `"*Ann*: undefined"`. There's no `text`, so it's assumed that `text === undefined`.

If we want to use a "default" `text` in this case, then we can specify it after `=`:

```js run
function showMessage(from, *!*text = "no text given"*/!*) {
  alert( from + ": " + text );
}

showMessage("Ann"); // Ann: no text given
```

Now if the `text` parameter is not passed, it will get the value `"no text given"`

Here `"no text given"` is a string, but it can be a more complex expression, which is only evaluated and assigned if the parameter is missing. So, this is also possible:

```js run
function showMessage(from, text = anotherFunction()) {
  // anotherFunction() only executed if no text given
  // its result becomes the value of text
}
```

```smart header="Evaluation of default parameters"
In JavaScript, a default parameter is evaluated every time the function is called without the respective parameter.

In the example above, `anotherFunction()` is called every time `showMessage()` is called without the `text` parameter.
```

### Alternative default parameters

Sometimes it makes sense to set default values for parameters not in the function declaration, but at a later stage, during its execution.

To check for an omitted parameter, we can compare it with `undefined`:

```js run
function showMessage(text) {
*!*
  if (text === undefined) {
    text = 'empty message';
  }
*/!*

  alert(text);
}

showMessage(); // empty message
```

...Or we could use the `||` operator:

```js
// if text parameter is omitted or "" is passed, set it to 'empty'
function showMessage(text) {
  text = text || 'empty';
  ...
}
```

Modern JavaScript engines support the [nullish coalescing operator](info:nullish-coalescing-operator) `??`, it's better when falsy values, such as `0`, are considered regular:

```js run
// if there's no "count" parameter, show "unknown"
function showCount(count) {
  alert(count ?? "unknown");
}

showCount(0); // 0
showCount(null); // unknown
showCount(); // unknown
```

## Returning a value

A function can return a value back into the calling code as the result.

The simplest example would be a function that sums two values:

```js run no-beautify
function sum(a, b) {
  *!*return*/!* a + b;
}

let result = sum(1, 2);
alert( result ); // 3
```

The directive `return` can be in any place of the function. When the execution reaches it, the function stops, and the value is returned to the calling code (assigned to `result` above).

There may be many occurrences of `return` in a single function. For instance:

```js run
function checkAge(age) {
  if (age >= 18) {
*!*
    return true;
*/!*
  } else {
*!*
    return confirm('Do you have permission from your parents?');
*/!*
  }
}

let age = prompt('How old are you?', 18);

if ( checkAge(age) ) {
  alert( 'Access granted' );
} else {
  alert( 'Access denied' );
}
```

It is possible to use `return` without a value. That causes the function to exit immediately.

For example:

```js
function showMovie(age) {
  if ( !checkAge(age) ) {
*!*
    return;
*/!*
  }

  alert( "Showing you the movie" ); // (*)
  // ...
}
```

In the code above, if `checkAge(age)` returns `false`, then `showMovie` won't proceed to the `alert`.

````smart header="A function with an empty `return` or without it returns `undefined`"
If a function does not return a value, it is the same as if it returns `undefined`:

```js run
function doNothing() { /* empty */ }

alert( doNothing() === undefined ); // true
```

An empty `return` is also the same as `return undefined`:

```js run
function doNothing() {
  return;
}

alert( doNothing() === undefined ); // true
```
````

````warn header="Never add a newline between `return` and the value"
For a long expression in `return`, it might be tempting to put it on a separate line, like this:

```js
return
 (some + long + expression + or + whatever * f(a) + f(b))
```
That doesn't work, because JavaScript assumes a semicolon after `return`. That'll work the same as:

```js
return*!*;*/!*
 (some + long + expression + or + whatever * f(a) + f(b))
```

So, it effectively becomes an empty return.

If we want the returned expression to wrap across multiple lines, we should start it at the same line as `return`. Or at least put the opening parentheses there as follows:

```js
return (
  some + long + expression
  + or +
  whatever * f(a) + f(b)
  )
```
And it will work just as we expect it to.
````

## Naming a function [#function-naming]

Functions are actions. So their name is usually a verb. It should be brief, as accurate as possible and describe what the function does, so that someone reading the code gets an indication of what the function does.

It is a widespread practice to start a function with a verbal prefix which vaguely describes the action. There must be an agreement within the team on the meaning of the prefixes.

For instance, functions that start with `"show"` usually show something.

Function की शुरुआत...

- `"get…"` -- return a value,
- `"calc…"` -- calculate something,
- `"create…"` -- create something,
- `"check…"` -- check something and return a boolean, etc.

ऐसे नामों के उदाहरण:

```js no-beautify
showMessage(..)     // shows a message
getAge(..)          // returns the age (gets it somehow)
calcSum(..)         // calculates a sum and returns the result
createForm(..)      // creates a form (and usually returns it)
checkPermission(..) // checks a permission, returns true/false
```
उपसर्गों के साथ, फ़ंक्शन नाम पर एक नज़र यह समझ देती है कि यह किस प्रकार का कार्य करता है और यह किस प्रकार का मूल्य देता है।

```smart header="One function -- one action"
A function should do exactly what is suggested by its name, no more.

Two independent actions usually deserve two functions, even if they are usually called together (in that case we can make a 3rd function that calls those two).

A few examples of breaking this rule:

- `getAge` -- would be bad if it shows an `alert` with the age (should only get).
- `createForm` -- would be bad if it modifies the document, adding a form to it (should only create it and return).
- `checkPermission` -- would be bad if it displays the `access granted/denied` message (should only perform the check and return the result).

These examples assume common meanings of prefixes. You and your team are free to agree on other meanings, but usually they're not much different. In any case, you should have a firm understanding of what a prefix means, what a prefixed function can and cannot do. All same-prefixed functions should obey the rules. And the team should share the knowledge.
```

```smart header="Ultrashort function names"
Functions that are used *very often* sometimes have ultrashort names.

For example, the [jQuery](http://jquery.com) framework defines a function with `$`. The [Lodash](http://lodash.com/) library has its core function named `_`.

These are exceptions. Generally functions names should be concise and descriptive.
```

## Functions == Comments

कार्य छोटे होने चाहिए और ठीक एक काम करना चाहिए। अगर वह चीज बड़ी है, तो शायद यह फ़ंक्शन को कुछ छोटे कार्यों में विभाजित करने के लायक है। कभी-कभी इस नियम का पालन करना इतना आसान नहीं हो सकता है, लेकिन यह निश्चित रूप से एक अच्छी बात है।

एक अलग फ़ंक्शन न केवल परीक्षण और डीबग करना आसान है - इसका अस्तित्व एक महान टिप्पणी है!

उदाहरण के लिए, दो कार्यों की तुलना करें `showPrimes(n)` नीचे। हर एक आउटपुट [प्राइम नंबर](https://hi.wikipedia.org/wiki/%E0%A4%85%E0%A4%AD%E0%A4%BE%E0%A4%9C%E0%A5%8D%E0%A4%AF_%E0%A4%B8%E0%A4%82%E0%A4%96%E0%A5%8D%E0%A4%AF%E0%A4%BE) तक `n`.

पहला संस्करण एक लेबल का उपयोग करता है:

```js
function showPrimes(n) {
  nextPrime: for (let i = 2; i < n; i++) {

    for (let j = 2; j < i; j++) {
      if (i % j == 0) continue nextPrime;
    }

    alert( i ); // a prime
  }
}
```

दूसरा संस्करण एक अतिरिक्त फ़ंक्शन का उपयोग करता है `isPrime(n)` प्राथमिकता के लिए परीक्षण करने के लिए:

```js
function showPrimes(n) {

  for (let i = 2; i < n; i++) {
    *!*if (!isPrime(i)) continue;*/!*

    alert(i);  // a prime
  }
}

function isPrime(n) {
  for (let i = 2; i < n; i++) {
    if ( n % i == 0) return false;
  }
  return true;
}
```
दूसरा संस्करण समझना आसान है, है ना? कोड पीस के बजाय हम क्रिया का एक नाम देखते हैं (`isPrime`).कभी-कभी लोग इस तरह के कोड का उल्लेख करते हैं: *स्वयं का वर्णन*.
इसलिए, फ़ंक्शन बनाए जा सकते हैं, भले ही हम उनका पुन: उपयोग करने का इरादा न रखते हों। वे कोड की संरचना करते हैं और इसे पठनीय बनाते हैं।

## Summary

एक समारोह घोषणा इस तरह दिखती है:

```js
function name(parameters, delimited, by, comma) {
  /* code */
}
```

- किसी फ़ंक्शन को दिए गए मान पैरामीटर के रूप में उसके स्थानीय चर में कॉपी किए जाते हैं।
- एक फ़ंक्शन बाहरी चरों तक पहुँच सकता है। लेकिन यह केवल अंदर से बाहर काम करता है। फ़ंक्शन के बाहर का कोड इसके स्थानीय चर नहीं देखता है।
- एक फ़ंक्शन एक मान वापस कर सकता है। यदि ऐसा नहीं होता है, तो इसका परिणाम है `undefined`.

कोड को साफ और समझने में आसान बनाने के लिए, फ़ंक्शन में मुख्य रूप से स्थानीय चर और पैरामीटर का उपयोग करने की अनुशंसा की जाती है, बाहरी चर नहीं।

एक फ़ंक्शन को समझना हमेशा आसान होता है जो पैरामीटर प्राप्त करता है, उनके साथ काम करता है और एक ऐसे फ़ंक्शन की तुलना में परिणाम देता है जिसमें कोई पैरामीटर नहीं होता है, लेकिन बाहरी चर को साइड-इफेक्ट के रूप में संशोधित करता है।
एक फ़ंक्शन को समझना हमेशा आसान होता है जो पैरामीटर प्राप्त करता है, उनके साथ काम करता है और एक ऐसे फ़ंक्शन की तुलना में परिणाम देता है जिसमें कोई पैरामीटर नहीं होता है, लेकिन बाहरी चर को साइड-इफेक्ट के रूप में संशोधित करता है।
Function naming:

- एक नाम स्पष्ट रूप से वर्णन करना चाहिए कि फ़ंक्शन क्या करता है। जब हम कोड में एक फ़ंक्शन कॉल देखते हैं, तो एक अच्छा नाम हमें तुरंत समझ देता है कि यह क्या करता है और वापस लौटता है।
- एक फ़ंक्शन एक क्रिया है, इसलिए फ़ंक्शन नाम आमतौर पर मौखिक होते हैं।
- कई प्रसिद्ध फ़ंक्शन उपसर्ग मौजूद हैं जैसे `create…`, `show…`, `get…`, `check…` और इसी तरह। फ़ंक्शन क्या करता है यह संकेत देने के लिए उनका उपयोग करें।

कार्य लिपियों के मुख्य निर्माण खंड हैं। अब हमने मूल बातें शामिल कर ली हैं, इसलिए हम वास्तव में उन्हें बनाना और उनका उपयोग करना शुरू कर सकते हैं। लेकिन यह सिर्फ रास्ते की शुरुआत है। हम कई बार उनके पास वापस जा रहे हैं, उनकी उन्नत सुविधाओं में और अधिक गहराई से जा रहे हैं।

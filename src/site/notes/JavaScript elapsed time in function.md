---
{"dg-publish":true,"permalink":"/java-script-elapsed-time-in-function/"}
---


[[🗺 Code\|🗺 Code]]

This allows us to log how long various steps of a Node or other JS function take:

At beginning:

```js
var start = process.hrtime();
var elapsed_time = function(note) {
  var precision = 3; // 3 decimal places
  var elapsed = process.hrtime(start)[1] / 1000000; // divide by a million to get nano to milli
  console.log(process.hrtime(start)[0] + ' s, ' + elapsed.toFixed(precision) + ' ms - ' + note); // print message + time
   start = process.hrtime();  reset the timer
};
```

Now, anywhere you want to get a time:

```js
elapsed_time('this is a note');
```
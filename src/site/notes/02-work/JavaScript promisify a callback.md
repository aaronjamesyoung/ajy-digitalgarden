---
{"dg-publish":true,"grade":2,"permalink":"/02-work/java-script-promisify-a-callback/","dgPassFrontmatter":true}
---


[[📘 Code and Work\|📘 Code and Work]]

If we have a callback-style function, we can wrap it like so, so we get a promise instead.

I did this in Brians, pricing.js

https://nodejs.org/api/util.html#util_util_promisify_original

```js
async function wrapperFunc() {
  const { promisify } = require('util');
  const promisifiedFunction = promisify(callbackFunction);
  const ret = await promisifiedFunction() // Get the return value from the promisified function
  return ret;
}
```
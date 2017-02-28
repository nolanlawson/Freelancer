![Moggy](media/logo.png)

> An implementation of on-the-fly defined WebWorkers that are created inline using data URIs, rather than separate physical files &mdash; for the benefit of all humanity.

[![forthebadge](http://forthebadge.com/images/badges/compatibility-betamax.svg)](http://forthebadge.com)
[![forthebadge](http://forthebadge.com/images/badges/built-with-love.svg)](http://forthebadge.com)

## Getting Started

`Freelancer` uses the **same** interface as `Worker` except the passed parameters upon instantiation are *slightly* different. Normally when invoking `new Worker` you pass the location of the file, whereas with `new Freelancer` you pass a function that contains the body of the worker &ndash; `Freelancer` also allows an *optional* second parameter to be passed that allows you to send additional parameters to the worker.

```javascript
import { Freelancer } from 'freelancer';

const worker = new Freelancer(() => {
   
    self.addEventListener('message', () => {
        self.postMessage('Pong!');
    });
    
});

worker.postMessage('Ping?');
```

It's worth bearing in mind that the worker is still a separate thread and thus the typical [rules of closures](https://developer.mozilla.org/en/docs/Web/JavaScript/Closures) no longer apply &ndash; any parameters you would like to `postMessage` or [pass parameters](#passing-parameters) upon instantiation.

## Passing Parameters

Upon instantiation of `Freelancer` using the second parameter you can pass an object of options that will be pushed into the worker.

```javascript
import { Freelancer } from 'freelancer';

const options = { send: 'Ping?', respond: 'Pong!' };

const worker = new Freelancer(options => {
   
    self.addEventListener('message', () => {
        self.postMessage(options.respond);
    });
    
}, options);

worker.postMessage(options.send);
```

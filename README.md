![Freelancer](media/logo.png)

> `npm i freelancer --save`<br /><br />
> An implementation of on-the-fly defined WebWorkers that are created inline using data URIs, rather than separate physical files &mdash; for the benefit of all humanity.<br /><br />
> **example:** [esnextb.in](https://esnextb.in/?gist=26cda2d5ce0e508d367744b936200a58) &nbsp;&nbsp;&bull;&nbsp;&nbsp; ~500 bytes gzipped.

[![forthebadge](http://forthebadge.com/images/badges/compatibility-betamax.svg)](http://forthebadge.com)
[![forthebadge](http://forthebadge.com/images/badges/built-with-love.svg)](http://forthebadge.com)

## Getting Started

`Freelancer` uses the **same** interface as `Worker` except the passed parameters upon instantiation are *slightly* different.

Normally when invoking `new Worker` you pass the location of the file, whereas with `new Freelancer` you pass a function that contains the body of the worker.

`Freelancer` also allows an *optional* [second parameter](#passing-parameters) to be passed that allows you to send additional options to the worker.

```javascript
import { Freelancer } from 'freelancer';

const worker = new Freelancer(() => {
   
    self.addEventListener('message', event => {
        console.log(event.data);
        self.postMessage('Pong!');
    });
    
});

worker.addEventListener('message', event => console.log(event.data));
worker.postMessage('Ping?');
```

It's worth bearing in mind that the worker is still a separate thread and thus the typical [rules of closures](https://developer.mozilla.org/en/docs/Web/JavaScript/Closures) no longer apply &ndash; any parameters you would like to be received by the worker would need to be sent using `postMessage` or by [passing parameters](#passing-parameters) upon instantiation.

## Passing Parameters

Upon instantiation of `Freelancer` you can use the second parameter to pass options that will be pushed to the worker &ndash; passed options will be serialized using `JSON.stringify` and thus any data sent needs to be serializable &ndash; which essentially means you're unable to pass by reference, and circular references will cause issues.

```javascript
import { SharedFreelancer } from 'freelancer';

const options = { send: 'Ping?', respond: 'Pong!' };

const worker = new SharedFreelancer(options => {
   
    self.addEventListener('message', event => {
        console.log(event.data);
        self.postMessage(options.respond);
    });
    
}, options);

worker.addEventListener('message', event => console.log(event.data));
worker.postMessage(options.send);
```

Although we refer to it as the *second parameter* you are in fact able to pass an infinite amount of parameters to the worker &ndash; the only requirement is that the first parameter is the worker's function.
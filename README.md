# zipkin-instrumentation-koa
a zipkin middleware for koa2

## version

```
koa: 2.7.0
zipkin-context-cls: 0.6.1
zipkin-transport-http: 0.7.3
zipkin: 0.16.2
```

## How to use

```js
const Koa = require('koa');
const CLSContext = require('zipkin-context-cls');
const {HttpLogger} = require('zipkin-transport-http');
const {Tracer, ExplicitContext, ConsoleRecorder, BatchRecorder, jsonEncoder: {JSON_V2}} = require('zipkin');
const zipkinMiddleware = require('zipkin-instrumentation-koa').koaMiddleware

const ctxImpl = new CLSContext();
const localServiceName = 'service-a';
const recorder = new BatchRecorder({
  logger: new HttpLogger({
    endpoint: 'http://host:9411/api/v2/spans',
    jsonEncoder: JSON_V2
  })
})
const tracer = new Tracer({ctxImpl, recorder, localServiceName});
const app = new Koa();

app.use(zipkinMiddleware({tracer}));
app.use(async ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);
```
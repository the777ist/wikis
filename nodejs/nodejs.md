# NODEJS

Start nodejs-express server:
```js
const express = require('express');
const app = express();

app.get('/', function(req, res) {
    res.send('<h1>Hello World</h1>');
});

app.listen(3000);
```

Use middleware:
```js
function loggingMiddleware(req, res, next) {
    // something
    next();
}

app.use(loggingMiddleware);
```

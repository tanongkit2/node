node
====
step
#install nodejs
#create app.js
<code>
<pre>var express = require('express');var app = express();
app.get('/', function (req, res) {
res.send('Hello World')
})
app.listen(3000)
module.exports = app;</pre>
</code>
node

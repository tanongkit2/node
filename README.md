node
====
step 1 <br />
install nodejs
<pre>
npm install nodejs
</pre>
create folder project <br />
create file app.js
<pre>var express = require('express');var app = express();
app.get('/', function (req, res) {
res.send('Hello World')
})
app.listen(3000)
module.exports = app;</pre>

node

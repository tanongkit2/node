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
step 2 <br />create views folder
change render view from jade to html
create file index.html > view folder
app.js
<pre>var express = require('express');
var app = express();
app.engine('.html', require('ejs').__express);
app.set('views', __dirname + '/views');
app.set('view engine', 'html');
app.get('/', function(req, res){
res.render('index', {
title: "EJS example",
header: "Some users"
  });
});
app.listen(3000)
module.exports = app;
</pre>
index.html
<pre>
DOCTYPE html
body
I love you index html
/body
/html
</pre>
step 3 <br />
create routes folder</br>point to routing
<pre>var express = require('express');
var path = require('path');
var routes = require('./routes/index');
var app = express();
app.engine('.html', require('ejs').__express);
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'html');
app.use(express.static(path.join(__dirname, 'public')));
app.use('/', routes);
app.listen(3000)
module.exports = app;<pre>

node

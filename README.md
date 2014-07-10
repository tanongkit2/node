node
====
step 1 <br />
install nodejs
<pre>
npm install nodejs
</pre>
create folder project <br />
create file app.js <br />
app.js
<pre>
var express = require('express');
var app = express();
app.get('/', function (req, res) {
  res.send('Hello World')
})
app.listen(3000)
module.exports = app;
</pre>

step 2 <br />
create views folder <br />
change render view from jade to html <br />
create file index.html > view folder <br />
app.js
<pre>
var express = require('express');
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
create routes folder</br>
create public folder</br>
point to routing </br>
<pre>
npm install path
</pre>
app.js
<pre>
var express = require('express');
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

step 4 <br />
install mongoDB
npm install mongoose
connect mongooose
<pre>
npm install mongooose
</pre>




node

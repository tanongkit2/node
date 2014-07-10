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
<pre>
npm install ejs
</pre>
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
create index.js > routes folder</br>
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
module.exports = app;</pre>
index.js
<pre>
var express = require('express');
var router = express.Router();
router.get('/', function(req, res) {
  res.render('index', { title: 'index'});
});
module.exports = router;
<pre>

step 4 <br />
install mongoDB
npm install mongoose
connect mongooose
<pre>
npm install mongooose
</pre>
routes > index.js
<pre>
var express = require('express');
var router = express.Router();
var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/test' , function(err){});
</pre>

step 5 <br />
mongoose Schema <br />
mongoose model <br />
routes > index.js
<pre>
var express = require('express');
var router = express.Router();
var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/test' , function(err){});
var UserSchema = new mongoose.Schema({
  user : {
    type : String
  },  
  pwd : {
    type : String
  }
});
var User = mongoose.model('User', UserSchema);
router.get('/', function(req, res) {
  res.render('index', { title: 'index'});
});
module.exports = router;
</pre>

step 6 <br />
include body-parser <br />
create routing service mongodb <br />
app.js
<pre>
var express = require('express');
var path = require('path');
var bodyParser = require('body-parser');
app.use(express.bodyParser());
var routes = require('./routes/index');
var app = express();
app.engine('.html', require('ejs').__express);
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'html');
app.use(express.static(path.join(__dirname, 'public')));
app.use('/', routes);
app.listen(3000)
module.exports = app;
</pre>
routes > index.js
<pre>
var express = require('express');
var router = express.Router();
var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/test' , function(err){});
var UserSchema = new mongoose.Schema({
  user : {
    type : String
  },  
  pwd : {
    type : String
  }
});
var User = mongoose.model('User', UserSchema);
router.get('/', function(req, res) {
  res.render('index', { title: 'index'});
});
router.post('/save', function(req, res) {
  var user = new User({
        user : req.body.user ,
        pwd : req.body.pwd
      });
  user.save(function(err , user){
      res.jsonp(user);
  });
});
module.exports = router;
</pre>

node

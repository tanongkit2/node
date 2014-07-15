
node
====
### step 1 install node.js
install nodejs
<pre> npm install nodejs </pre>
create folder project <br />
create file app.js <br />
####app.js
<pre>
var express = require('express');
var app = express();
app.get('/', function (req, res) {
  res.send('Hello World')
})
app.listen(3000)
module.exports = app;
</pre>

### step 2 change render .jade to .html file
create views folder <br />
change render view from jade to html <br />
create file index.html > view folder <br />
<pre> npm install ejs </pre>
####app.js
<pre>
app.engine('.html', require('ejs').__express);
app.set('views', __dirname + '/views');
app.set('view engine', 'html');

app.get('/', function(req, res){
    res.render('index', {
      title: "EJS example",
      header: "Some users"
    });
});
</pre>
####index.html
```
<html>
<DOCTYPE html>
<body>
I love you index html
</body>
</html>
```
### step 3 init routes
create routes folder</br>
create public folder</br>
create index.js > routes folder</br>
<pre> npm install path </pre>
####app.js
<pre>
var path = require('path');
var routes = require('./routes/index');

app.engine('.html', require('ejs').__express);
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'html');
app.use(express.static(path.join(__dirname, 'public')));
app.use('/', routes);
</pre>
####index.js
<pre>
var express = require('express');
var router = express.Router();
router.get('/', function(req, res) {
  res.render('index', { title: 'index'});
});
module.exports = router;
</pre>

### step 4 install mongodb & npm mongoose
install mongoDB <br />
npm install mongoose <br />
connect mongoose <br />
<pre> npm install mongoose </pre>
####app.js
<pre>
var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/test' , function(err){});
</pre>

### step 5 init mongoose Schema Model
mongoose Schema <br />
mongoose model <br />

### model > user.js
<pre>
var mongoose = require('mongoose');

var UserSchema = new mongoose.Schema({
	user : {
		type : String
	},
	pwd : {
		type : String
	}
});
mongoose.model('User' , UserSchema);
</pre>

### step 5.1 ssh shell mongo & mongod
<pre>
terminal1 > mongod
</pre>

terminal2 > mongo localhost 
create collection
<pre>
db.createCollection("people")
db.getCollectionNames()
</pre>

insert data to collection
```
var topicChoices = ['male', 'female','mongodb','query'];
var commentChoices = ['black', 'brown', 'blond', 'red', 'auburn',
'chestnut', 'white'];
var userChoices = ['golf', 'jone', 'ken', 'red', 'odete',
'chestnut', 'white', 'gety'];

var createDate = new Date();
createDate.setYear(1970);

for (var i = 0; i < 1000000; ++i) {
  createDate.setHours(createDate.getHours() + 1);
  db.people.insert({
    topicID : i,
    topic : topicChoices [Math.floor(Math.random() *topicChoices.length)]+i,
    comments : [ ],
    user: userChoices [Math.floor(Math.random() *userChoices.length)],
    date : createDate  
  })
}
```

insert data to sub collection
```
var commentChoices = ['black', 'brown', 'blond', 'red', 'auburn',
'chestnut', 'white'];
var userChoices = ['golf', 'jone', 'ken', 'red', 'odete',
'chestnut', 'white', 'gety'];
var createDate = new Date();
createDate.setYear(1970);

for (var i = 0; i < 100000; ++i) {
  var rantopicID = Math.floor(Math.random() * 1000);
  createDate.setHours(createDate.getHours() + 1);
  db.people.update({
    "topicID" : rantopicID
  },{
     $push: {
       'comments' : {
         commentID: i,
         text : commentChoices [Math.floor(Math.random() *commentChoices.length)],
         user : userChoices [Math.floor(Math.random() *userChoices.length)],
         date : createDate  
       }
     }
   })
} 
```

use explain()
<pre>
db.people.find().explain()
</pre>
create index
<pre>
db.people.ensureIndex({date : 1})
db.people.ensureIndex({date : -1})
db.people.find().sort({date:-1}).limit(1).explain();
db.people.getIndexes()
db.people.dropIndex({topicID:-1})
</pre>
### step 6 include body-parser with init parameter client to server
include body-parser <br />
create routing service mongodb <br />
####app.js
<pre>
var bodyParser = require('body-parser');
app.use(express.bodyParser());
</pre>
####routes > index.js
<pre>
var mongoose = require('mongoose');
require('../model/user');
var User = mongoose.model('User');

router.post('/save', function(req, res) {
  var user = new User({
        user : req.body.user ,
        pwd : req.body.pwd
      });
  user.save(function(err , user){
      res.jsonp(user);
  });
});
</pre>

###step 7 post data with jquery
send data client to server save do mongodb <br />
callback jsonp to client <br />

####routes > index.js
<pre>
router.get('/test', function(req, res) {
  User.find().exec(function(err , users){
    res.render('test', { title: 'test' , users : users});
  });
});
</pre>

####views > test.html
```
<DOCTYPE html>
<body>
test html
<input type="text" id="user">
<input type="button" value="save" id="s">
</body>
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
<script>
$(document).ready(function(){
  $("‪#‎s‬").click(function(){
    $.post("/save", {
      user: $('‪#‎user‬').val(),
      pwd:"Duckburg"
    },function(data,status){
      $('body').append('id : ' +data._id);
      $('body').append('user : ' +data.user);
      $('body').append('pass : ' +data.pwd);
      $('body').append('-----------');
    });
  });
});
</script>
```
###step 8 include angular js in client
include angularjs <br>
####routes > index.js
<pre>
router.get('/angular', function(req, res) {
  res.render('angular', { title: 'angular' });
});
router.get('/users', function(req, res) {
  User.find().exec(function(err , users){
  res.jsonp(users);
});
</pre>

####views > angular.html
```
<!DOCTYPE html>
<html ng-app>
<script src="http://ajax.googleapis.com/ajax/libs/angularjs/1.0.8/angular.min.js"></script>
<script src="http://ajax.googleapis.com/ajax/libs/angularjs/1.0.8/angular-resource.min.js"></script>
<body ng-controller='angularCtrl' data-ng-init="find()">
<angular html>
<ul>
<li ng-repeat="contact in contacts"> {{ contact }} </li>
</ul>
<ul>
<li data-ng-repeat="user in users" class="list-group-item">
{{user._id}} + {{user.user}} + {{user.pwd}}
</li>
</ul>
</body>
<script>
function angularCtrl($scope , $http) {
  $scope.find = function() {
    $http.get('/users' ,{}).success(function(response) {
      $scope.users = response;
    }).error(function(response) {
      $scope.error = response.message;
    });
  };
  $scope.contacts = ["hi@email.com", "hello@email.com"];
}
</script>
</html>
```
###step 9 include passport
npm install req passport & session & cookie
#### app.js
<pre>
var passport = require('passport');
var LocalStrategy = require('passport-local').Strategy;
var flash = require('connect-flash');
var methodOverride = require('method-override');
var cookieParser = require('cookie-parser');
var session = require('express-session');
var logger = require('morgan');

app.use(cookieParser());
app.use(methodOverride());
app.use(session({ secret: 'keyboard cat' }));
app.use(flash());
app.use(passport.initialize());
app.use(passport.session());

require('./controller/passport')();

var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/test' , function(err){
	if(err) console.log(err);
});
</pre>

### controller > passport.js
<pre>
var passport = require('passport'),
LocalStrategy = require('passport-local').Strategy;
var mongoose = require('mongoose');
require('../model/user');
var User = mongoose.model('User');

module.exports = function() {

	passport.serializeUser(function(user, done) {
		done(null, user.id);
	});

	// Deserialize sessions
	passport.deserializeUser(function(id, done) {
		User.findOne({
			_id: id
		},function(err, user) {
			done(err, user);
		});
	});

	passport.use(new LocalStrategy({
			usernameField: 'user',
			passwordField: 'pwd'
		},function(username, password, done) {
			console.log("LocalStrategy working...");
			User.findOne({
				user: username
			}, function(err, user) {
				if (err) {
				return done(err);
				}
				if (!user) {
					return done(null, false, {
						message: 'Unknown user'
					});
				}
				if (!user.pwd == password) {
					return done(null, false, {
					message: 'Invalid password'
				});
			}
			console.log(user);
			return done(null, user);
			});
		}
	));
};
</pre>

### routes > index.js
<pre>
var mongoose = require('mongoose');
require('../model/user');
var User = mongoose.model('User');

var passport = require('passport');
var LocalStrategy = require('passport-local').Strategy;

router.get('/login', function(req, res) {
	res.render('login', { title: 'index'});
});

router.post('/login', passport.authenticate('local', { failureRedirect: '/login', failureFlash: true }),
  function(req, res) {
  	res.redirect('/');
  });

router.get('/logout' , function(req, res) {
	req.logout();
	res.redirect('/');
});

// change mothod index
router.get('/', function(req, res) {
	var userid = req.session.passport.user;
	console.log("userid" , userid);
	res.render('index', { title: 'index' ,userid : userid});
});
</pre>

### view > index.html
```
id login <%=userid%>
```
###step 9 include socket.io
### routes > index.js
<pre>
router.get('/socket', function(req, res) {
	if(req.isAuthenticated()){
		var userid = req.session.passport.user;
		res.render('socket', { title: 'index',userid : userid});
	}else {
		res.redirect('/login');
	}
});
</pre>

### view > socket.html
```
<!DOCTYPE html>
<html ng-app>
<script src="http://ajax.googleapis.com/ajax/libs/angularjs/1.0.8/angular.min.js"></script>
<script src="http://ajax.googleapis.com/ajax/libs/angularjs/1.0.8/angular-resource.min.js"></script>
<script src="https://cdn.socket.io/socket.io-1.0.6.js"></script>
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
<script>
var socket = io.connect('localhost:3000');
</script>
<body>
socketCtrl html
<div class="chatArea">
<ul class="messages"></ul>
</div>
user <%=userid%><input class="inputMessage" placeholder="Type here..."/> <input type="button" id="a" value="send"/>

</body>

<script>
var socket = io.connect('localhost:3000');

$(document).ready(function(){
  $("#a").click(function(){
 	var message = $('.inputMessage').val();
    if (message) {
      $('.inputMessage').val('');
     	var addChatMessage = {
        	message: message
      	};
     	console.log('ckuc' ,addChatMessage );
      socket.emit('new_message', addChatMessage);
    }


  });

  socket.on('re_message', function (data) {
		console.log('re',data);
	   $('.messages').append("<li>"+data.username + ": " + data.message.message+"</li>");
	});
});
</script>
</html>
```

### app.js
<pre>
var io = require('socket.io').listen(server);
io.sockets.on('connection', function (socket) {
  socket.on('new_message', function (data) {
  	console.log('new_message',data);
   	socket.emit('re_message', {
      username: 'test',
      message: data
    });
  });
});
</pre>

###step 10 socket.io

#### views->socket.html
```
<!DOCTYPE html>
<html ng-app>
<script src="http://ajax.googleapis.com/ajax/libs/angularjs/1.0.8/angular.min.js"></script>
<script src="http://ajax.googleapis.com/ajax/libs/angularjs/1.0.8/angular-resource.min.js"></script>
<script src="https://cdn.socket.io/socket.io-1.0.6.js"></script>
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
<body>
socketCtrl html
<input type="hidden" id="c" value="<%=userid%>"/><div class="num"></div><div>
<input type="button" id="b" value="conenct"/>
<div style="width: 200px; height: 100px; overflow-y: scroll;" id="area_cb">
</div>
<div class="chatArea">
<ul class="messages"></ul>
</div>
user : <%=userid%><input class="inputMessage" placeholder="Type here..."/> <input type="button" id="a" value="send"/>

</body>

<script>
var socket = io.connect('http://192.168.133.129:4000');

$(document).ready(function(){
	$("#a").click(function(){
		var message = $('.inputMessage').val();
	if (message) {
	  	$('.inputMessage').val('');
	 	var addChatMessage = {
	    	message: message
	  	};
		socket.emit('new_message', addChatMessage);
	}
	});

	$("#b").click(function(){
		socket.emit('add_user', $("#c").val());
		socket.emit('sub');
	});

	socket.on('re_sub' , function(data) {
		$('#area_cb').prepend("<p> -- "+data+" -- </p>");
  	});

	socket.on('user_joined', function (data) {
		$('.num').html(data.username + ": online -> total : " + data.numUsers);
	});
  	socket.on('re_message', function (data) {
		console.log('re',data);
   		$('.messages').append("<li>"+data.username + ": " + data.message.message+"</li>");
	});
});
</script>
</html>
```
#### socket.io
<pre>
var server = app.listen(4000)
var usernames = {};
var numUsers = 0;
var userSub = []; 
var cbarr = [];

var io = require('socket.io').listen(server);
io.sockets.on('connection', function (socket) {
  var addedUser = false;
  socket.on('add_user', function (username) {
    
    socket.username = username;
    usernames[username] = username;
    ++numUsers;
   
    socket.broadcast.emit('user_joined', {
      username: socket.username,
      numUsers: numUsers
    });
  });

  socket.on('new_message', function (data) {
  	socket.broadcast.emit('re_message', {
      username: 'test',
      message: data
    });
  });

  socket.on('sub' , function(user , cb) {
  	socket.join('random');
  });

  function callback(ran){
  	io.to('random').emit('re_sub' , ran);
  }

  setInterval(function(){
  	var ran = Math.floor(Math.random() * 100000);
  	callback(ran);
  }, 3000);

});
</pre>
end node

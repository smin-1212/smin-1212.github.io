---
layout: post
title: nodejs + express
tags: [javascript, js, es6, nodejs]
---

#### nodejs + express request
```javascript
var express = require('express'), http = require('http');

var app = express();

app.use(function(req, res, next){
    console.log('첫번째 미들웨어에서 요청을 처리함');
    req.user = 'mike';
    next();
});

app.use('/',function(req,res,next){
    console.log('두번째 미들웨어에서 요청을 처리함.');
    res.writeHead('200', {'Content-Type': 'text/html;charset=utf8'});
    res.end('<h1>Express 서버에서'+req.user +' 응답한 결과입니다.</h1>');
})

http.createServer(app).listen(3000, function(){
    console.log('Express 서버가 3000번 포트에서 시작됨');
});
```


#### nodejs, express request , return JSON
```javascript
var express = require('express'), http = require('http');

var app = express();

app.use(function(req, res, next){
    console.log('첫번째 미들웨어에서 요청을 처리함');
    res.send({name: "hello", age:10});
});
```


#### nodejs, express request, Post
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>로그인 테스트</title>
    </head>
    <body>
        <h1>Log - In</h1>
        <br>
        <form method="post" >
            <table>
                <tr>
                    <td><label>ID</label></td>
                    <td><input type="text" name="id"></td>
                </tr>
                <tr>
                    <td><label>PASSWORD</label></td>
                    <td><input type="password" name="password"></td>
                </tr>
            </table>
            <input type="submit" value="Request!" name="">
        </form>
    </body>
</html>
```

```javascript
var express = require('express'), http=require('http'), path=require('path');
var bodyParser = require('body-parser'), static = require('serve-static');

var app = express();
app.set('port', process.env.PORT || 3000);
app.use(bodyParser.urlencoded({extended: false}));
app.use(bodyParser.json());

// app.use('/public', static(path.join(__dirname, 'public')));
app.use('/', static(path.join(__dirname, 'public')));

app.use(function(req, res, next){
    console.log('First middleware request process');

    var paramId = req.body.id || req.query.id;
    var paramPassword = req.body.password || req.query.password;

    res.writeHead('200', {'Content-Type': 'text/html;charset=utf8'});
    res.write('<h1>Express Server Response Result.</h1>');
    res.write('<div><p>Param id : ' + paramId + '</p></div>');
    res.write('<div><p>Param password : '+ paramPassword + '</p></div>');
    res.end();
});

http.createServer(app).listen(3000, function(){
    console.log('Express 서버가 3000번 포트에서 시작');
});
```


#### Get 방식 요청
```javascript
var express = require('express'), http=require('http'), path=require('path');
var bodyParser = require('body-parser'), static = require('serve-static');
var router = express.Router();

var expressErrorHandler = require('express-error-handler');

var errorHndler = expressErrorHandler({
    static:{
        '404':'./public/404.html'
    }
});

router.route('/process/login/:name').post(function(req, res){
    console.log('/process/login/:name excuted');

    var paramName = req.params.name;
    var paramId = req.body.id || req.query.id;
    var paramPassword = req.body.password || req.query.password;

    res.writeHead('200', {'Content-Type': 'text/html;charset=utf8'});
    res.write('<h1>Express Server Response Result.</h1>');
    res.write('<div><p>Param Name : ' + paramName + '</p></div>');
    res.write('<div><p>Param id : ' + paramId + '</p></div>');
    res.write('<div><p>Param password : '+ paramPassword + '</p></div>');
    res.write('<br><br><a href="/public/login3.html">ReDirect Log-In page</a>');
    res.end();
});

router.route('/process/user/:id').get(function(req, res){
    console.log('/process/user/:id 처리함');

    var paramId = req.params.id;

    console.log('process/user 와 토큰 %s 를 이용해 처리함 ', paramId);

    res.writeHead('200', {'Content-Type':'text/html;charset=utf8'});
    res.write('<h1>Express 서버에서 응답한 결과입니다.</h1>');
    res.write('<div><p>Param id : '+paramId+'</p></div>');
    res.end();
});

var app = express();
app.set('port', process.env.PORT || 3000);
app.use(bodyParser.urlencoded({extended: false}));
app.use(bodyParser.json());
app.use('/public', static(path.join(__dirname, 'public')));

app.use('/', router);
app.use(expressErrorHandler.httpError(404));
app.use(errorHndler);

http.createServer(app).listen(3000, function(){
    console.log('Express 서버가 3000번 포트에서 시작');
});
```


## cookie
```javascript
var express = require('express'), http=require('http'), path=require('path');
var bodyParser = require('body-parser'), static = require('serve-static');
var expressErrorHandler = require('express-error-handler');
var cookieParser = require('cookie-parser');

var app = express();

var router = express.Router();

var errorHndler = expressErrorHandler({
    static:{
        '404':'./public/404.html'
    }
});

app.set('port', process.env.PORT || 3000);
app.use(bodyParser.urlencoded({extended: false}));
app.use(bodyParser.json());
app.use('/public', static(path.join(__dirname, 'public')));

app.use(cookieParser());

router.route('/process/showCookie').get(function(req, res){
    console.log('/process/showCookie 호출');
    res.send(req.cookies);
});

router.route('/process/setUserCookie').get(function(req, res){
    console.log('/process/setUserCookie 호출됨');
    res.cookie('user', {
        id:'mike',
        name:'MIKE',
        authorized : true
    });
    res.redirect('/process/showCookie');
});

router.route('/process/login/:name').post(function(req, res){
    console.log('/process/login/:name excuted');

    var paramName = req.params.name;
    var paramId = req.body.id || req.query.id;
    var paramPassword = req.body.password || req.query.password;

    res.writeHead('200', {'Content-Type': 'text/html;charset=utf8'});
    res.write('<h1>Express Server Response Result.</h1>');
    res.write('<div><p>Param Name : ' + paramName + '</p></div>');
    res.write('<div><p>Param id : ' + paramId + '</p></div>');
    res.write('<div><p>Param password : '+ paramPassword + '</p></div>');
    res.write('<br><br><a href="/public/login3.html">ReDirect Log-In page</a>');
    res.end();
});

router.route('/process/user/:id').get(function(req, res){
    console.log('/process/user/:id 처리함');

    var paramId = req.params.id;

    console.log('process/user 와 토큰 %s 를 이용해 처리함 ', paramId);

    res.writeHead('200', {'Content-Type':'text/html;charset=utf8'});
    res.write('<h1>Express 서버에서 응답한 결과입니다.</h1>');
    res.write('<div><p>Param id : '+paramId+'</p></div>');
    res.end();
});


app.use('/', router);
app.use(expressErrorHandler.httpError(404));
app.use(errorHndler);



http.createServer(app).listen(3000, function(){
    console.log('Express 서버가 3000번 포트에서 시작');
});

```


### Session 을 사용한 로그인 페이지
```javascript
var express = require('express'), http=require('http'), path=require('path');
var bodyParser = require('body-parser'), static = require('serve-static');
var expressErrorHandler = require('express-error-handler');
var cookieParser = require('cookie-parser');
var expressSession = require('express-session');

var app = express();

var router = express.Router();

var errorHndler = expressErrorHandler({
    static:{
        '404':'./public/404.html'
    }
});

app.set('port', process.env.PORT || 3000);
app.use(bodyParser.urlencoded({extended: false}));
app.use(bodyParser.json());
app.use('/public', static(path.join(__dirname, 'public')));

app.use(cookieParser());

app.use(expressSession({
    secret:'my Key',
    resave:true,
    saveUninitialized:true
}));

router.route('/process/product').get(function(req, res ){
    console.log('/process/product 호출됨');

    if(req.session.user){
        res.redirect('/public/product.html');
    }else{
        res.redirect('/public/login2.html');
    }
});

router.route('/process/showCookie').get(function(req, res){
    console.log('/process/showCookie 호출');
    res.send(req.cookies);
});

router.route('/process/setUserCookie').get(function(req, res){
    console.log('/process/setUserCookie 호출됨');
    res.cookie('user', {
        id:'mike',
        name:'MIKE',
        authorized : true
    });
    res.redirect('/process/showCookie');
});

router.route('/process/login').post(function(req, res){
    console.log('/process/login 호출됨');

    var paramId = req.body.id || req.query.id;
    var paramPassword = req.body.password || req.query.password;

    if(req.session.user){
        console.log("이미 로그인되어 상품페이지로 이동");
        res.redirect('/public/product.html');
    }else{
        req.session.user = {
            id:paramId,
            name:'MIKE',
            authorized : true
        };
        res.writeHead('200', {'Content-Type': 'text/html;charset=utf8'});
        res.write('<h1>로그인 성공</h1>');
        res.write('<div><p>Param id : ' + paramId + '</p></div>');
        res.write('<div><p>Param password : '+ paramPassword + '</p></div>');
        res.write('<br><br><a href="/process/product">상품페이지로 이동</a>');
	res.end();
    }
});

router.route('/process/logout').post(function(req, res){
    console.log('/process/logout 호출');

    if(req.session.user){
        console.log('logout~');
        req.session.destroy(function(err){
            if(err){throw err;}
            console.log('세션을 삭제하고 로그아웃');
            res.redirect('/public/login2.html');
        });
    }else{
        console.log('아직 로그인 되어있지 않습니다. ');
        res.redirect('/public/login2.html');
    }
});

/*
router.route('/process/login/:name').post(function(req, res){
    console.log('/process/login/:name excuted');

    var paramName = req.params.name;
    var paramId = req.body.id || req.query.id;
    var paramPassword = req.body.password || req.query.password;

    res.writeHead('200', {'Content-Type': 'text/html;charset=utf8'});
    res.write('<h1>Express Server Response Result.</h1>');
    res.write('<div><p>Param Name : ' + paramName + '</p></div>');
    res.write('<div><p>Param id : ' + paramId + '</p></div>');
    res.write('<div><p>Param password : '+ paramPassword + '</p></div>');
    res.write('<br><br><a href="/public/login3.html">ReDirect Log-In page</a>');
    res.end();
});
*/
router.route('/process/user/:id').get(function(req, res){
    console.log('/process/user/:id 처리함');

    var paramId = req.params.id;

    console.log('process/user 와 토큰 %s 를 이용해 처리함 ', paramId);

    res.writeHead('200', {'Content-Type':'text/html;charset=utf8'});
    res.write('<h1>Express 서버에서 응답한 결과입니다.</h1>');
    res.write('<div><p>Param id : '+paramId+'</p></div>');
    res.end();
});


app.use('/', router);
app.use(expressErrorHandler.httpError(404));
app.use(errorHndler);



http.createServer(app).listen(3000, function(){
    console.log('Express 서버가 3000번 포트에서 시작');
});

```
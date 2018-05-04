# node6------async-await
async\await只有ES7对应的node6之后的版本支持，如果node6、ES6支持这种需要添加对应babel支持

解决nodejs不支持async和await关键字的问题

1、$ npm install --save-dev babel-cli 
2、$ npm install --save-dev babel-preset-es2015 babel-preset-es2017  
3、Create .babelrc in the project root folder with the following contents:
 {
 "presets": ["es2015","es2017"] 
 }

4、Run your script with babel-node

$ babel-node async.js


package.json  文件

{
  "name": "testop",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "babel-node app.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "koa": "^2.5.1",
    "koa-bodyparser": "^2.5.0",
    "koa-router": "^7.4.0"
  },
  "devDependencies": {
    "babel-cli": "^6.26.0",
    "babel-preset-es2015": "^6.24.1",
    "babel-preset-es2017": "^6.24.1"
  }
}


app.js   文件


const Koa = require('koa');
const  router = require('koa-router')();
const bodyParser = require('koa-bodyparser'); //使用post提交数据时，解析ctx.request.body的对应数据

// 创建一个Koa对象表示web app本身:
const app = new Koa();
app.use(bodyParser());
app.use(async (ctx, next) => {
    console.log("22222");
    await next();
});
app.use(async (ctx,next)=>{
    console.log(`${ctx.request.method} ${ctx.req} ${ctx.request.url} ${ctx.request.path}`);
    if(ctx.request.path==="/no"){
        ctx.response.type = 'text/html';
        ctx.response.body = '<h1>no found!</h1>';
        ctx.response.status=404;
    }else{
        await next();
    }
});
// 对于任何请求，app将调用该异步函数处理请求：
app.use(async (ctx, next) => {
    if(ctx.request.path==="/"){
        ctx.response.type = 'text/html';
        ctx.response.body = '<h1>Hello, koa2!</h1>';
        ctx.response.status=200;
    }else{
        await next();
    }
});
router.get("/login",function(ctx,next){
    ctx.response.body = `<h1>Index</h1>
        <form action="/signin" method="post">
            <p>Name: <input name="name" value="koa"></p>
            <p>Password: <input name="password" type="password"></p>
            <p><input type="submit" value="Submit"></p>
        </form>`;
});

router.post("/signin",function (ctx,next) {
    console.log(ctx.request.body);
    var name = ctx.request.body.name || '',
        password = ctx.request.body.password || '';
    console.log(`signin with name: ${name}, password: ${password}`);
    if (name === 'koa' && password === '12345') {
        ctx.response.body = `<h1>Welcome, ${name}!</h1>`;
    } else {
        ctx.response.body = `<h1>Login failed!</h1>
        <p><a href="/">Try again</a></p>`;
    }
})

router.get("/index/:name",function(ctx,next){
    // ctx.throw(400,"no found");
    ctx.response.type="text/html";
    ctx.response.body="<h1>Hello,index"+ctx.params.name+"!!</h1>";
});

app.use(router.routes());

// 在端口3000监听:
app.listen(3000);
console.log('app started at port 3000...');



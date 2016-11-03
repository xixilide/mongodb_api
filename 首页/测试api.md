# Chrome 浏览器中测试 API

### 安装 body-parser
```
npm install --save body-parser
```

body-parser 是一个由 Expressjs 团队维护的 Express 中间件，它的功能是解析 HTTP 请求中的正文信息，并把这些信息存储到 req.body 对象中，比方说，客户端提交 form 表单的数据。
使用 body-parser

打开 index.js 文件，首先导入 body-parser 功能模块

var bodyParser = require('body-parser');
然后在 mongoose.connect(...) 代码之后，添加一行代码：

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false })); // 如果想使用 form 提交，这一行是必要的
解析 HTTP 请求正文的中的 JSON 数据，并存储到 req.body 对象中。

编写测试 API

在项目根目录下，新建一个 routes.js 文件，将存放本案例中的所有 API，添加一些代码：

var express = require('express');

module.exports = function(app) {
  app.get('/api', function(req, res) {
    res.json({message: 'get request!'})
  });

  app.post('/api', function(req, res) {
    console.log(req.body)
    res.json({message: 'post request!'})
  });
}
参考接口文档：

基本路由
res.json()
接下来，打开 index.js 文件，导入刚刚定义的路由

var routes = require('./routes');
然后，在 mongoose.connect(...); 之后添加代码，让路由生效：

routes(app);
测试 API

安装一个 Chrome 插件 Advanced REST client，一个具有 Material UI 设计风格的 API 调试工具

注意： 在向 'http://localhost:3000/api' 发送 POST 请求的时候，要设置 header 属性 Content-Type: application/json，并且发送的 JSON 数据格式必须符合规范，如下所示：

{
  "title": "test api",
  "content": "JSON"
}
注意：一定是双引号，不是单引号。JSON 数据格式正确，Post 请求发送成功之后，body-parser 会解析发送过来的 JSON 数据，并存储到 req.body 对象中，对象的数值可以通过 console.log(req.body) 打印到终端中查看。

等价的 curl 命令如下：

curl -H "Content-Type: application/json" -X POST -d '{"title":"test
api","content":"JSON"}' http://localhost:3000/posts
安装 morgan

另外，可以通过 morgan 记录每次 HTTP 请求的信息

npm install --save morgan
然后，打开 index.js 文件，添加代码：

var logger = require('morgan');
然后使用它：

app.use(logger('dev'));
这样，每次 HTPP 请求的信息都会遵照一定的格式在终端打印出来，上述采用的格式是 dev，格式如下：

:method :url :status :response-time ms - :res[content-length]
若向 /api 发送 HTTP GET 请求，则在终端显示这样的日志信息：

GET /api 200 4.317 ms - 26

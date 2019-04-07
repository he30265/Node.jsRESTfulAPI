# Node.jsRESTfulAPI

### 一、初始化项目

```
npm init
```
生成 package.json。


### 二、安装依赖

- express（node.js Web 应用框架）。
- body-parser：node.js 中间件，用于处理 JSON, Raw, Text 和 URL 编码的数据。
```
npm install express -s
npm install body-parse -s
```


### 三、模拟数据

我在 news.json 中添加了一些新闻数据，以供下边测试。

![](https://i.loli.net/2019/04/07/5ca9655f7aada.jpg)

[news.json 下载](https://note.youdao.com/)



### 四、新建 app.js 文件

app.js
```
const fs = require("fs"); // 文件模块
const path = require("path"); // 系统路径模块

const express = require("express"); // node.js Web 应用框架
const bodyParser = require("body-parser"); // node.js 中间件，用于处理 JSON, Raw, Text 和 URL 编码的数据

const app = express();
app.use(bodyParser.json()); // 返回一个只解析 json 的中间件，最后保存的数据都存放在 request.body 对象上
app.use(
  bodyParser.urlencoded({
    extended: true
  })
); // 返回的对象为任意类型

// 设置跨域访问
app.all("*", function(request, response, next) {
  response.header("Access-Control-Allow-Origin", "*"); // 设置跨域的域名，* 代表允许任意域名跨域
  response.header("Access-Control-Allow-Headers", "X-Requested-With");
  response.header(
    "Access-Control-Allow-Methods",
    "PUT,POST,GET,DELETE,OPTIONS"
  );
  response.header("X-Powered-By", " 3.2.1");
  response.header("Content-Type", "application/json;charset=utf-8");
  next();
});

// get getAPITest
app.get("/getAPITest", (request, response) => {
  request.statusCode = 200;
  const file = path.join(__dirname, "./news.json"); // 读取 json 文件
  fs.readFile(file, "utf-8", (err, data) => {
    if (err) {
      response.send("文件读取失败！");
    } else {
      response.send(data);
    }
  });
});

// post 请求接口：postAPITest
app.post("/postAPITest", (request, response) => {
  response.json(request.body);
});

// 配置服务端口
const server = app.listen(3000, () => {
  console.log("服务启动成功！打开 http://localhost:3000/");
});
```

启动服务，访问 localhost:3000/getAPITest

![](https://i.loli.net/2019/04/07/5ca966fd7a245.jpg)


### 五、新建 index.html 请求接口

index.html
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <script src='https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js'></script>
    <script>
        $(function () {
            // get 请求
            $.ajax({
                type: "get",
                url: "http://localhost:3000/getAPITest",
                success: function (data) {
                    console.log(data);
                },
                error: function (err) {
                    console.log(err)
                }
            });

            // post请求
            $.ajax({
                type: "post",
                url: "http://localhost:3000/postAPITest",
                data: {
                    name: 'liu',
                    password: "123456"
                },
                success: function (data) {
                    console.log(data);
                },
                error: function (err) {
                    console.log(err);
                }
            });
        })
    </script>
</body>

</html>
```

开启服务，下图表示两个接口调用成功。

![](https://i.loli.net/2019/04/07/5ca967e9305b7.jpg)

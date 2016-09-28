# Nodejs-Express

Studying Nodejs-Express

1.nodejs 下添加包的依赖（以 orm 为例）
  - 1. 在 package.json 中添加对包的依赖："orm":"~2.1.30",
  - 2. npm install 下载包: npm install （它会自动检索 package.json 下未安装的包 ）
  - 3. 或者直接 npm install orm;

2.npm start 启动顺序：
  - 1.package.json:"start": "node ./bin/www"
  - 2.node ./bin/www
  - 3.app.js 
  - 4.app.use('/', routes);

3.nodejs 项目目录结构

  - 1.package.json:定义了系统需要的其他的第三方模块、包
  - 2.node_modules: 存放通过npm安装的第三方包的地方
  - 3.routes: 路由控制器目录， js 文件
  - 4.view:   视图/模版目录，MVC的view层，存放所有的 html 页面
  - 5.public: 项目公共目录，其中又包含了images、javascripts、stylesheets目录,以及开源项目的使用，如DLShouwen
  - 6.app.js: 应用的入口(如何有 /bin/www,则会在 /bin/www 调用 app.js)

4.html 页面
```c
 - 1.//文档加载完毕执行
      $(function () {
          operFun();//操作函数
       });
 - 2.function operFun(){
      $.post("/rest/dosth", { "o_id": ""},//指定路由，即是rest.js文件下的 dosth post 方法
      function (data) {                           //传递的参数是 key:value 键值对，返回结果给 data
          if (data.length < 1) {
              $('.loadEffect').hide();
              return;
          } else {
              window.location.href="/";           //回到首页（当前页面）
              //window.open("/");                 //回到首页（打开新的标签页）
          }           
      }, "json");
    }
  - 3.router.post('/dosth', function (req, res) {res.send({"message":data});});
  - 4.router.get ('/index', function (req, res) {res.render('index', { title: title });});
  
```

5.nodejs 操作数据库 配置
```c
方式一：
 -database/setting.js文件：
 	module.exports = {
    	statistic_db_url : 'mysql://username:password@localhost/databasename'
	};
 -database/database.js文件：
 	var orm = require('orm');
	var settings = require('../database/setting.js');
	var db = orm.connect(settings.statistic_db_url);
	db.on('connect',function (err)
	{
	    if (err) {
	        console.log(err);
	        throw err;
	    }
	    console.log("suceee");
	});
	module.exports=db;

 -index.js 操作数据库
 	var settings = require('../database/setting.js');
	var db = require('../database/database.js');
	var orm = require("orm");

	var sql= 'SELECT * from 表名';
  	db.driver.execQuery(sql, function (err, results) {
	    if (err) {
	      res.render('error',{error:err});
	      return;
	    }
	    console.log(results);          //输出结果集
	    console.log(results['字段名']);//输出该字段内容
	});


方式二：
var mysql      = require('mysql');
var connection = mysql.createConnection({
  host     : localhost,      //远程MySQL数据库的ip地址
  user     : userame,        //用户名
  password : ppassword,      //密码
});
var database = databasename ;//数据库名
//连接数据库
connection.connect();
//使用数据库
connection.query('use'+' '+database+' ;');
//查询 orig_trans 表的详细信息
connection.query('SELECT * from orig_trans;',function(err,results,fields){
  if(err){
    console.log('GetData Error: ' + err.message);
    client.end();
    return;
  }
  //操作...
 });
 //关闭数据库
connection.end();

方式三：
var mysql = require('mysql');
var pool = mysql.createPool({
    host: localhost,
    user: username,
    password: password,
    database: databasename,
    port: 3306
});
var selectSQL = 'select * from orig_trans';
pool.getConnection(function (err, conn) {
    if (err) console.log("POOL ==> " + err);

    conn.query(selectSQL,function(err,rows){
        if (err) console.log(err);
        //console.log("SELECT ==> ");
        //...操作
        conn.release();
    });
});

```

6.nodejs Async 操作

Async 包括三部分：
  - 流程控制：简化十种常见流程的处理
  - 集合处理：如何使用异步操作处理集合中的数据
  - 工具类：几个常用的工具类
  - 
---------------------------------------------------------------------------------------------  
  - 1.在 package.json 中添加对 Async 的依赖："async": "*"
  - 2.npm install 安装 Async 包
  - 3.async.waterfall 方法
    waterfall(tasks,[calback]):多个函数依次执行，且前一个输出为后一个的输入，即每一个函数产生的值都将传递到下一个函数
                               如果中途出错，后面的函数不会执行，错误信息以及之前的结果将传给 waterfall 最终的 calback

```c
//经常会遇到需要插入数据到 tablenameC 时，需要验证其中的一些字段在 tabenameA ， tablenameB ... 中存在
 async.parallel([
        function (callback) {
            var sql="SELECT * FROM "+ tableameA+" WHERE 条件;
            db.driver.execQuery(sql, function (err, data) {
                console.log(data);
                if(data.length>0) callback(null,null);
                else callback(null,"XXX不存在 ");
            });
        },
        function (callback) {
            var sql="SELECT * FROM "+tablenameB;
            console.log(sql);
            db.driver.execQuery(sql,function (err,data) {
                if(data.length>0) callback(null,null);
                else callback(null,"BBB不存在 ");
            });
        }
    ],
    function (err,results) {
        var message="";
        if(results[0]) message+=results[0];
        if(results[1]) message+=results[1];
        console.log(message);
        if(message) res.send({"message":message+"fail"});
        else
        {
            sql="INSERT INTO "+tablenameC+;
            db.driver.execQuery(sql,function (err,data) {
               if(err)
               {
                   console.log(err);
                   throw err;
               }
               res.send({"message":"success"});
            });
        }
    });

```

7.js 时间格式转换 

```c
  var d = new Date('Thu May 12 2016 08:00:00 GMT+0800 (中国标准时间)');  
  trans_date=d.getFullYear() + '-' + (d.getMonth() + 1) + '-' + d.getDate() + ' ' + d.getHours() + ':' + d.getMinutes() + ':' + d.getSeconds();
  //yyyy-MM-dd hh:mm:ss 
  var d = new Date();  
  trans_date=d.getFullYear() + '-' + (d.getMonth() + 1) + '-' + d.getDate();

  var  dt = new Date(Date.parse(req.body.trans_date));
  trans_date= dt.getFullYear() + "-" + ("0" + (dt.getMonth() + 1)).substr(("0" + (dt.getMonth() + 1)).length - 2, 2) + "-" + ("0" + dt.getDate()).substr(("0" + dt.getDate()).length - 2, 2);
  
  function  changeDate(date) {
    var dt = new Date(Date.parse(date));
    return dt.getFullYear() + "-" + ("0" + (dt.getMonth() + 1)).substr(("0" + (dt.getMonth() + 1)).length - 2, 2) + "-" +
        ("0" + dt.getDate()).substr(("0" + dt.getDate()).length - 2, 2);
}
  
```

8.vim 操作

-  (1).删除每行“（）”里面的字符；
	 :%s/([^()]*)//g
-  (2)删除每行“（” 后面的字符；
	:%s/(.*//
-  (3)删除前 n 列
	:% s/^.\{n\}//g
-  (4)删除所有空白行
	: g/^\s*$/d


```c
//float 保留两位小数(四舍五入)
function toDecimal(x) {
    var f = parseFloat(x);
    if (isNaN(f)) {
        return;
    }
    f = Math.round(x*100)/100;
    return f;
}
//100000 ---> 100 000
function parseNum(num){
    var list = new String(num).split('').reverse();
    for(var i = 0; i < list.length; i++){
        if(i % 4 == 3){
            list.splice(i, 0, ' ');
        }
    }
    return list.reverse().join('');
}
```

MD5加密:

```c
  var crypto = require('crypto');
  //生成口令的散列值
  var md5 = crypto.createHash('md5');   //crypto模块功能是加密并生成各种散列
  var value= md5.update("value").digest('hex');
```

汉字转拼音(全拼/简拼)

```c
  var py  = require("pinyin");
  var pinyin = py("汉字", {
      style: py.STYLE_FIRST_LETTER, // 设置拼音风格:简拼
      heteronym: false
  });
  var apinyin = py("汉字", {
      style: py.STYLE_NORMAL,       // 设置拼音风格:全拼
      heteronym: false
  });
```

netstat -nlp | grep :3000

netstat -nlp | grep :3000 | awk '{print $7}'

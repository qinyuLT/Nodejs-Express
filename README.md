# Nodejs-Express

Studying Nodejs-Express

1.nodejs 操作数据库 配置
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
  host     : '192.168.84.72',//远程MySQL数据库的ip地址
  user     : 'qinyu',        //用户名
  password : 'qinyu',        //密码
});
var database = 'qinyu' ;     //数据库名
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
    host: '192.168.84.72',
    user: 'qinyu',
    password: 'qinyu',
    database: 'qinyu',
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

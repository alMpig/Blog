# node


	在 Node 中，采用 EcmaScript 进行编码
	没有 BOM、DOM
	和浏览器中的javascript不一样

## nrm

> 注意：npm只是单纯的提供了几个常用的下载包的URL地址，并能够让我们在几个地址之间，很方便的进行切换，但是我们每次装包的时候，使用的装包工具永远都是，npm i ***

* 运行`npm i nrm -g`全局安装nrm包；
* 使用`nrm ls`查看当前所有可用的镜像源地址以及当前所使用的镜像源地址
* 使用`nrm use npm`或者`nrm use taobao`切换不同的镜像源地址

## 创建一个简单的http服务器程序

```
//1.加载http模块
const http = require('http');
//2.创建一个http服务器对象
var server = http.createServer();
//3.监听用户请求事件(request事件)
//request 对象包含了用户请求报文中的所有内容，通过request对象可以获取所有用户提交过来的数据
//response 对象用来向用户相应一些数据，当服务器要向客户端相应数据的时候必须使用response对象
//有了 request 对象 response 对象，就既可以获取用户提交的数据，也可以向用户相应数据了
server.on('request',function(req,res){
	//解决乱码的思路，服务器通过设置http响应报文头，告诉浏览器使用相应的代码来解析网页
	res.setHeader('Content-Type','text/html;charset=utf-8')
	res.write('Hello <h1>World</h1>!!!! 世界')
	//对于每一个请求，服务器必须结束响应，否则客户端（浏览器）会一直等待服务器响应结束
	res.end()
})
//4.启动服务器
server.listen(8888,function(){
	console.log('http://localhost:8888');
})
```
##  简化版

```
const http = require('http');
http.createServer(function(req,res){
	res.setHeader('Content-Type','text/html;charset=utf-8')
	//通过req.url获取用户请求的路径，根据不同的请求路径服务做出不同的相应
	if(req.url === '/'){
		res.end('Hello <h1>World</h1>!!!! 世界');
	}else{
		res.end('404');
	}
}).listen(8888,function(){
	console.log('http://localhost:8888');
})
```

## 读取html文件并返回给浏览器

```
const http = require('http');
//加载fs模块
var fs = require('fs');
//加载path模块
var path = require('path');
http.createServer(function(req,res){
	//通过req.url获取用户请求的路径，根据不同的请求路径服务做出不同的相应
	if(req.url === '/index.html' || req.url === '/'){
		//读取index.html文件  join(拼接地址)
		fs.readFile(path.join(__dirname,'index.html'),function(err,data){
			if(err){
				throw err;
			}
			//把读取到的index.html中的内容直接发送到浏览器
			res.end(data);
		})
	}else{
		res.end('404');
	}
}).listen(8888,function(){
	console.log('http://localhost:8888');
})
```

## 读取/写入文件操作

```
const http = require('http');
//加载fs模块
var fs = require('fs');
//加载path模块
var path = require('path');
http.createServer(function(req,res){
	//__dirname得到的是文件的当前路径
	//写入文件
	fs.writeFile(path.join(__dirname,'index.text'),'大家好','utf8',function(err){
		//err永远都是错误信息
		if(err){
			throw err;
		}
		res.write('写入成功');
	});
	//读取文件
	fs.readFile(path.join(__dirname,'index.text'),'utf8',function(err,data){
		if(err){
			throw err;
		}
		res.end(data);
	});
}).listen(8888,function(){
	console.log('http://localhost:8888');
})
```

## 判断错误
```
if(err.code === 'ENOENT'){}
```

## 访问图片,css

```
res.setHeader('Content-Type','image/png');
res.setHeader('Content-Type','text/css');
```

## 模拟Apache

```
//导入模块
var http = require('http');
//拼接路径
var path = require('path');
//读取文件
var fs = require('fs');
//Content-Type类型
var mime = require('mime');
//创建服务
http.createServer(function(req,res){
	//2. 获取public目录的完整路径
	var publicDir = path.join(__dirname,'public');
	//3. 根据public的路径请求，最终计算出用户请求的静态资源的完整路径
	var filename = path.join(publicDir,req.url);
	//4. 根据文件的完整路径读取文件，如果读取到了，则把文件返回给用户，如果访问不到返回404
	fs.readFile(filename,function(err,data){
		if(err){
			//err如果存在那么说明有报错
			res.end('文件不存在 404');
		}else{
			//通过第三方模块 mime 来判断不同的资源对应不同的Content-Type类型
			res.setHeader('Content-Type',mime.getType(filename));
			//如果找到用户要读取的文件，那么直接把该文件返回给用户
			res.end(data)
		}
	})
	//启动9090端口服务器
}).listen(9090,function(){
	console.log('http://localhost:9090');
})
```

> 在请求服务器的时候，请求的 url 就只是一个标识

# 封装（入口文件、业务逻辑文件）

## index.js - 入口文件

```


//当前项目（包）的入口文件

// 1.加载http模块
var http = require('http');


//加载context模块
var context = require('./context.js')
//加载路由模块
var router = require('./router.js')
//加载配置模块
var config = require('./config.js');
// 2.创建服务
http.createServer(function(req,res){
	//调用context模块函数，并将 req 和 res 对象传递给 context.js 模块
	context(req,res);
	//调用路由模块并返回值（函数），并将 req 和 res 对象传递给 router.js 模块
	router(req,res);
}).listen(config.port,function(){
	console.log('http://localhost:'+config.port);
});
```
## config.js - 配置文件

```
var path = require('path')

module.exports = {
	"port":9090,
	"dataPath":path.join(__dirname,'data','data.json')
}
```
## context.js - req,res扩展文件

```
//该模块负责 req 和 res 对象进行扩展

//加载url模块
var url = require('url');
//加载fs文件加载模块
var fs = require('fs');
var mime = require('mime');
var _ = require('underscore');
//让当前模块对外暴露一个函数，通过这个函数将index.js中的req,res传递到当前context.js这个模块中
module.exports = function(req,res){
	//把url提取成一个可用的对象
	var urlObj = url.parse(req.url.toLowerCase(),true);
	//把请求方法req.method转换成小写
	req.method = req.method.toLowerCase();
	//1. 为 req 增加一个 query 属性，该属性中保存的就是用户 get 请求提交过来的数据
	// - req.query
	req.query = urlObj.query;
	//2. 为 req 增加一个 pathname 属性 
	// - req.pathname
	req.pathname = urlObj.pathname;
	//3. 为res 增加一个 render 函数
	res.render = function render(fileName,tplData){
		fs.readFile(fileName,function(err,data){
			if(err){
				res.writeHead(404,'Not Found',{
					'Content-Type':'text/html;charset=utf-8'
				})
				res.end('404,Page Not Found.');
				return;
			}
			if(tplData){
				//如果用户传递了模板数据，表示要进行模板替换
				//那么就使用 underscore 的 template 方法进行替换
				var compiled = _.template(data.toString('utf8'));
				data = compiled(tplData);
			}
			//判断当前请求的是什么，并且返回对应的报文头
			res.setHeader('Content-Type',mime.getType(fileName))
			res.end(data);
		})
	}
}
```

## router.js - 路由模块

```
//该模块负责封装所有路由判断代码
var handler = require('./handler.js')
module.exports = function(req,res){
if ( req.pathname === '/' || req.pathname === '/index.html' && req.method === 'get' ) {
		handler.index(req,res)
	} else if (req.url === '/list.html' && req.method === 'get' ) {
		handler.list(req,res)
	} else if (req.pathname === '/item' && req.method === 'get' ) {
		handler.item(req,res)	
	} else if (req.url === '/submit.html' && req.method === 'get' ) {
		handler.submit(req,res)
	} else if (req.pathname === '/add' && req.method === 'get' ) {
		handler.addGet(req,res)
	} else if (req.url === '/add' && req.method === 'post' ) {
		handler.addPost(req,res)
	} else if(req.pathname === '/del' && req.method === 'get' ){
		handler.del(req,res)
	} else if (req.url.startsWith('/static') && req.method === 'get' ) {
		handler.static(req,res)
	} else {
		handler.handlerErre(req,res);
	}
}
```

## handler.js 业务处理

```


//该模块负责对具体的业务进行处理
//buffer格式转换
var querystring = require('querystring');
var fs = require('fs');
var path = require('path');
var config = require('./config.js');
//处理请求 / 和 index 的业务方法
module.exports.index = function(req,res){
	res.render(path.join(__dirname,'views','index.html'))
}
//处理 静态资源
module.exports.static = function(req,res){
	res.render(path.join(__dirname,req.url))
}
//处理列表
module.exports.item = function(req,res){
	readNewData(function(list_news){
			var model = null;
		for(var i = 0 ; i < list_news.length; i++){
			if(list_news[i].id.toString() === req.query.id){
				model = list_news[i];
				break;
			}
		}
		if(model){
			res.render(path.join(__dirname,'views','item.html'),{abc:model})
		}else{
			res.end('No Such Item');
		}
	})
}
//处理添加页面
module.exports.submit = function(req,res){
	res.render(path.join(__dirname,'views','submit.html'))
}
//处理新增get
module.exports.addGet = function(req,res){
	readNewData(function(list){
		req.query.id = Date.parse(new Date());
		list.push(req.query);
		writeNewData(JSON.stringify(list),function(){
			res.statusCode = 302;
			res.statusMessage = 'Found';
			res.setHeader('Location','/list.html');
			res.end();
		})
	})
}
//处理新增post方法
module.exports.addPost = function(req,res){
	readNewData(function(list){
		postBodyData(req,function(postData){
			postData.id = Date.parse(new Date());
			list.push(postData);
			writeNewData(JSON.stringify(list),function(){
				res.statusCode = 302;
				res.statusMessage = 'Found';
				res.setHeader('Location','/list.html');
				res.end();
			})
		})
	})
}
//处理删除
module.exports.del = function(req,res){
	readNewData(function(list_news){
		for(var i = 0 ; i < list_news.length; i++){
			if(list_news[i].id.toString() === req.query.id){
				list_news.splice(i,1);
				break;
			}
		}
		writeNewData(JSON.stringify(list_news),function(){
			res.statusCode = 302;
			res.statusMessage = 'Found';
			res.setHeader('Location','/list.html');
			res.end();
		})
		
	})
}
//处理数据列表
module.exports.list = function(req,res){
	//读取data.js文件中的数据
	readNewData(function(list){
		res.render(path.join(__dirname,'views','list.html'),{list:list});
	})
}
//处理404
module.exports.handlerErre = function(req,res){
	//可以写成一个好看的404.html
	//这里是写死了
	res.writeHead(404,'Not Found',{
		'Content-Type':'text/html;charset=utf-8'
	})
	res.end('404,Page Not Found.');
}

//封装一个读取data.json函数

function readNewData(callback){
	fs.readFile(config.dataPath,'utf8',function(err,data){
		if(err && err.code !== 'ENOENT'){
			throw err
		}
		var list = JSON.parse(data || '[]');
		callback(list);
	});


}

//封装一个写入data.json函数
function writeNewData(data,callback){
	fs.writeFile(config.dataPath,data,function(err){
		if(err){
			throw err;
		}
		//调用 callback() 来执行写入成功数据后的操作
		callback()
	})
}

//封装一个获取用户 post 提交的数据的方法

function postBodyData(req,callback){
	var array = [];
	req.on('data',function(chunk){
		array.push(chunk);
	});
	req.on('end',function(){
		var postBody = Buffer.concat(array);
		postBody = postBody.toString('utf8');
		postBody = querystring.parse(postBody);

		//把用户提交过来的数据提交出去
		callback(postBody)
	});
}

```

# express

mkdir express-deom

cd express-deom

npm init -y

npm i -S express

## index.js 

```

// app.js 模块职责： 负责启动服务


// 1. 加载 express 模块
var express = require('express');
//    加载配置模块
var config = require('./config.js')
//    加载路由模块
var router = require('./router.js')

// 2. 创建 APP 对象
var app = express();

// 3. 注册路由 设置 app 与 router 相关联
app.use(router);
// 上面等价于下下面这种写法，不传'/',默认也会传
// app.user('/',function(req,res){
// 	res.end('ok')
// })


// 4. 启动服务
app.listen(config.port,function(){
	console.log('http://localhost:' + config.port);
})
```

## config.js 

```
module.exports = {
	port:9090
}
```

## handler.js 

```
// 加载 ejs 模板（渲染并替换模板数据）
var ejs = require('ejs');
var path = require('path');

module.exports.index = function (req,res){
	//sendFile()方法虽然可以读取对应的文件并返回，但是我们不使用
	//原因是，将来我们要对 index.html 中的模板代码进行执行并替换
	//render 方法是不能用的，需要为express配置一个模板引擎，然后才可以使用
	ejs.renderFile(path.join(__dirname,'views','index.html'),{name:'张三'},function(err,result){
		if(err){
			throw err
		}
		res.send(result)
	})
}
```

## router.js 

```
// 1. 加载 express 模块
var express = require('express');
// 加载业务模块
var handler = require('./handler.js');
// 创建一个 router 对象 （是个对象，也是个函数）
var router = express.Router()
var path = require('path');
// 2. 通过 router 对象设置（挂载）路由
router.get( '/' , handler.index );
// 实现静态文件托管，判断请求是不是以/static开头，是的话就拼接路径
router.use('/static',express.static(path.join(__dirname,'static')))

// 3. 返回 router 对象
module.exports = router;
```

# node 操作 mysql

## mysql.config

```
//配置链接数据库参数
module.exports = {
    host : 'localhost',
    port : 3306,//端口号
    database : 'nodetest',//数据库名
    user : 'root',//数据库用户名
    password : 'root'//数据库密码
};
```

## mysql.js 

```
let mysql = require('mysql');//引入mysql模块
var databaseConfig = require('./mysql.config');  //引入数据库配置模块中的数据

//向外暴露方法
module.exports = {
    query : function(sql,params,callback){
        //每次使用的时候需要创建链接，数据操作完成之后要关闭连接
        var connection = mysql.createPool(databaseConfig);        
        connection.connect(function(err){
            if(err){
                console.log('数据库链接失败');
                throw err;
            }
         //开始数据操作
         //传入三个参数，第一个参数sql语句，第二个参数sql语句中需要的数据，第三个参数回调函数
        connection.query( sql, params, function(err,results,fields ){
           if(err){
                console.log('数据操作失败');
                throw err;
            }
            //将查询出来的数据返回给回调函数
            callback && callback(results, fields);
            //results作为数据操作后的结果，fields作为数据库连接的一些字段
            //停止链接数据库，必须再查询语句后，要不然一调用这个方法，就直接停止链接，数据操作就会失败
             connection.end(function(err){
                  if(err){
                      console.log('关闭数据库连接失败！');
                      throw err;
                  }
              });
           });
       });
    }
};

```

## 操作（增删改查）

```
// 查询实例
// db.query('select name,id from t_user WHERE id=1', [],function(result,fields){
//     console.log('查询成功');
//     console.log(result);
// });
//添加实例
// var  addSql = 'INSERT INTO t_user(username,password) VALUES(?,?)';
// var  addSqlParams =['咕噜先森', '666'];
// db.query(addSql,addSqlParams,function(result,fields){
//     console.log('添加成功')
// })
//修改
// var sql = 'UPDATE t_user SET username = ? WHERE id = ?';
// var  addSqlParams =['咕噜先森',1];
// db.query(sql,addSqlParams,function(result,fields){
//     console.log('修改成功')
// })
//删除
// var sql = 'DELETE FROM t_user  WHERE id = ?';
// var  addSqlParams =1;
// db.query(sql,addSqlParams,function(result,fields){
//     console.log('删除成功')
// })
```


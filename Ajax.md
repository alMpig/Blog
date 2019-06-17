# ajax

## 初步茅庐

> 使用Ajax发送请求需要如下几步：

```javascript
// 1、创建XMLHttpRequest对象
var xhr = null;
if (window.XMLHttpRequest){
    // code for IE7+, Firefox, Chrome, Opera, Safari
    xhr = new XMLHttpRequest();
}else{
    // code for IE6, IE5
    xhr = new ActiveXObject("Microsoft.XMLHTTP");
}
// 2、准备发送（方式，地址[get传参]，默认是异步发送请求，false是同步）
//encodeURI()针对IE用来对中文参数进行编码，防止乱码
var param = 'username='+uname+'&password='+pw
xhr.open('get','url?'+encodeURI(param),true);
// 3、执行发送动作（get请求直接传null,post传入相应的数据）
xhr.send(null);
//[post请求]需要设置请求头
//xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");
//xhr.send(param);不需要转码
// 4、指定回调函数
xhr.onreadystatechange = function(){
    //AJAX运行步骤与状态值说明
    //xhr.readyState ==  0: 请求未初始化
    //xhr.readyState ==  1: 服务器连接已建立
    //xhr.readyState ==  2: 请求已接收
    //xhr.readyState ==  3: 请求处理中
    //xhr.readyState ==  4: 请求已完成，且响应已就绪
    if(xhr.readyState == 4){
        //AJAX状态码说明(详情：https://www.cnblogs.com/liu-fei-fei/p/5618782.html)
        //[200]表示响应成功
        //[404]没找找到请求的资源
        //[500]服务器端错误
        if(xhr.status == 200){
            //服务器返回来的文本数据
            var data = xhr.responseText;
            //根据服务器返回来的文本数据进行判断再执行下一步操作
            console.log(JSON.parse(data))
        }
    }
}
```

## json

```javascript
/*
    json数据和普通的js对象的区别：
    1、json数据没有变量
    2、json形式的数据结尾没有分号
    3、json数据中的键必须用双引号包住
*/
var str = '{"name":"zhangsan","age":"12"}';
var obj = JSON.parse(str);//把json形式的字符串转成对象
var str1 = JSON.stringify(obj);//把对象转成字符串

```

## 单线程+事件队列

```javascript
事件队列中的任务执行的条件：
    1、主线程已经空闲
    2、任务满足触发条件
        2.1、定时函数（延时时间已经达到）
        2.2、事件函数（特定事件被触发）
        2.3、ajax的回调函数（服务器端有数据相应）
		console.log(1);
        setTimeout(function(){
            console.log(2);
        },10);
        var sum = 0;
        for(var i=0;i<100000000;i++){
            sum += i;
        }
        console.log(sum);
        console.log(3);
```

## 封装ajax

```javascript

function ajax(obj){
    // 默认参数
    var defaults = {
        type : 'get',
        data : {},
        url : '#',
        dataType : 'text',
        async : true,
        success : function(data){console.log(data)}
    }
    // 处理形参，传递参数的时候就覆盖默认参数，不传递就使用默认参数
    for(var key in obj){
        defaults[key] = obj[key];
    }
    // 1、创建XMLHttpRequest对象
    var xhr = null;
    if(window.XMLHttpRequest){
        xhr = new XMLHttpRequest();
    }else{
        xhr = new ActiveXObject('Microsoft.XMLHTTP');
    }
    // 把对象形式的参数转化为字符串形式的参数
    /*
    {username:'zhangsan','password':123}
    转换为
    username=zhangsan&password=123
    */
    var param = '';
    for(var attr in obj.data){
        param += attr + '=' + obj.data[attr] + '&';
    }
    if(param){
        param = param.substring(0,param.length - 1);
    }
    // 处理get请求参数并且处理中文乱码问题
    if(defaults.type == 'get'){
        defaults.url += '?' + encodeURI(param);
    }
    // 2、准备发送（设置发送的参数）
    xhr.open(defaults.type,defaults.url,defaults.async);
    // 处理post请求参数并且设置请求头信息（必须设置）
    var data = null;
    if(defaults.type == 'post'){
        data = param;
        xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
    }
    // 3、执行发送动作
    xhr.send(data);
    // 处理同步请求，不会调用回调函数
    if(!defaults.async){
        if(defaults.dataType == 'json'){
            return JSON.parse(xhr.responseText);
        }else{
            return xhr.responseText;
        }
    }
    // 4、指定回调函数（处理服务器响应数据）
    xhr.onreadystatechange = function(){
        if(xhr.readyState == 4){
            if(xhr.status == 200){
                var data = xhr.responseText;
                if(defaults.dataType == 'json'){
                    // data = eval("("+ data +")");
                    data = JSON.parse(data);
                }
                defaults.success(data);
            }
        }
    }
}
```

## sublime插件

### sublimeServer

> 可以为我们启动静态服务器，这样就可以发送ajax请求，当然也可以直接打开html文件

1. 安装

2. 启动 

   - Tools > SublimeServer > Star SublimeServer

3. 配置(为了能打开.json文件)

   - 在配置文件添加 Tools > SublimeServer > Settings

   * ".json":"application/json"
   * 配置完重启

### jsonView

- chrome插件

> 在浏览器打开json文件可以格式化

### 更改默认浏览器

> Preferences > Package Settings > Settings - User

```javascript
{
	"default_browser":"chrome"
}
```

# ajax跨域

- script标签支持跨域
- 把请求路径写到src中，服务器调用success()，并把请求的数据传入到实参中

```javascript
// 这里是默认的回调函数名称
    // expando: "jQuery" + ( version + Math.random() ).replace( /\D/g, "" ),
    var cbName = 'jQuery' + ('1.11.1' + Math.random()).replace(/\D/g,"") + '_' + (new Date().getTime());
    if(defaults.jsonpCallback){
        cbName = defaults.jsonpCallback;
    }

    // 这里就是回调函数，调用方式：服务器响应内容来调用
    // 向window对象中添加了一个方法，方法名称是变量cbName的值
    window[cbName] = function(data){
        defaults.success(data);//这里success的data是实参
    }

    var param = '';
    for(var attr in defaults.data){
        param += attr + '=' + defaults.data[attr] + '&';
    }
    if(param){
        param = param.substring(0,param.length-1);
        param = '&' + param;
    }
    var script = document.createElement('script');
    script.src = defaults.url + '?' + defaults.jsonp + '=' + cbName + param;
    var head = document.getElementsByTagName('head')[0];
    head.appendChild(script);
```

# 模仿jQuery封装AJAX

```javascript

function ajax(obj){
    var defaults = {
        type : 'get',
        async : true,
        url : '#',
        dataType : 'text',
        jsonp : 'callback',
        data : {},
        success:function(data){console.log(data);}
    }

    for(var key in obj){
        defaults[key] = obj[key];
    }

    if(defaults.dataType == 'jsonp'){
        ajax4Jsonp(defaults);
    }else{
        ajax4Json(defaults);
    }
}

function ajax4Json(defaults){
    // 1、创建XMLHttpRequest对象
    var xhr = null;
    if(window.XMLHttpRequest){
        xhr = new XMLHttpRequest();
    }else{
        xhr = new ActiveXObject('Microsoft.XMLHTTP');
    }
    // 把对象形式的参数转化为字符串形式的参数
    /*
    {username:'zhangsan','password':123}
    转换为
    username=zhangsan&password=123
    */
    var param = '';
    for(var attr in defaults.data){
        param += attr + '=' + defaults.data[attr] + '&';
    }
    if(param){
        param = param.substring(0,param.length - 1);
    }
    // 处理get请求参数并且处理中文乱码问题
    if(defaults.type == 'get'){
        defaults.url += '?' + encodeURI(param);
    }
    // 2、准备发送（设置发送的参数）
    xhr.open(defaults.type,defaults.url,defaults.async);
    // 处理post请求参数并且设置请求头信息（必须设置）
    var data = null;
    if(defaults.type == 'post'){
        data = param;
        xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
    }
    // 3、执行发送动作
    xhr.send(data);
    // 处理同步请求，不会调用回调函数
    if(!defaults.async){
        if(defaults.dataType == 'json'){
            return JSON.parse(xhr.responseText);
        }else{
            return xhr.responseText;
        }
    }
    // 4、指定回调函数（处理服务器响应数据）
    xhr.onreadystatechange = function(){
        if(xhr.readyState == 4){
            if(xhr.status == 200){
                var data = xhr.responseText;
                if(defaults.dataType == 'json'){
                    // data = eval("("+ data +")");
                    data = JSON.parse(data);
                }
                defaults.success(data);
            }
        }
    }
}
function ajax4Jsonp(defaults){
    // 这里是默认的回调函数名称
    // expando: "jQuery" + ( version + Math.random() ).replace( /\D/g, "" ),
    var cbName = 'jQuery' + ('1.11.1' + Math.random()).replace(/\D/g,"") + '_' + (new Date().getTime());
    if(defaults.jsonpCallback){
        cbName = defaults.jsonpCallback;
    }

    // 这里就是回调函数，调用方式：服务器响应内容来调用
    // 向window对象中添加了一个方法，方法名称是变量cbName的值
    window[cbName] = function(data){
        defaults.success(data);//这里success的data是实参
    }

    var param = '';
    for(var attr in defaults.data){
        param += attr + '=' + defaults.data[attr] + '&';
    }
    if(param){
        param = param.substring(0,param.length-1);
        param = '&' + param;
    }
    var script = document.createElement('script');
    script.src = defaults.url + '?' + defaults.jsonp + '=' + cbName + param;
    var head = document.getElementsByTagName('head')[0];
    head.appendChild(script);
}
```

## 不跨域调用

```javascript
ajax({
    type:'get',
    url:'同源url',
    dataType:'json',
    success:function(data){
        console.log(data.username,data.password);
    }
});
```

## 跨域调用

```javascript
ajax({
    type:'get',
    url:'跨域url',
    dataType:'jsonp',
    data:{username:'zhangsan',password:'123'},
    jsonp:'cb',
    jsonpCallback:'abc',
    success:function(data){
        console.log(data);
    }
});
```


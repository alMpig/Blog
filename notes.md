# 复习Dom操作
## 创建
```
	创建元素节点: document.createElement( 'div' );
	创建文本节点: document.createTextNode();
	创建属性节点: document.createAttribute( 'date-name' );
	innerHTML\innerText
    克隆节点: node.cloneNode(true);
    值为空则只复制本身  为true克隆子元素
```

## 加入
```
	追加到: appendChild
	重新赋值: innerHTML
	将新元素插入到旧元素的前面: node.insertBefore( '新元素' , '旧元素' );
```
## 获取节点、元素
```
    获取上一个兄弟节点: node.previousSibling;
    获得下一个兄弟节点: node.nextSibling;
    获得父元素: node.parentNode;
    获得最后一个子元素: node.firstChild;
    获得第一个子元素: node.lastChild;
    获得子元素集合: node.childNodes;
    获取伪元素: getComputedStyle(this,':after');
    获取style: getComputedStyle(this,null)["background"];
    获得属性集合: node.attributes['name'];
    获取倒数第n个元素: node[node.length-2];
    获取全部子节点:node.children;
```
##  操作class
```
   dom.classList.add('add');新增
   dom.classList.remove('red');删除
   dom.classList.toggle('toggle');有就删除没有就加上
   dom.classList.contains('add'); 判断有没有
```
##  操作dom
```
document.querySelector('ul').onclick = function(e){
  var thisli = e.target;
  获取当前点击的li
}
获取自定义属性集合 data-name='ls' data-name-age='13'
var set = dom.dataset ;
获取单个
var name = set.name;
var age = set.nameAge;
设置
set.name = 'gg';


```
## 获取属性值
```
	获取属性节点: node.getAttributeNode( '属性名' );
	获取属性节点的值: node.nodeValue;
    获得元素的节点名称: node.nodeName;
	获取属性: node.getAttribute( '属性名' );
    判断节点类型: node.nodeType;
    元素节点: 1 ;属性节点 : 2 ;
```
## 删除元素节点
```
	node.removeChild( div );
	node.innerHTML = '';
```
## 删除属性
```
	删除属性节点: node.removeAttributeNode( attrnode );
	直接删除属性: node.removeAttribute( '属性名' );
```
## input
```
    跳转到输入框: node.focus();
```
## div代替textarea实现输入框高度随内容变化
```
#leave-message-textarea{
    width: 100%; 
    min-height:20px;
    max-height:70px;
    outline: 0;
    border: 1px solid #000;
    font-size: 13px;
    overflow-x: hidden;
    overflow-y: auto;
    -webkit-user-modify: read-write-plaintext-only;
}
[contentEditable=true]:empty:not(:focus):before{
    content:attr(data-text);
}
<div id="leave-message-textarea" contenteditable="true" data-text="输入留言"></div>
```
## ios点击阴影去掉
### input,textad
```
outline: none;-webkit-appearance: none;-webkit-tap-highlight-color:rgba(255,0,0,0);
```
### a\btn
```
-webkit-tap-highlight-color:rgba(255,0,0,0);
```
## 向左向上分别平移自身的一半

```
  position: absolute;  
  top: 50%;  
  left: 50%;  
  transform: translate(-50%, -50%);
  -webkit-transform: translate(-50%, -50%);  
  -moz-transform: translate(-50%, -50%); 
```
## 文字超出..

```
input输入框长度限制：maxlength="10";
p{overflow: hidden;display: -webkit-box;-webkit-line-clamp: 2;-webkit-box-orient: vertical;word-break: break-all;}
字符换行:word-break: break-all;
```

## 小图标
```
 <link rel="shortcut icon" href="favicon.ico" type="image/x-icon" />
```
## css处理
```
select {
  /*Chrome和Firefox里面的边框是不一样的，所以复写了一下*/
  border: solid 1px #000;

  /*很关键：将默认的select选择框样式清除*/
  appearance:none;
  -moz-appearance:none;
  -webkit-appearance:none;

  /*在选择框的最右侧中间显示小箭头图片*/
  background: url("http://ourjs.github.io/static/2015/arrow.png") no-repeat scroll right center transparent;


  /*为下拉小箭头留出一点位置，避免被文字覆盖*/
  padding-right: 14px;
}
input,textarea {outline: none;}/* 去除点击时的外边框 */
ul, ol, li {list-style: none;}/* 重置列表元素 */
```
## 命名规则
```
页头：header
登录条：loginBar
标志：logo
侧栏：sideBar
广告：banner
导航：nav
子导航：subNav
菜单：menu
子菜单：subMenu
搜索：search
滚动：scroll
页面主体：main
内容：content
标签页：tab
文章列表：list
提示信息：msg
小技巧：tips
栏目标题：title
加入：joinus
指南：guild
服务：service
热点：hot
新闻：news
下载：download
注册：register
状态：status
按钮：btn
投票：vote
合作伙伴：partner
友情链接：friendLink
页脚：footer
版权：copyRight
链　接：link
容　器：container
图　标：icon
信息： info
外　套：　　wrap
主导航：　　mainNav
子导航：　　subNav
页　脚：　　footer
整个页面：　content
页　眉：　　header
页　脚：　　footer
商　标：　　label
标　题：　　title
主导航：　　mainNav（globalNav）
顶导航：　　topNav
边导航：　　sidebar
左导航：　　leftsideBar
右导航：　　rightsideBar
标　志：　　logo
标　语：　　banner
菜单内容1： menu1Content
菜单容量：　menuContainer
子菜单：　　subMenu
边导航图标：sidebarIcon
注释：　　　note
面包屑：　　breadCrumb(即页面所处位置导航提示）
容器：　　　container
内容：　　　content
搜索：　　　search
登陆：　　　login
功能区：　　shop(如购物车，收银台)
当前的　　　current
主要的 master.css
布局，版面 layout.css
专栏 columns.css
文字 font.css
打印样式 print.css
主题 themes.css
全局样式：global.css；
框架布局：layout.css；
字体样式：font.css；
链接样式：link.css；
打印样式：print.css；
```
## input 输入限制

```
只能输入数字onkeyup="value=value.replace(/[^\d]/g,'')"
判断文字 /[\u4e00-\u9fa5]/
```


## 去掉默认样式
```
s{text-decoration: blink;}
i,em{font-style:normal;}
label{font-weight:normal}
```
## CSS选择器
```
1. 选择属于其父元素最后一个子元素每个p元素。
p:last-child
2. 选择属于其父元素的第二个子元素的每个p元素。
p:nth-child(2)
3. 选择属于其父元素的首个p元素的每个p元素。
p:first-of-type
```
## 伪类元素
```
:before  在每个元素的内容之前插入内容。
:after   在每个元素的内容之后插入内容。
style{content:"";}
```
## 商品列表
不管怎么伸缩   左边的图片宽高不变 - 右边的按钮也不变 只有中间收缩
```
//父元素
display: -webkit-box;
align-items: center;
-webkit-box-align: center;
//子元素  中间
-webkit-flex: 1;
-webkit-box-flex: 1;
flex: 1;
//子元素  左右
正常设置rem宽高   不要使用百分比做单位
```
> 例：商品列表
***
![Mou icon](toc/images/liebiao.png)

## 多个li点击的时候判断是第几个

```
window.onload=function(){
    var getLi=pUl.getElementsByTagName("li");
    for(i=0;i<getLi.length;i++)
    {
        getLi[i].index=i      //就是这行是重点了，分别赋予每个li的index值。
        getLi[i].onclick= function ()
        {
            alert(this.index);
        }
    }
}
```
## 跑马灯

```
@keyframes fly01{
    0%{transform:translateY(10px);}
    100%{transform:translateY(-290px);}
}

.dome{width: 150px;height: 20px;position: absolute;overflow: hidden;background: rgba(0, 0, 0, .4);border-radius: 10px;z-index: 999;left: 1rem;top: 5rem;}
.dome ul{
    position: absolute;left:0;top:0;margin:0;padding:0; -webkit-animation:13s fly01 infinite linear;
}
.dome ul li{height: 20px;color: #fff;margin-bottom:10px;display: flex;align-items: center;padding-left: 3px;}
.dome ul span{font-size: 0.8rem;margin-left: 5px}
.dome li img{height: 18px;width: 18px;border-radius: 50%;}
.dome .icon-close{position: absolute;right: 5px;color: #fff;line-height: 20px;}

<div class="dome">
    <ul>
        <li><img src="/public/img_goods/20180504/img_1525415752.png" alt=""><span>佣金11</span><span>一分钟前</span></li>
        <li><img src="/public/img_goods/20180508/img_1525768224.png" alt=""><span>佣金22</span><span>二分钟前</span></li>
        <li><img src="/public/img_goods/20180508/img_1525767441.png" alt=""><span>佣金33</span><span>三分钟前</span></li>
        <li><img src="/public/img_goods/20180508/img_1525768868.png" alt=""><span>佣金44</span><span>四分钟前</span></li>
        <li><img src="/public/img_index_type//20171120/141427_6681.jpg" alt=""><span>佣金55</span><span>五分钟前</span></li>
        <li><img src="/public//img_supplier/20180507/sid59_1525671775.jpg" alt=""><span>佣金66</span><span>六分钟前</span></li>
        <li><img src="/public//img_supplier/20180506/sid62_1525580024.jpg" alt=""><span>佣金77</span><span>七分钟前</span></li>
        <li><img src="/public//img_supplier/20180331/sid42_1522481508.jpg" alt=""><span>佣金88</span><span>八分钟前</span></li>
        <li><img src="/public//img_supplier/20180507/sid49_1525676043.jpg" alt=""><span>佣金99</span><span>九分钟前</span></li>
        <li><img src="/public//img_supplier/20180517/sid75_1526542717.png" alt=""><span>佣金100</span><span>十分钟前</span></li>
    </ul>
    <i class="iconfont icon-close"></i>
</div>
```


## 字符串转换后需另一个变量接收

```
//img_date = 1;
 var s = parseInt(img_date);
 console.log(s += 1);
```
## 时间格式互转

### 1. 将时间戳转换成日期格式：
```
function timestampToTime(timestamp) {
        var date = new Date(timestamp * 1000);//时间戳为10位需*1000，时间戳为13位的话不需乘1000
        Y = date.getFullYear() + '-';
        M = (date.getMonth()+1 < 10 ? '0'+(date.getMonth()+1) : date.getMonth()+1) + '-';
        D = date.getDate() + ' ';
        h = date.getHours() + ':';
        m = date.getMinutes() + ':';
        s = date.getSeconds();
        return Y+M+D+h+m+s;
    }
    timestampToTime(1403058804);
    console.log(timestampToTime(1403058804));//2014-06-18 10:33:24
```

### 2. 将日期格式转换成时间戳：
```
var date = new Date('2014-04-23 18:55:49:123');
// 有三种方式获取
var time1 = date.getTime();
var time2 = date.valueOf();
var time3 = Date.parse(date);
console.log(time1);//1398250549123
console.log(time2);//1398250549123
console.log(time3);//1398250549000
```

## 方法二：substr直接按字符串截取
```
var num1 = 55.3785 + ""; 
console.log("substr方式保留两位小数：");
console.log(num1.substr(0,num1.indexOf(".")+3));
这种方式没有四舍五入的功能，直接按位截取的，也没有补位功能
```

## Css三角形

```
  width: 0;
  height: 0;
  border-bottom: 100px solid red;
  border-left: 100px solid transparent;
```

## 值计算

```
width: calc(100% - 2rem);
```

## 数组

```
concat()  连接两个或更多的数组，并返回结果。
join()  把数组的所有元素放入一个字符串。元素通过指定的分隔符进行分隔。
pop() 删除并返回数组的最后一个元素
push()  向数组的末尾添加一个或更多元素，并返回新的长度。
reverse() 颠倒数组中元素的顺序。
shift() 删除并返回数组的第一个元素
slice() 从某个已有的数组返回选定的元素
sort()  对数组的元素进行排序
splice()  删除元素，并向数组添加新元素。
toSource()  返回该对象的源代码。
toString()  把数组转换为字符串，并返回结果。
toLocaleString()  把数组转换为本地数组，并返回结果。
unshift() 向数组的开头添加一个或更多元素，并返回新的长度。
valueOf() 返回数组对象的原始值
```

## display: flex
### 父容器
1. 设置子容器沿主轴排列：justify-content:
  * 起始端对齐 justify-content:flex-start;
  * 末尾段对齐 justify-content:flex-end;
  * 居中对齐 justify-content:center;
  * 等边距均匀分布 justify-content:space-around;
  * 等间距均匀分布 justify-content:space-between;
2. 设置子容器如何沿交叉轴排列：align-items:
  * 顶部对齐 align-items:flex-start;
  * 底部对其 align-items:flex-end;
  * 居中对齐 align-items:center;
  * 拉伸子容器与父元素一致 align-items:center;

### 子容器

## animation

1. 设置动画
  1. 可添加多个属性

```
@keyframes myfirst{
  0%   {background: red; left:0px; top:0px;}
  25%  {background:yellow;}
  50%  {background:blue;}
  100% {background:green;}
}
//@-moz-keyframes myfirst /* Firefox */
//@-webkit-keyframes myfirst /* Safari 和 Chrome */
//@-o-keyframes myfirst /* Opera */
```
2. 调用动画
```
animation:myfirst 5s;
```
3. 播放设置
  * animation-iteration-count: n|infinite;播放次数 | 无限次播放
  * animation-timing-function:cubic-bezier(0,0,0.58,1);播放速度曲线
  * animation-duration:2s;设置动画在两秒内完成:
  * animation-delay:2s;等待两秒，然后开始动画：
4. 播放的顺序
  1. animation-direction 属性定义是否循环交替反向播放动画。
  2. 注意：如果动画被设置为只播放一次，该属性将不起作用。
  3. alternate  动画在奇数次（1、3、5...）正向播放，在偶数次（2、4、6...）反向播放。
  4. alternate-reverse  动画在奇数次（1、3、5...）反向播放，在偶数次（2、4、6...）正向播放。
  5. reverse  动画反向播放。
  6. normal 默认值。动画按正常播放。
  7. inherit  从父元素继承属性。
  8. initial  设置属性为其默认值。
  9. 语法：animation-direction: normal|reverse|alternate|alternate-reverse|initial|inherit;
5. 暂停或者继续播放
  1. animation-play-state:paused 指定暂停动画
  2. animation-play-state:running  指定正在运行的动画

## transform

1. transform: none|transform-functions;
  * transition: 0.3s;过滤时间
  * translate3d(x,y,z)  定义 3D 转换。
  * translate(x,y)  定义 2D 转换。
  * translateX(x) 定义转换，只是用 X 轴的值。
  * translateY(y) 定义转换，只是用 Y 轴的值。
  * transform: translate(45px, 1em);

## 左右切换按钮
1. css
```
.switch-slide-label {
            display: block;
            width: 34px;
            height: 18px;
            background: #ccc;
            border-radius: 30px;
            cursor: pointer;
            position: relative;
            -webkit-transition: 0.3s ease;
            transition: 0.3s ease;
        }
        
        .switch-slide-label:after {
            content: '';
            display: block;
            width: 16px;
            height: 16px;
            border-radius: 100%;
            background: #fff;
            box-shadow: 0 1px 1px rgba(0, 0, 0, .1);
            position: absolute;
            left: 1px;
            top: 1px;
            -webkit-transform: translateZ(0);
            transform: translateZ(0);
            -webkit-transition:0.3s ease;
            transition:0.3s ease;
        }
        
        .switch-slide input:checked+label {
            background: #34bf49;
            transition: 0.3s ease;
        }
        .switch-slide input:checked+label:after {
            left: 17px;
        }
```
2. html


```
<label class="switch-slide">
    <input type="checkbox" id="menu-right" hidden>
    <label for="menu-right" class="switch-slide-label"></label>
</label>

```

## 定时器
```
重复: setInterval
不重复: setTimeout
clearTimeout(对象) 清除已设置的setTimeout对象 
clearInterval(对象) 清除已设置的setInterval对象 
```
## 小提示JS版本
```
//创建dom
        function bubble( node , time ){
         time=time||2000;
         var bubble = document.getElementsByClassName('bubble')[0];
         if(!bubble){
          var div = document.createElement('div');
             div.className = 'bubble';
             div.innerText = node ;
             div.style.cssText = 'padding: 1rem 12px;transform: translate(-50%, -50%);background: #afafaf;position: fixed;left: 50%;top: 200px;color: #ff0019;border-radius: 10px;'
             document.body.appendChild(div);
             setTimeout( function(){
              var bubble = document.getElementsByClassName('bubble')[0];
                 for(var i=200; i<700;i++){
                     setTimeout(function(){
                             var top = bubble.style.top ? bubble.style.top : 0;
                             top = parseInt(top) + 1;
                             bubble.style.top = top + "px";
                     },i);
                 }
               setTimeout( function(){
              document.body.removeChild(bubble)
               }, 1000);
             }, time);
         }   
        }
        bubble('请检查输入框是否填写',1000);
```
# 正则表达式

## 测试网址: https://regex101.com

## 你脑子里要想着 /^what❓$/.test(user) 的结构

```
var mas = 'abc123'
mas.match() 可以匹配出来字符，并且返回值是一个数组
mas.test()  的返回值是bool类型
mas.replace() 还有第二个参数可以做字符替换
```

## 元字符

```
\d
[0-9]
+   加号表示至少匹配一次
```

> 只能是数字

```

\w和([a-zA-Z]|\d)相同表示字母和数字的组合,用()括起来，形成了一个正则组
x{1}表示匹配前面的字符1次
x{1,3}表示匹配符合x正则的符合最少1次，最多3次

如果使用\d，就只能一个个匹配出来[1,2,3]，这需要在match方法中使用，在test方法中，必须匹配整个字符串是否符合正则
加号表示至少匹配一次数字,匹配的是整个字符串中的数字

var user = '123' //可以把123改成任意字符来测试。
if (/^\d+$/g.test(user)) {} //写法1 if(true){}
if (/^[0-9]+$/g.test(user)) {} //写法2 if(true){}


```
```
if(/^[a-zA-Z]{1}\w{5,9}$/g.test('Hyy123')){} //true
^：开头
[a-zA-Z]{1}：第一个字符匹配一次，且只能是字母
\w{5,9}：后面的字符是字母或者数字的组合，且长度是6-10，因为第一个字符占了一个长度，所以这里匹配的是5-9的长度
$：结束
```

# 面向对象开发
```
var each = function( arr , fn ){
        for( var i = 0; i < arr.length; i++){
            if( fn.call( arr[i] , i , arr[i] ) === false ){
                break;
            }
        }
    };
var getTag = function( tag , results ){
    results = results || [];
    results.push.apply( results, document.getElementsByTagName( tag ));
    return results;
};
var getId = function( id , results ){
    results = results || [];
    results.push( document.getElementById( id ));
    return results;
};
var getClass = function( classname , results ){
    results = results || [];
    results.push.apply( results, document.getElementsByClassName( classname ));
    return results;
};
var get = function ( selector , results ){
    results = results || [];
    //正则表达式，   匹配     id    class     标签      *
    var rquickExpr = /^(?:#([\w-]+)|\.([\w-]+)|([\w]+)|(\*))$/,
        m = rquickExpr.exec( selector );
    if( m ){
        if( m[1] ){
            results = getId( m[1] , results );
        }else if( m[2] ){
            results = getClass( m[2] , results );
        }else if( m[3] ){
            results = getTag( m[3] , results );
        }else if( m[4] ){
            results = getTag( m[4] , results );
        }
        //3、4可简化
        //else{
        //  results = getClass( m[3] || "*" , results ); 
        //}
    }
    return results;
}

each( get("div") , function(){
    this.style.background = 'red';
    this.style.width = 100+'px';
    this.style.height = 100+'px';
});
function DivTag(){
    this.Dom = document.createElement('div');
    this.appendTo = function( node ){
        node.appendChild( this.Dom );
        return this;
    }
    this.css = function( opent){
        for(var k in opent ){
            this.Dom.style[k] = opent[k];
        }
        return this;
    }
}
var DivTag = new DivTag();
DivTag.css({'border':'1px solid red','padding':'5px'}).appendTo( document.body );
```



# i5ting_toc
## 编译 i5ting_toc -f notes.mdown -o
1. 如果一段文字被定义为标题，只要在这段文字前加 # 号即可

```

`#` 一级标题

`##` 二级标题

`###` 三级标题

```

## 列表的显示只需要在文字前加上 - 或 * 即可变为无序列表，有序列表则直接在文字前加 1. 2. 3. 符号要和文字之间加上一个字符的空格

```

* 无线列表
* 无线列表
1. 有序列表
2. 有序列表

```

## 表格

|Tables| Are | Cool  |
| col 3 is | right-aligned| $1600 |

| col 2 is | centered|$12|

| zebra stripes | are neat|$1|

## 分割线

分割线的语法只需要另起一行，连续输入三个星号 `***` 即可。

## 引用

如果你需要引用一小段别处的句子，那么就要用引用的格式。

> 例如这样

只需要在文本前加入 > 这种尖括号（大于号）即可

## 图片——链接

[Baidu](http://baidu.com)
![Mou icon](http://mouapp.com/Mou_128.png)
`[Baidu](http://baidu.com)`
`![Mou icon](http://mouapp.com/Mou_128.png)`


# canvas
## 简单的线条
```
var canvas = document.getElementById("canvas");
var context = canvas.getContext("2d");

context.beginPath();//设置对象起始点和终点
//设置对象起始点和终点
context.moveTo(10,10);
context.lineTo(30,10);lineTo()的起始点是以上一个点为准
context.lineTo(30,30);
context.lineWidth = 1;  
context.strokeStyle = "#F5270B";

context.stroke();//绘制

context.beginPath();//设置对象起始点和终点
context.moveTo(30,30);//这里的moveTo换成lineTo效果是一样的
context.lineTo(10,30);
context.lineTo(10,10);
//strokeStyle的颜色有新的值，则覆盖上面设置的值
//lineWidth没有新的值，则按上面设置的值显示
context.strokeStyle = "#0D25F6";

context.stroke();//绘制

```
## 线条属性
```
lineCap 属性设置或返回线条末端线帽的样式，可以取以下几个值： 
“butt” 向线条的每个末端添加平直的边缘（默认）； 
“round” 向线条的每个末端添加圆形线帽； 
“square” 向线条的每个末端添加正方形线帽。

lineJoin 属性当两条线交汇时设置或返回所创建边角的类型，可以取以下几个值： 
“miter” 创建尖角(默认)； 
“bevel” 创建斜角； 
“round” 创建圆角。

miterLimit 属性设置或返回最大斜接长度（默认为10）。斜接长度指的是在两条线交汇处内角和外角之间的距离。只有当 lineJoin 属性为 “miter” 时，miterLimit 才有效。
```
## 绘制矩形
```
context.rect( x , y , width , height )：只定义矩形的路径；
context.fillRect( x , y , width , height )：直接绘制出填充的矩形；
context.strokeRect( x , y , width , height )：直接绘制出矩形边框；

//使用rect方法
context.rect(10,10,190,190);
context.lineWidth = 2;
context.fillStyle = "#3EE4CB";
context.strokeStyle = "#F5270B";
context.fill();
context.stroke();

//使用fillRect方法
context.fillStyle = "#1424DE";
context.fillRect(210,10,190,190);

//使用strokeRect方法
context.strokeStyle = "#F5270B";
context.strokeRect(410,10,190,190);

//同时使用strokeRect方法和fillRect方法
context.fillStyle = "#1424DE";
context.strokeStyle = "#F5270B";
context.strokeRect(610,10,190,190);
context.fillRect(610,10,190,190);
```
## 清空画布
```
清除矩形区域：context.clearRect(x,y,width,height)
```
## 绘制五角星案例
```
var canvas = document.getElementById("canvas");
var context = canvas.getContext("2d");
context.beginPath();
//设置是个顶点的坐标，根据顶点制定路径
for (var i = 0; i < 5; i++) {
    context.lineTo(Math.cos((18+i*72)/180*Math.PI)*200+200,
                    -Math.sin((18+i*72)/180*Math.PI)*200+200);
    context.lineTo(Math.cos((54+i*72)/180*Math.PI)*80+200,
                    -Math.sin((54+i*72)/180*Math.PI)*80+200);
}
context.closePath();
//设置边框样式以及填充颜色
context.lineWidth="3";
context.fillStyle = "#F6F152";
context.strokeStyle = "#F5270B";
context.fill();
context.stroke();
```
## 填充
```
线性渐变 
使用步骤 
（1）var grd = context.createLinearGradient( xstart , ystart, xend , yend )创建一个线性渐变，设置起始坐标和终点坐标； 
（2）grd.addColorStop( stop , color )为线性渐变添加颜色，stop为0~1的值； 
（3）context.fillStyle=grd将赋值给context。

径向渐变 
该方法与线性渐变使用方法类似，只是第一步接收的参数不一样 
var grd = context.createRadialGradient(x0 , y0, r0 , x1 , y1 , r1 );接收起始圆心的坐标和圆半径以及终点圆心的坐标和圆的半径。

位图填充 
createPattern( img , repeat-style )使用图片填充，repeat-style可以取repeat、repeat-x、repeat-y、no-repeat。

var canvas = document.getElementById("canvas");
var context = canvas.getContext("2d");

//线性渐变
var grd = context.createLinearGradient( 10 , 10, 100 , 350 );
grd.addColorStop(0,"#1EF9F7");
grd.addColorStop(0.25,"#FC0F31");
grd.addColorStop(0.5,"#ECF811");
grd.addColorStop(0.75,"#2F0AF1");
grd.addColorStop(1,"#160303");
context.fillStyle = grd;
context.fillRect(10,10,100,350);

//径向渐变
var grd = context.createRadialGradient(325 , 200, 0 , 325 , 200 , 200 );
grd.addColorStop(0,"#1EF9F7");
grd.addColorStop(0.25,"#FC0F31");
grd.addColorStop(0.5,"#ECF811");
grd.addColorStop(0.75,"#2F0AF1");
grd.addColorStop(1,"#160303");
context.fillStyle = grd;
context.fillRect(150,10,350,350);

//位图填充
var bgimg = new Image();
bgimg.src = "background.jpg";
bgimg.onload=function(){
    var pattern = context.createPattern(bgimg, "repeat");
    context.fillStyle = pattern;
    context.strokeStyle="#F20B0B";
    context.fillRect(600, 100, 200,200);
    context.strokeRect(600, 100, 200,200);
};

```
## 图形变换
```
平移：context.translate(x,y)，接收参数分别为原点在x轴方向平移x，在y轴方向平移y。

缩放：context.scale(x,y)，接收参数分别为x坐标轴按x比例缩放，y坐标轴按y比例缩放。

旋转：context.rotate(angle)，接收参数是坐标轴旋转的角度。

需要说明的是，对图形进行变化后，接下来的一次绘图是紧接着上一次的状态的，所以如果需要回到初始状态，要用到context.save();和context.restore();来保存和恢复当前状态:

var canvas = document.getElementById("canvas");
var context = canvas.getContext("2d");

//translate()
context.save();
context.fillStyle = "#1424DE";
context.translate(10,10);
context.fillRect(0,0,200,200);
context.restore();

//scale()
context.save();
context.fillStyle = "#F5270B";
context.scale(0.5,0.5);
context.fillRect(500,50,200,200);
context.restore();
//rotate()

context.save();
context.fillStyle = "#18EB0F";
context.rotate(Math.PI / 4);
context.fillRect(300,10,200,200);
context.restore();

另外一个跟图形变换相关的是：矩阵变换 ：context.transform(a, b, c, d, e, f, g)。参数的含义如下： 
a 水平缩放 ( 默认为1 ) 
b 水平倾斜 ( 默认为 0 ) 
c 垂直倾斜 ( 默认为 0 ) 
d 垂直缩放 ( 默认为1 ) 
e 水平位移 ( 默认为 0 ) 
f 垂直位移 ( 默认为 0 ) 
```
## 绘制曲线
```
context.arc(x,y,r,sAngle,eAngle,counterclockwise);用于创建弧/曲线（用于创建圆或部分圆）。接收的参数含义： 
| 参数 | 含义 | 
| :————- |:————-| 
| x | 圆的中心的 x 坐标 | 
|y|圆的中心的 y 坐标| 
|r|圆的半径| 
|sAngle|起始角，以弧度计（弧的圆形的三点钟位置是 0 度）| 
|eAngle|结束角，以弧度计| 
|counterclockwise|可选。规定应该逆时针还是顺时针绘图。False = 顺时针，true = 逆时针| 

    var canvas = document.getElementById("canvas");
    var context = canvas.getContext("2d");

    context.strokeStyle = "#F22D0D";
    context.lineWidth = "2";
    //绘制圆
    context.beginPath();
    context.arc(100,100,40,0,2*Math.PI);
    context.stroke();

    //绘制半圆
    context.beginPath();
    context.arc(200,100,40,0,Math.PI);
    context.stroke();

    //绘制半圆,逆时针
    context.beginPath();
    context.arc(300,100,40,0,Math.PI,true);
    context.stroke();

    //绘制封闭半圆
    context.beginPath();
    context.arc(400,100,40,0,Math.PI);
    context.closePath();
    context.stroke();




```
## 介于两个切线之间的弧
```

context.arcTo(x1,y1,x2,y2,r); 在画布上创建介于两个切线之间的弧/曲线

x1  弧的控制点的 x 坐标
y1  弧的控制点的 y 坐标
x2  弧的终点的 x 坐标
y2  弧的终点的 y 坐标
r 弧的半径

这里需要注意的是arcTo函数绘制的曲线的起始点需要通过moveTo(）函数来设置，下面利用arcTo函数绘制一个圆角矩形：

function createRoundRect(context , x1 , y1 , width , height , radius)
    {
        // 移动到左上角
        context.moveTo(x1 + radius , y1);
        // 添加一条连接到右上角的线段
        context.lineTo(x1 + width - radius, y1);
        // 添加一段圆弧
        context.arcTo(x1 + width , y1, x1 + width, y1 + radius, radius);
        // 添加一条连接到右下角的线段
        context.lineTo(x1 + width, y1 + height - radius);
        // 添加一段圆弧
        context.arcTo(x1 + width, y1 + height , x1 + width - radius, y1 + height , radius);
        // 添加一条连接到左下角的线段
        context.lineTo(x1 + radius, y1 + height); 
        // 添加一段圆弧
        context.arcTo(x1, y1 + height , x1 , y1 + height - radius , radius);
        // 添加一条连接到左上角的线段
        context.lineTo(x1 , y1 + radius);
        // 添加一段圆弧
        context.arcTo(x1 , y1 , x1 + radius , y1 , radius);
        context.closePath();
    }
    // 获取canvas元素对应的DOM对象
    var canvas = document.getElementById('mc');
    // 获取在canvas上绘图的CanvasRenderingContext2D对象
    var context = canvas.getContext('2d');
    context.lineWidth = 3;
    context.strokeStyle = "#F9230B";
    createRoundRect(context , 30 , 30 , 400 , 200 , 50);
    context.stroke();
```
## 二次贝塞曲线
```
context.quadraticCurveTo(cpx,cpy,x,y);
cpx 贝塞尔控制点的 x 坐标
cpy 贝塞尔控制点的 y 坐标
x 结束点的 x 坐标
y 结束点的 y 坐标

曲线的开始点是当前路径中最后一个点。如果路径不存在，那么请使用 beginPath() 和 moveTo() 方法来定义开始点。



```
## 三次贝塞尔曲线
```
cp1x  第一个贝塞尔控制点的 x 坐标
cp1y  第一个贝塞尔控制点的 y 坐标
cp2x  第二个贝塞尔控制点的 x 坐标
cp2y  第二个贝塞尔控制点的 y 坐标
x 结束点的 x 坐标
y 结束点的 y 坐标
```
## 文字渲染
```
//文字样式
ctx.font="40px Arial";
//左右对齐
ctx.textAlign="start";
ctx.textAlign="end";
ctx.textAlign="left";
ctx.textAlign="center";
ctx.textAlign="right";
//上下对齐
ctx.textBaseline="top";
ctx.textBaseline="bottom";
ctx.textBaseline="middle";
ctx.textBaseline="alphabetic";
ctx.textBaseline="hanging";

fillText()  在画布上绘制”被填充的”文本
strokeText()  在画布上绘制文本（无填充）
measureText() 返回包含指定文本宽度的对象

var canvas = document.getElementById("canvas");
var context = canvas.getContext("2d");

context.font="bold 30px Arial"; //设置样式
context.strokeStyle = "#1712F4";
context.strokeText("欢迎来到我的博客！",30,100);

context.font="bold 50px Arial"; 
var grd = context.createLinearGradient( 30 , 200, 400 , 300 );//设置渐变填充样式
grd.addColorStop(0,"#1EF9F7");
grd.addColorStop(0.25,"#FC0F31");
grd.addColorStop(0.5,"#ECF811");
grd.addColorStop(0.75,"#2F0AF1");
grd.addColorStop(1,"#160303");
context.fillStyle = grd;
context.fillText("欢迎来到我的博客！",30,200);

context.save();
context.moveTo(200,280);
context.lineTo(200,420);
context.stroke();
context.font="bold 20px Arial"; 
context.fillStyle = "#F80707";
context.textAlign="left";
context.fillText("文本在指定的位置开始",200,300);
context.textAlign="center";
context.fillText("文本的中心被放置在指定的位置",200,350);
context.textAlign="right";
context.fillText("文本在指定的位置结束",200,400);
context.restore();

context.save();
context.moveTo(10,500);
context.lineTo(500,500);
context.stroke();
context.fillStyle="#F60D0D";
context.font="bold 20px Arial"; 
context.textBaseline="top";
context.fillText("指定位置在上面",10,500);
context.textBaseline="bottom";
context.fillText("指定位置在下面",150,500);
context.textBaseline="middle";
context.fillText("指定位置居中",300,500);
context.restore();


context.font="bold 40px Arial"; 
context.strokeStyle = "#16F643";
var text = "欢迎来到我的博客！";
context.strokeText("欢迎来到我的博客！",10,600);
context.strokeText("上面字符串的宽度为："+context.measureText(text).width,10,650);
```
## 阴影绘制
```
shadowColor 设置或返回用于阴影的颜色。
shadowBlur 设置或返回用于阴影的模糊级别（数值越大越模糊）。
shadowOffsetX 设置或返回阴影与形状的水平距离。
shadowOffsetY 设置或返回阴影与形状的垂直距离。

我们为之前绘制的五角星添加一下阴影
    var canvas = document.getElementById("canvas");
    var context = canvas.getContext("2d");
    context.beginPath();
    //设置是个顶点的坐标，根据顶点制定路径
    for (var i = 0; i < 5; i++) {
        context.lineTo(Math.cos((18+i*72)/180*Math.PI)*200+200,
                        -Math.sin((18+i*72)/180*Math.PI)*200+200);
        context.lineTo(Math.cos((54+i*72)/180*Math.PI)*80+200,
                        -Math.sin((54+i*72)/180*Math.PI)*80+200);
    }
    context.closePath();
    //设置边框样式以及填充颜色
    context.lineWidth="3";
    context.fillStyle = "#F6F152";
    context.strokeStyle = "#F5270B";
    context.shadowColor = "#F7F2B4";
    context.shadowOffsetX = 30;
    context.shadowOffsetY = 30;
    context.shadowBlur = 2;
    context.fill();
    context.stroke();
```
## 剪辑区域
```
clip()方法从原始画布中剪切任意形状和尺寸。 
提示：一旦剪切了某个区域，则所有之后的绘图都会被限制在被剪切的区域内（不能访问画布上的其他区域）。您也可以在使用 clip() 方法前通过使用 save() 方法对当前画布区域进行保存，并在以后的任意时间对其进行恢复（通过 restore() 方法） 

var canvas = document.getElementById("canvas");
    var context = canvas.getContext("2d");

    context.beginPath();
    context.fillStyle = "#0C0101";
    context.fillRect(0,0,canvas.width,canvas.height);

    context.beginPath();
    context.fillStyle = "#FFFDFD";
    context.arc(400,400,100,0,2*Math.PI);
    context.fill();
    context.clip();

    context.beginPath();
    context.fillStyle = "#F60825";
    context.fillRect(200, 350, 400,50);
```
## 除了上述的属性的和方法，还有以下等方法

```
drawImage()： 向画布上绘制图像、画布或视频。

toDataURL() ：保存图形

isPointInPath()： 如果指定的点位于当前路径中，则返回 true，否则返回 false。
```

# 解决问题的方法

## 移动端头部声明
```
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=0">
```

## css实现16:9的图片比例

> 边距值设置为百分比的话，是基于父元素的宽度

```
.imgBox{
  padding-bottom: 56%;
  width: 100%;
  position: relative;
}
.imgBox img{
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;  
}
```
```
<div class="imgBox"><img src="img.jpg" alt=""></div>
```

## iframe自适应高度

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <style>
        #external-frame{min-height: 400px}
    </style>
    <iframe src="CSS实现滚动显示进度.html" frameborder="0" scrolling="no" id="external-frame"></iframe>
    <script type="text/javascript">
        //参考 http://caibaojian.com/iframe-adjust-content-height.html
        //需要服务器环境
        function setIframeHeight(iframe) {
            if (iframe) {
                var iframeWin = iframe.contentWindow || iframe.contentDocument.parentWindow;
                if (iframeWin.document.body) {
                    iframe.height = iframeWin.document.documentElement.scrollHeight || iframeWin.document.body.scrollHeight;
                }
            }
        };

        window.onload = function () {
            setIframeHeight(document.getElementById('external-frame'));
        };
    </script>
</body>
</html>
```

## CSS实现滚动显示进度

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style>
			body {
				background-image: linear-gradient(to right top, #ffcc00 50%, #eee 50%);
				background-size: 100% calc(100% - 100vh + 5px);
				background-repeat: no-repeat;
			}
			body::after {
				content: "";
				position: fixed;
				top: 5px;
				left: 0;
				bottom: 0;
				right: 0;
				background: #fff;
				z-index: -1;
			}
		</style>
	</head>
	<body>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
		11111111</br>
	</body>
</html>

```

## div{ cursor:pointer } 手指形状 链接选择效果

## a链接

最常用的方法，也是最周全的方法，onclick方法负责执行js函数，而void是一个操作符，void(0)返回undefined，地址不发生跳转。

```javascript
<a href="javascript:void(0);" onclick="js_method()">6666</a>
```












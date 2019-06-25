# 严格模式

```javascript
'use strict'; // 开启严格模式
```

# switch判断

```
switch (x) {
  case 1:
    console.log('x 等于1');
    break;
  case 2:
    console.log('x 等于2');
    break;
  default:
    console.log('x 等于其他值');
}
```

# 三元运算符

```javascript
//(条件) ? 表达式1 : 表达式2
var bolu = ( a === b) ? 'ok':'no'
		//当a等于b的时候返回ok，不等的是否返回no
```

# 循环

## while 循环

`While`语句包括一个循环条件和一段代码块，只要条件为真，就不断循环执行代码块。

```javascript
var i = 0;
while (i < 100) {
  console.log('i 当前为：' + i);
  i = i + 1;
}
```

## do...while 循环

`do...while`循环与`while`循环类似，唯一的区别就是先运行一次循环体，然后判断循环条件。

```javascript
do
  语句
while (条件);

// 或者
do {
  语句
} while (条件);
```

## break 语句和 continue 语句

如果存在多重循环，不带参数的`break`语句和`continue`语句都只针对最内层循环。

### break

```javascript
//break语句用于跳出代码块或循环。
for (var i = 0; i < 5; i++) {
  console.log(i);
  if (i === 3)
    break;
}
var i = 0;

while(i < 100) {
  console.log('i 当前为：' + i);
  i++;
  if (i === 10) break;
}
```

### continue

```javascript
//continue语句用于立即终止本轮循环，返回循环结构的头部，开始下一轮循环。
var i = 0;

while (i < 100){
  i++;
  if (i % 2 === 0) continue;
  console.log('i 当前为：' + i);
}
```

# 标签（label）

JavaScript 语言允许，语句的前面有标签（label），相当于定位符，用于跳转到程序的任意位置，标签的格式如下。

```javascript
//label:
//  语句
//标签可以是任意的标识符，但不能是保留字，语句部分可以是任意语句。
//标签通常与break语句和continue语句配合使用，跳出特定的循环。
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) break top;
      console.log('i=' + i + ', j=' + j);
    }
  }
//上面代码为一个双重循环区块，break命令后面加上了top标签（注意，top不用加引号），满足条件时，直接跳出双层循环。如果break语句后面不使用标签，则只能跳出内层循环，进入下一次的外层循环。

```

## 标签也可以用于跳出代码块。

```javascript
foo: {
  console.log(1);
  break foo;
  console.log('本行不会输出');
}
console.log(2);
// 1
// 2
```

## `continue`语句也可以与标签配合使用。

```javascript
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) continue top;
      console.log('i=' + i + ', j=' + j);
    }
  }
//上面代码中，continue命令后面有一个标签名，满足条件时，会跳过当前循环，直接进入下一轮外层循环。如果continue语句后面不使用标签，则只能进入下一轮的内层循环。
```

# typeof

JavaScript 有三种方法，可以确定一个值到底是什么类型。

* `typeof`运算符
* `instanceof`运算符
* `Object.prototype.toString`方法

```javascript
typeof 123 // "number"
typeof '123' // "string"
typeof false // "boolean"
typeof null // "object"
typeof window // "object"
typeof {} // "object"
typeof [] // "object"
//-----------------------
var o = {};
var a = [];
o instanceof Array // false
a instanceof Array // true
//-----------------------
function f() {}
typeof f
// "function"
//-----------------------
// 正确的写法
if (typeof v === "undefined") {
  // ...
}
```

# null 和 undefined

`null`与`undefined`都可以表示“没有”，含义非常相似。将一个变量赋值为`undefined`或`null`，老实说，语法效果几乎没区别。

```javascript
Number(null) // 0
5 + null // 5
Number(undefined) // NaN
5 + undefined // NaN
//----------------------
undefined == null // true

```

# 与数值相关的全局方法

```javascript
//parseInt方法用于将字符串转为整数。
//如果字符串头部有空格，空格会被自动去除。
//如果parseInt的参数不是字符串，则会先转为字符串再转换。
parseInt('123') // 123
//parseFloat方法会自动过滤字符串前导的空格。
parseFloat('\t\v\r12.34\n ') // 12.34
//isNaN方法可以用来判断一个值是否为NaN。
isNaN(NaN) // true
isNaN(123) // false
//isNaN只对数值有效，如果传入其他值，会被先转成数值。比如，传入字符串的时候，字符串会被先转成NaN，所以最后返回true，这一点要特别引起注意。也就是说，isNaN为true的值，有可能不是NaN，而是一个字符串。
//因此，使用isNaN之前，最好判断一下数据类型。
function myIsNaN(value) {
  return typeof value === 'number' && isNaN(value);
}
//判断NaN更可靠的方法是，利用NaN为唯一不等于自身的值的这个特点，进行判断。
function myIsNaN(value) {
  return value !== value;
}
//isFinite方法返回一个布尔值，表示某个值是否为正常的数值。
//除了Infinity、-Infinity、NaN和undefined这几个值会返回false，isFinite对于其他的数值都会返回true。
isFinite(Infinity) // false
isFinite(-Infinity) // false
isFinite(NaN) // false
isFinite(undefined) // false
isFinite(null) // true
isFinite(-1) // true
```

# 字符串

```javascript
'Did she say \'Hello\'?'
// "Did she say 'Hello'?"
```

## 转义

```javascript
\0 ：null（\u0000）
\b ：后退键（\u0008）
\f ：换页符（\u000C）
\n ：换行符（\u000A）
\r ：回车键（\u000D）
\t ：制表符（\u0009）
\v ：垂直制表符（\u000B）
\' ：单引号（\u0027）
\" ：双引号（\u0022）
\\ ：反斜杠（\u005C）
```

## Base64 转码 

```javascript
var string = 'Hello World!';
btoa(string) // "SGVsbG8gV29ybGQh"
atob('SGVsbG8gV29ybGQh') // "Hello World!"
//要将非 ASCII 码字符转为 Base64 编码，必须中间插入一个转码环节，再使用这两个方法。
function b64Encode(str) {
  return btoa(encodeURIComponent(str));
}

function b64Decode(str) {
  return decodeURIComponent(atob(str));
}

b64Encode('你好') // "JUU0JUJEJUEwJUU1JUE1JUJE"
b64Decode('JUU0JUJEJUEwJUU1JUE1JUJE') // "你好"
```

# 对象

```javascript
eval('{foo: 123}') // 123
eval('({foo: 123})') // {foo: 123}
```

## 属性的操作

### 属性的读取

读取对象的属性，有两种方法，一种是使用点运算符，还有一种是使用方括号运算符。

```javascript
var obj = {
  p: 'Hello World'
};

obj.p // "Hello World"
obj['p'] // "Hello World"
//数字键可以不加引号，因为会自动转成字符串。
var obj = {
  0.7: 'Hello World'
};
obj['0.7'] // "Hello World"
obj[0.7] // "Hello World"
//--------------------------------
var obj = {
  123: 'hello world'
};

obj.123 // 报错
obj[123] // "hello world"
```

### 属性的查看

```javascript
var obj = {
  key1: 1,
  key2: 2
};

Object.keys(obj);
// ['key1', 'key2']
```

### 属性的删除

```javascript
delete obj.p // true
```

### arguments 对象

由于 JavaScript 允许函数有不定数目的参数，所以需要一种机制，可以在函数体内部读取所有参数。这就是`arguments`对象的由来。

`arguments`对象包含了函数运行时的所有参数，`arguments[0]`就是第一个参数，`arguments[1]`就是第二个参数，以此类推。这个对象只有在函数体内部，才可以使用。

```javascript
//严格模式下，arguments对象是一个只读对象，修改它是无效的，但不会报错。
//通过arguments对象的length属性，可以判断函数调用时到底带几个参数。
//需要注意的是，虽然arguments很像数组，但它是一个对象。数组专有的方法（比如slice和forEach），不能在arguments对象上直接使用。

//如果要让arguments对象使用数组方法，真正的解决方法是将arguments转为真正的数组。下面是两种常用的转换方法：slice方法和逐一填入新数组。
var args = Array.prototype.slice.call(arguments);

// 或者
var args = [];
for (var i = 0; i < arguments.length; i++) {
  args.push(arguments[i]);
}
```

### in 运算符

`in`运算符用于检查对象是否包含某个属性（注意，检查的是键名，不是键值），如果包含就返回`true`，否则返回`false`。它的左边是一个字符串，表示属性名，右边是一个对象。

```javascript
//in运算符的一个问题是，它不能识别哪些属性是对象自身的，哪些属性是继承的。就像上面代码中，对象obj本身并没有toString属性，但是in运算符会返回true，因为这个属性是继承的。

//这时，可以使用对象的hasOwnProperty方法判断一下，是否为对象自身的属性。

var obj = {};
if ('toString' in obj) {
  console.log(obj.hasOwnProperty('toString')) // false
}
```

### 属性的遍历：for...in 循环

如果继承的属性是可遍历的，那么就会被`for...in`循环遍历到。但是，一般情况下，都是只想遍历对象自身的属性，所以使用`for...in`的时候，应该结合使用`hasOwnProperty`方法，在循环内部判断一下，某个属性是否为对象自身的属性。

```javascript
var person = { name: '老张' };

for (var key in person) {
  if (person.hasOwnProperty(key)) {
    console.log(key);
    console.log(person[key]);
  }
}
// name
// 老张
```

# 函数

## 递归

函数可以调用自身，这就是递归（recursion）。下面就是通过递归，计算斐波那契数列的代码。

```javascript
function fib(num) {
  if (num === 0) return 0;
  if (num === 1) return 1;
  return fib(num - 2) + fib(num - 1);
}

fib(6) // 8
```

## name

```javascript
//属性的一个用处，就是获取参数函数的名字。
var myFunc = function () {};

function test(f) {
  console.log(f.name);
}

test(myFunc) // myFunc
```

## 参数的省略

需要注意的是，函数的`length`属性与实际传入的参数个数无关，只反映函数预期传入的参数个数。

```javascript
//如果一定要省略靠前的参数，只有显式传入undefined
f(undefined, 1)
```

## eval

`eval`命令接受一个字符串作为参数，并将这个字符串当作语句执行。

`eval`最常见的场合是解析 JSON 数据的字符串，不过正确的做法应该是使用原生的`JSON.parse`方法。

```javascript
eval('var a = 1;');
a // 1
//如果参数字符串无法当作语句运行，那么就会报错。
eval('3x') // Uncaught SyntaxError: Invalid or unexpected token
//如果eval的参数不是字符串，那么会原样返回。
eval(123) // 123
```

# 数组

### 数组的本质

本质上，数组属于一种特殊的对象。`typeof`运算符会返回数组的类型是`object`。

```javascript
typeof [1, 2, 3] // "object"
//上面代码表明，typeof运算符认为数组的类型就是对象。
var arr = ['a', 'b', 'c'];
Object.keys(arr)
// ["0", "1", "2"]
//数组的特殊性体现在，它的键名是按次序排列的一组整数（0，1，2...）。
```

### 逆向循环

```javascript
var a = [1, 2, 3];
var l = a.length;
while (l--) {
  console.log(a[l]);
}
```

### forEach

```javascript
var colors = ['red', 'green', 'blue'];
colors.forEach(function (color) {
  console.log(color);
});
// red
// green
// blue
```

### slice

数组的`slice`方法可以将“类似数组的对象”变成真正的数组。

```javascript
// arguments对象
//由于 JavaScript 允许函数有不定数目的参数，所以需要一种机制，可以在函数体内部读取所有参数。这就是arguments对象的由来。
//arguments对象包含了函数运行时的所有参数，arguments[0]就是第一个参数，arguments[1]就是第二个参数，以此类推。这个对象只有在函数体内部，才可以使用。
function args() { return arguments }
var arrayLike = args('a', 'b');
var arr = Array.prototype.slice.call(arrayLike);
arr.forEach(function (chr) {
  console.log(chr);
});
```

# 运算符

## 算术运算符

JavaScript 共提供10个算术运算符，用来完成基本的算术运算。

- **加法运算符**：`x + y`
- **减法运算符**： `x - y`
- **乘法运算符**： `x * y`
- **除法运算符**：`x / y`
- **指数运算符**：`x ** y`
- **余数运算符**：`x % y`
- **自增运算符**：`++x` 或者 `x++`
- **自减运算符**：`--x` 或者 `x--`
- **数值运算符**： `+x`
- **负数值运算符**：`-x`

## 比较运算符

JavaScript 一共提供了8个比较运算符。

- `>` 大于运算符
- `<` 小于运算符
- `<=` 小于或等于运算符
- `>=` 大于或等于运算符
- `==` 相等运算符
- `===` 严格相等运算符
- `!=` 不相等运算符
- `!==` 严格不相等运算符

## 布尔运算符

布尔运算符用于将表达式转为布尔值，一共包含四个运算符。

- 取反运算符：`!`
- 且运算符：`&&`
- 或运算符：`||`
- 三元运算符：`?:`

# 数据类型的转换

## Number()

使用`Number`函数，可以将任意类型的值转化成数值。

`Number`函数将字符串转为数值，要比`parseInt`函数严格很多。基本上，只要有一个字符无法转成数值，整个字符串就会被转为`NaN`。

```javascript
// 数值：转换后还是原来的值
Number(324) // 324

// 字符串：如果可以被解析为数值，则转换为相应的数值
Number('324') // 324

// 字符串：如果不可以被解析为数值，返回 NaN
Number('324abc') // NaN

// 空字符串转为0
Number('') // 0

// 布尔值：true 转成 1，false 转成 0
Number(true) // 1
Number(false) // 0

// undefined：转成 NaN
Number(undefined) // NaN

// null：转成0
Number(null) // 0
```

## String()

`String`函数可以将任意类型的值转化成字符串，转换规则如下。

**（1）原始类型值**

- **数值**：转为相应的字符串。
- **字符串**：转换后还是原来的值。
- **布尔值**：`true`转为字符串`"true"`，`false`转为字符串`"false"`。
- **undefined**：转为字符串`"undefined"`。
- **null**：转为字符串`"null"`。

```javascript
String(123) // "123"
String('abc') // "abc"
String(true) // "true"
String(undefined) // "undefined"
String(null) // "null"
```

## Boolean()

`Boolean`函数可以将任意类型的值转为布尔值。

它的转换规则相对简单：除了以下五个值的转换结果为`false`，其他的值全部为`true`。

- `undefined`
- `null`
- `-0`或`+0`
- `NaN`
- `''`（空字符串）

```javascript
Boolean(undefined) // false
Boolean(null) // false
Boolean(0) // false
Boolean(NaN) // false
Boolean('') // false
```

# 错误处理机制

## Error 实例对象

抛出`Error`实例对象以后，整个程序就中断在发生错误的地方，不再往下执行。

```javascript
var err = new Error('出错了');
err.message // "出错了"
//message：错误提示信息
//name：错误名称（非标准属性）
//stack：错误的堆栈（非标准属性）
```

## 原生错误类型

* `SyntaxError`对象是解析代码时发生的语法错误。
* `ReferenceError`对象是引用一个不存在的变量时发生的错误。
* `RangeError`对象是一个值超出有效范围时发生的错误。主要有几种情况，一是数组长度为负数，二是`Number`对象的方法参数超出范围，以及函数堆栈超过最大值。
* `TypeError`对象是变量或参数不是预期类型时发生的错误。比如，对字符串、布尔值、数值等原始类型的值使用`new`命令，就会抛出这种错误，因为`new`命令的参数应该是一个构造函数。
* `URIError`对象是 URI 相关函数的参数不正确时抛出的错误，主要涉及`encodeURI()`、`decodeURI()`、`encodeURIComponent()`、`decodeURIComponent()`、`escape()`和`unescape()`这六个函数。
* `eval`函数没有被正确执行时，会抛出`EvalError`错误。该错误类型已经不再使用了，只是为了保证与以前代码兼容，才继续保留。

### 总结

以上这6种派生错误，连同原始的`Error`对象，都是构造函数。开发者可以使用它们，手动生成错误对象的实例。这些构造函数都接受一个参数，代表错误提示信息（message）。

```javascript
var err1 = new Error('出错了！');
var err2 = new RangeError('出错了，变量超出有效范围！');
var err3 = new TypeError('出错了，变量类型无效！');

err1.message // "出错了！"
err2.message // "出错了，变量超出有效范围！"
err3.message // "出错了，变量类型无效！"
```

## 自定义错误

除了 JavaScript 原生提供的七种错误对象，还可以定义自己的错误对象。

```javascript
function UserError(message) {
  this.message = message || '默认信息';
  this.name = 'UserError';
}

UserError.prototype = new Error();
UserError.prototype.constructor = UserError;
//上面代码自定义一个错误对象UserError，让它继承Error对象。然后，就可以生成这种自定义类型的错误了。
new UserError('这是自定义的错误！');
```

## throw 语句

```javascript
//throw语句的作用是手动中断程序执行，抛出一个错误。
if (x <= 0) {
  throw new Error('x 必须为正数');
}
//throw也可以抛出自定义错误。
function UserError(message) {
  this.message = message || '默认信息';
  this.name = 'UserError';
}

throw new UserError('出错了！');
// Uncaught UserError {message: "出错了！", name: "UserError"}
```

实际上，`throw`可以抛出任何类型的值。也就是说，它的参数可以是任何值。

```javascript
// 抛出一个字符串
throw 'Error！';
// Uncaught Error！

// 抛出一个数值
throw 42;
// Uncaught 42

// 抛出一个布尔值
throw true;
// Uncaught true

// 抛出一个对象
throw {
  toString: function () {
    return 'Error!';
  }
};
// Uncaught {toString: ƒ}
```

## try...catch

一旦发生错误，程序就中止执行了。JavaScript 提供了`try...catch`结构，允许对错误进行处理，选择是否往下执行。

```javascript
try {
  throw new Error('出错了!');
} catch (e) {
  console.log(e.name + ": " + e.message);
  console.log(e.stack);
}
// Error: 出错了!
//   at <anonymous>:3:9
//   ...
//上面代码中，try代码块抛出错误（上例用的是throw语句），JavaScript 引擎就立即把代码的执行，转到catch代码块，或者说错误被catch代码块捕获了。catch接受一个参数，表示try代码块抛出的值。
```

## try...finally 

`try...catch`结构允许在最后添加一个`finally`代码块，表示不管是否出现错误，都必需在最后运行的语句。

```javascript
function cleansUp() {
  try {
    throw new Error('出错了……');
    console.log('此行不会执行');
  } finally {
    console.log('完成清理工作');
  }
}

cleansUp()
// 完成清理工作
// Uncaught Error: 出错了……
//    at cleansUp (<anonymous>:3:11)
//    at <anonymous>:10:1
//上面代码中，由于没有catch语句块，一旦发生错误，代码就会中断执行。中断执行之前，会先执行finally代码块，然后再向用户提示报错信息。
```

## 典型场景

```javascript
//openFile();
try {
  writeFile(Data);
} catch(e) {
  handleError(e);
} finally {
  closeFile();
}
//上面代码首先打开一个文件，然后在try代码块中写入文件，如果没有发生错误，则运行finally代码块关闭文件；一旦发生错误，则先使用catch代码块处理错误，再使用finally代码块关闭文件。
```

# console.log

## console.warn()

`warn`方法和`error`方法也是在控制台输出信息，它们与`log`方法的不同之处在于，`warn`方法输出信息时，在最前面加一个黄色三角，表示警告；`error`方法输出信息时，在最前面加一个红色的叉，表示出错。同时，还会高亮显示输出文字和错误发生的堆栈。其他方面都一样。

```javascript
console.error('Error: %s (%i)', 'Server is not responding', 500)
// Error: Server is not responding (500)
console.warn('Warning! Too few nodes (%d)', document.childNodes.length)
// Warning! Too few nodes (1)
```

## console.table()

```javascript
//输出表格
var languages = [
  { name: "JavaScript", fileExtension: ".js" },
  { name: "TypeScript", fileExtension: ".ts" },
  { name: "CoffeeScript", fileExtension: ".coffee" }
];
console.table(languages);
//--------------------------
var languages = {
  csharp: { name: "C#", paradigm: "object-oriented" },
  fsharp: { name: "F#", paradigm: "functional" }
};
console.table(languages);
```

## console.dir()

```javascript
//dir方法用来对一个对象进行检查（inspect），并以易于阅读和打印的格式显示。
console.dir({f1: 'foo', f2: 'bar'})
// Object
//   f1: "foo"
//   f2: "bar"
//   __proto__: Object
```

## console.dirxml

```javascript
//dirxml方法主要用于以目录树的形式，显示 DOM 节点。
//如果参数不是 DOM 节点，而是普通的 JavaScript 对象，console.dirxml等同于console.dir。
console.dirxml(document.body)
```

## console.assert()

`console.assert`方法主要用于程序运行过程中，进行条件判断，如果不满足条件，就显示一个错误，但不会中断程序执行。这样就相当于提示用户，内部状态不正确。

```javascript
console.assert(false, '判断条件不成立')
// Assertion failed: 判断条件不成立

// 相当于
try {
  if (!false) {
    throw new Error('判断条件不成立');
  }
} catch(e) {
  console.error(e);
}
```

## console.time()，console.timeEnd()

这两个方法用于计时，可以算出一个操作所花费的准确时间。

```javascript
console.time('Array initialize');

var array= new Array(1000000);
for (var i = array.length - 1; i >= 0; i--) {
  array[i] = new Object();
};

console.timeEnd('Array initialize');
// Array initialize: 1914.481ms
```

## console.group()，console.groupEnd()，console.groupCollapsed()

`console.group`和`console.groupEnd`这两个方法用于将显示的信息分组。它只在输出大量信息时有用，分在一组的信息，可以用鼠标折叠/展开。

```javascript
console.group('一级分组');
console.log('一级分组的内容');

console.group('二级分组');
console.log('二级分组的内容');

console.groupEnd(); // 二级分组结束
console.groupEnd(); // 一级分组结束
```

`console.groupCollapsed`方法与`console.group`方法很类似，唯一的区别是该组的内容，在第一次显示时是收起的（collapsed），而不是展开的。

```javascript
console.groupCollapsed('Fetching Data');

console.log('Request Sent');
console.error('Error: Server not responding (500)');

console.groupEnd();
```

## console.trace()，console.clear()

`console.trace`方法显示当前执行的代码在堆栈中的调用路径。

`console.clear`方法用于清除当前控制台的所有输出，将光标回置到第一行。如果用户选中了控制台的“Preserve log”选项，`console.clear`方法将不起作用。



# 正则表达式

```javascript
var regex = /表达式/匹配模式; //效率较高
var regex = new RegExp('表达式'，'匹配模式');
//两种创建方式等价
a|b === [ab]   //或者的关系
/[a-z]/        //任意小写字母
/[A-Z]/        //任意大写字母
/[A-z]/        //任意字母
/[^abc]/       //查找任何不在方括号之间的字符
/[0-9]/        //任意数字
/ab{n}/        //b正好连续出现n次，只对大括号前面第一个字符起作用
/(ab){n,nn}c/  //ab正好连续出现n~nn次
/(ab){n,}c/    //ab正好连续出现n次以上
/ab+c/         //至少要有一个或着多个b
/ab*c/         //零个或着多个b
/ab?c/         //零个或着一个b
/^a/           //表示以a开头
/a$/           //表示以a结尾
/\./           //反斜杠表示转义字符   .在正则表示任意字符 
/\d/ //匹配0-9之间的任一数字，相当于[0-9]。
/\D/ //匹配所有0-9以外的字符，相当于[^0-9]。
/\w/ //匹配任意的字母、数字和下划线，相当于[A-Za-z0-9_]。
/\W/ //除所有字母、数字和下划线以外的字符，相当于[^A-Za-z0-9_]。
/\s/ //匹配空格（包括换行符、制表符、空格符等），相等于[ \t\r\n\v\f]。
/\S/ //匹配非空格的字符，相当于[^ \t\r\n\v\f]。
/\b/ //匹配词的边界。
/\B/ //匹配非词边界，即在词的内部。
/^\s*|\s*$/g  //匹配开头和结尾的空格
-------------------------------------------- 
//可以为一个正则表达式设置多个匹配模式，书写顺序忽略
RegExp.prototype.ignoreCase：返回一个布尔值，表示是否设置了i修饰符。
RegExp.prototype.global：返回一个布尔值，表示是否设置了g修饰符。
RegExp.prototype.multiline：返回一个布尔值，表示是否设置了m修饰符。
RegExp.prototype.flags：返回一个字符串，包含了已经设置的所有修饰符，按字母排序。
--------------------------------------------
//字符串的实例方法之中，有4种与正则表达式有关。
String.prototype.match()：返回一个数组，成员是所有匹配的子字符串。
String.prototype.search()：按照给定的正则表达式进行搜索，返回一个整数，表示匹配开始的位置。
String.prototype.replace()：按照给定的正则表达式进行替换，返回替换后的字符串。
String.prototype.split()：按照给定规则进行字符串分割，返回一个数组，包含分割后的各个成员。
```



# 遍历数组forEach

```
//  
var date = ['a','b','c'];
date.forEach(function( item , index ){
	console.log(item);//元素
	console.log(index);//下标
})
```
# 遍历对象in

```
var date = [
	{name:'zs',age:1},
	{name:'ls',age:2},
	{name:'ww',age:3}
];
date.forEach(function(itme){
	 //第一次循环创建行
		var a = document.createElement('p');
		a.style.cssText = 'width:300px;'
		box.appendChild(a);
	 //第二次循环创建列
	for(var key in itme){
		var b = document.createElement('span');
		b.style.cssText = 'width:98px;display: inline-block;text-align: center;border:1px solid #000;'
		b.innerHTML = itme[key];
		a.appendChild(b);
	}
	 //追加列
	var c = document.createElement('span');
		c.style.cssText = 'width:98px;display: inline-block;text-align: center;border:1px solid #000;'
		c.innerHTML = '删除';
		a.appendChild(c);
})
```
# 代码所消耗的时间
```
console.time('flag');
//中间放对应的执行代码
console.timeEnd('flag');
```
# 影响性能的因素
## 因素 
1. 字符串拼接 换成 数组
2. DOM树渲染非常耗时间 innerHTML渲染尽可能减少
3. 百度查询

## 删除元素注意事项
1. 建议使用 removeChild(nonde);
2. 使用 innerHTML 不会把元素身上对应的事件删掉，会造成内存泄漏

## 移动元素用 appendChld(node);
1. 找到对应的元素，然后直接添加到对应的位置即可

## 案例

### 案例1：innerHTML

1. 用字符串拼接多次渲染dom树

```
var box = document.getElementById('box'),
	data = ['a','b','c'];
data.forEach(function( item , index ){
	//元素item,下标index
	box.innerHTML += '<p>'+ item +'</p>';
})
```

2. 优化用数组拼接再一次渲染

```
var box = document.getElementById('box'),
	data = ['a','b','c'],
	arr = [];
data.forEach(function( item , index ){
	//元素item,下标index
	arr.push('<p>'+ item +'</p>');
})
box.innerHTML = arr.join('');//用空字符串拼接
```

### 案例2：提高性能
1.  要给循环出来的对象添加点击事件，应该把函数放到循环外
```
var link = document.getElementsByClass('box');
for(var i = 0 , a = link.length; i < s ; i++ ){
	link.onclick = linkclick;
}
function linkclick(){
	//函数
}
```

#  事件捕获／目标阶段／冒泡／事件委托／事件对象
## 案例：事件冒泡、捕获


```
var div1 = document.getElementById('div1');
var attr = [div1,document.body,document];
attr.forEach(function(item){
	item.addEventListener('click',function(){
	 console.log(this);
	},true);//true（捕获）、false（冒泡）
})

```
## 案例：事件委托

```
//结构 : ul#ul > li*5 
//点击li改变背景 ，不给li注册事件，通过事件委托来解决问题
var ul = document.getElementById('ul');
ul.onclick = function(e){
	//当事件发生的时候，系统会把事件发生时的一些数据传递给事件处理函数
	//this指向ul
	//e 事件对象（事件参数）
	//e.target 真正触发事件的参数对象 li
	e.target.style.background = 'red';
}

```
## 案例：事件对象
```
var box = document.getElementById('box');
box.onclick = fn;
box.onmouseover = fn;
box.onmouseout = fn;
function fn(e){
	//e有兼容性问题 ie9以前不支持
	//处理兼容性问题
	e = e || window.event;
	switch(e.type){
		case 'click' :
		 console.log('点击事件');
		break;
		case 'mouseover' :
		 console.log('鼠标经过');
		break;
		case 'mouseout' :
		 console.log('鼠标离开');
		break;
	}
}
//获取可是区域中的位置
//不能有滚动条
//console.log(e.clientX+'px');
//console.log(e.clientY+'px');
//获取鼠标在页面中的位置
//console.log(e. pageX+'px');
//console.log(e. pageY+'px');
```
## 案例：获取页面滚动出去的距离，兼容性问题处理
```
//ie 和 chrome
var box = document.getElementById('box');
document.onmousemove = function(e){
	box.style.left = getPage(e).pageX+"px";
	box.style.top = getPage(e).pageY+"px";
}
function getScroll(){
	return{
		scrollTop:document.documentElement.scrollTop || document.body.scrollTop,
		scrollLeft:document.documentElement.scrollLeft || document.body.scrollLeft
	}
}
function getPage(e){
	return{
		pageX:e.clientX + getScroll().scrollLeft,
		pageY:e.clientY + getScroll().scrollTop,
	}
}
```
## 案例：阻止事件冒泡
```javascript
var attr = [box,document.body,document];
attr.forEach(function(item){
	item.addEventListener('click',function(e){
	 console.log(this);
	 e.stopPropagation();
	},false);//true（捕获）、false（冒泡）
})
```
## 案例：阻止默认行为
```
//a#btn
btn.onclick = function(e){
	//return false;
	e.preventDefault();
}

```
# 接收函数返回值 ＋ 三元表达式
```
function a(b,c){
	var d,e,f;
	d = b + c;
	e = c - b;
	f = b * c;
	return{
		d:d,e:e,f:f
	}
}
//接收函数表达式的返回值
var obj = a(5,6);
console.log(obj.d+'-'+obj.e+'-'+obj.f);
//三元表达式
obj.d = obj.d < 10 ? '0'+obj.d : obj.d;
//如果obj小于0就返回'0'+obj.d否者返回obj.d
console.log(obj.d);
```

# 获取div的几个边的距离获取
## scroll
```
scrollTop = 滚动出去的距离；
scrollWidth = 算上隐藏的内容的大小；
scrollHeight ＝ 算上隐藏的内容的大小；
scrollLeft = 算上边框加上隐藏的内容的大小；
```
## offsetParent
```
offsetTop = 不算边框，到父容器的距离
offsetLeft = 不算边框，到父容器的距离
offsetWidth = 算边框加上内容
offsetHeight = 算边框加上内容
```
## client
```
clientWidth =  不算边框
clientHeight =  不算边框
clientLeft = 边框的宽度
clientTop = 边框的宽度
```
# 保证一个页面只有一个定时器,加上回调函数
```
btn.onclick = function(){
	var box = document.getElementById('box');
	fn(box,function(){
		console.log('执行完毕');
	});
}
function fn(elenemt,fn){
	if(elenemt.timer){
		clearInterval(elenemt.timer);
	}
	var cdr = 0;
	elenemt.timer = setInterval(function(){
		cdr++;
		if(cdr === 100){
			clearInterval(elenemt.timer);
			//回调函数 
			if(fn){
				fn();
			}
		}
		elenemt.style.left = cdr + 'px';
	},30)
}
```
#  构造函数

## 创建一个构造函数
```
function Person(name,age){
	//实例成员
	this.name = name;
	this.age = age;
}
var p1 = new Person;
```
## 原型对象有一个属是constructor
```
console.log(Person.constructor);
//指的是Person构造器的本身 
```
## prototype
```
Person.prototype.sayHi = function(){
	console.log(this.name);
}
var p1 = new Person('ls',18);
p1.sayHi();
Person.prototype.type = 'huang';
console.log(p1.type);
```
## 设置原型对象
```
//当重新设置构造函数prototypr的时候，一定要重新设置constructor，并且值是构造器
Person.prototype = {
	constructor:Person,
	sayHi : function(){
		console.log(this.name);
	}
}
var p1 = new Person('ls',18);
console.log(p1.constructor);
p1.sayHi();
```
##  扩展内置对象
```
//给所有的数组新增一个方法
var a = [1,2,3];
Array.prototype.getSum = function(){
	var sum = 0;
	for(var i = 0 ; i < this.length; i++){
		sum += this[i];
	}
	return sum;
}
console.log(a.getSum());

```
# 使用构造函数做案例
## 在页面随机生成小方块
```
//构造函数  box
//属性 : 
//		backgroundColor
//		width
//		height
//		x  ,  y 
//方法 : 
//		render：渲染
//		random：随机生成盒子的位置 
var _position = 'absolute';
var _map = null;
//随机生成一个数
var Tool = {
	getRandom: function(min,max){
		min = Math.ceil(min);
		max = Math.floor(max);
		return Math.floor(Math.random() * (max - min + 1)) + min;
	}
}
function Box(options){
	options = options || {};
	this.backgroundColor = options.backgroundColor || 'red';
	this.width = options.width || 20;
	this.height = options.height || 20;
	this.x = options.x || 0;
	this.y = options.y || 0;
	this._div = null;
}
//把盒子对象渲染到地图上
Box.prototype.render = function(map){
	_map = map;
	//动态创建div
	var div = document.createElement('div');
	this._div = div;
	map.appendChild(div);
	//设置样式
	div.style.backgroundColor = this.backgroundColor;
	div.style.width = this.width + 'px';
	div.style.height = this.height + 'px';
	div.style.left = this.x + 'px';
	div.style.top = this.y + 'px';
	div.style.position = _position;
}
//随机生成位置
Box.prototype.random = function(){
	if(!_map) return;
	this.x = Tool.getRandom(0,_map.offsetWidth / this.width - 1 ) * this.width;
	this.y = Tool.getRandom(0,_map.offsetHeight / this.height - 1 ) * this.height;
	this._div.style.left = this.x + 'px';
	this._div.style.top = this.y + 'px';
}
 
var arr = [];
for(var i = 0 ; i < 10; i++){
	var r = Tool.getRandom(0,255);
	var g = Tool.getRandom(0,255);
	var b = Tool.getRandom(0,255);
	var a = new Box({
		backgroundColor:'rgb('+r+','+g+','+b+')'
	});
	a.render(box);
	arr.push(a);
}
setInterval(remo,500);
remo();
function remo(){
	arr.forEach(function(item){
		item.random();
	})
}

```
# 继承
## 借用构造函数,继承属性
```
function Aa(name,age){
	this.name = name;
	this.age = age;
}
function Bb(name,age,sto){
	Aa.call(this,name,age);
	this.sto = sto;
}
var s1 = new Bb('ls',18,10);
console.dir(s1);
```
## 混合继承
```
function Aa(name,age){
	this.name = name;
	this.age = age;
}
Aa.prototype.sayHi = function(){
	console.log(this);
}
function Bb(name,age,sto){
	Aa.call(this,name,age);
	this.sto = sto;
}


Bb.prototype = Aa.prototype;
Bb.prototype.constructor = Bb;

var s1 = new Bb('ls',18,10);
console.dir(s1);
```
## 函数声明and函数表达式
```
fn();
function fn(){

}
var fun = function(){
	
}
1.区别
	函数声明会提升，函数表达式不会
```
# 改变this所用的方法
## apply 改变this第二个参数传数组
```
var max = Math.max(1,2,3,4,5);
console.log(max);

var arr = [34,1,22,10];
var max = Math.max.apply(Math,arr);
console.log(max);
```
## call 改变this第二个参数传多个数
```
var o = {
	0:'a',
	1:'b',
	2:'c',
	3:'d',
	length:4
}

Array.prototype.splice.call(o,0,2);
console.log(o);
Array.prototype.push.call(0,2);
console.log(o);

```
## bind  不调用函数，只改变函数中的this指向
```
btn.onclick = function(){

}.bind(a);

```
# 函数中的成员
```
//可变参数
function max(){
	var max  = arguments[0];

	for(var i = 0; i < arguments.length; i ++){
		if(max < arguments[i]){
			max = arguments[i];
		}
	}
	return max;
	//Array.prototype.forEach.call(arguments,function(item) {
	//	console.log(item);
	//});
}
var m = max(1,2,3);
console.log(m)
```
#  高阶函数（函数作为参数）
```
var arr = ['c','a','b','d'];
// 默认情况下按照ascii码排序
arr.sort();
console.log(arr)

var a = [1,111,10,40,30];
a.sort(function(n1,n2){
	// 从小到大排序
	//return n1 - n2;
	// 从大到小排序
	return - (n1 - n2);
})
console.log(a)
```
# 函数作为返回值 案例：1 x + n
```
//100 + n
//1000 + n

function a(n1){
	return function(n2){
		return n1 + n2;
	}
}

var b = a(100);
var c = a(1000);

console.log(b(1))
console.log(c(1))
```
## 闭包的经典案例
```
var lis = document.querySelectorAll('#ul li');
var len = lis.length;
for(var i = 0 ; i < len ; i++ ){
	var li = lis[i];
	(function(index){
		li.onclick = function(){
			alert(index);
		}
	})(i);
}
```
## 闭包思考题
```
var name  = 'The Window';
var object = {
	name: 'My Object',
	getNameFunc:function(){
		return function(){
			return this.name;
		}
	}
}
console.log(object.getNameFunc()());

//输出：The Window;   没有闭包，this ＝ window；
```
```
var name  = 'The Window';
var object = {
	name: 'My Object',
	getNameFunc:function(){
		var that = this;
		return function(){
			return that.name;
		}
	}
}
console.log(object.getNameFunc()());
//输出：My Object;   有闭包，this ＝ object;
```
## 闭包的用法
```
box.onclick = fn(12);
//不能直接把返回结果传给点击事件
function fn(a){
	return function(){
		console.log(a);
	}
}
//因为此函数返回的是一个函数所以可以这样传
```
# tab 封装案例
## html
```
<style>
.avi{color: #00ff00;}
.aiv a{color: #00ff00;}
</style>
<div class="box">
	<ul>
		<li class="c avi">111</li>
		<li class="c">222</li>
		<li class="c">333</li>
		<li class="c">444</li>
		<li class="c">555</li>
	</ul>
	<div>
		<div class="c aiv"><a href="">1111</a></div>
		<div class="c"><a href="">222</a></div>
		<div class="c"><a href="">333</a></div>
		<div class="c"><a href="">444</a></div>
		<div class="c"><a href="">555</a></div>
	</div>
</div>
```
## js
```
;(function(){
	
	var _menus = null,
		_mains = null,
		_that = null;

	function Tab(options){
		options = options || {};
		this.container = options.container || '#wrapper';
		this.tabMenuSelected = options.tabMenuSelected || 'selected';
		this.tabMainSelected = options.tabMainSelected || 'selected';

		_that = this;
		//实现tab切换
		_tab.call(this);
	}

	function _tab(){
		//1.获取需要的元素
		var container = document.querySelector(this.container);
		//菜单的容器
		var tabMenu = container.children[0];
		//详细内容的容器
		var tabMain = container.children[1];
		//获取容器的内部元素
		_menus = tabMenu.children;
		_mains = tabMain.children;

		//2.给tab拦注册点击事件
		var i = 0 , len = _menus.length;
		for(; i < len; i++){
			var menu = _menus[i];
			menu.index = i;
			menu.onclick = _menusClick;
		}
	}

	function _menusClick(){
		//3.点击的时候切换tab栏
		//3.1取消所有的meun的选中效果
		var i = 0 , len = _menus.length;
		for(; i < len; i++){
			var menu = _menus[i];
			menu.className = menu.className.replace(_that.tabMenuSelected,'');
		}
		//3.2让当前点击的menu选中 
		this.className = this.className + ' ' + _that.tabMenuSelected;
		//4.点击的时候，切换详细内容
		for(i = 0; i < len; i++){
			var item = _mains[i];
			item.className = item.className.replace(_that.tabMainSelected,'');
		}
		//获取当前点击meun的索引
		var index = this.index;
		var main = _mains[index];
		main.className = main.className + ' ' + _that.tabMainSelected;
	}

	window.Tab = Tab;
})(window);

//使用

	new Tab({
		container:'.box',
		tabMenuSelected:'avi',
		tabMainSelected:'aiv'
	})
```
# 历史相关
```
history.back()去上一条历史
history.forward()去下一条历史
history.go()相对当前，正往前走，负往后走
history.pushState();新增历史
history.replaceState();替换历史

```
# 网络状态
```
	window.addEventListenter('online',function(){
		console.log('网络已链接');
	})
	window.addEventListenter('offline',function(){
		console.log('网络已断开');
	})
```
# 本地存储

## sessionStorage 5M 关闭浏览器会消失
```
Google Application 可以看到
只能存储字符串

存: sessionStorage.setItem('name','ls');
取: sessionStorage.getItem('name'); 
删: sessionStorage.removeItem('name');
清空: sessionStorage.clear();

```
## localStorage 20M 永久存储
```
存: localStorage.setItem('name','ls');
取: localStorage.getItem('name'); 
删: localStorage.removeItem('name');
清空: localStorage.clear()

```
## 注意
```
存不了对象,只能转换成字符串格式存储;
sessionStorage.setItem('name','{"name1":"zs"}');
将字符串转换成json;
JSON.parse(sessionStorage.getItem('name'));
```
# 动画
```
js － css 动画的区别

一个帧动画 一个是补间动画

帧动画：使用定时器 每隔一段时间 更改当前元素的状态
补间动画：过渡（加过渡 只要状态发生改变产生动画）动画（多个节点来控制动画）性能更好

定义动画序列:

@keyframes fly01{
	from{
		transform:translateY(-50px);
	}
	to{
		transform:translateY(50px);
	}
}

调用动画:
.dome{
	animtion:fly01 1s linear infinite alternate;
	一秒  匀速执行 无穷次 逆播放
}
```

# 正则表达式

## 元字符
```
.  表示的是：除了\n以外的任意的一个字符，'asd12421';

[] 表示的是：范围[0-9]表示的是0-9之间任意的一个数字，'4567876';
   另外的含义，把正则表达式中元字符的意义干掉,[.]就是一个.

[a-z] 表示的是：所有小写字母中任意的一个

[A-Z] 表示的是：所有大写字母中任意的一个

[a-zA-Z] 表示的是：所有字母中的任意一个

[0-9-zA-Z] 表示的是：所有数字或者字母中的任意一个

/ 表示的是：或者 ,[0-9]/[a-z]，要么是数字要么是小写字母

() 表示的是：分组 ，提升优先级 [0-9]/([a-z])/[A-Z]


都是元字符，但也可以叫限定符，下面这些

* :前面出现0次到n次 [a-z][0-9]*，小写字母中的任意一个，后面是要么是没有数字的，要么是多个数字的
  'asdad123' [a-z][0-9]*

+ :前面的表达式出现了1次到多次
  'asdasd9asd' [a-z][9]+

? :前面的表达式出现了0次到1次，最少是0次，最多一次；另一个含义；阻止贪婪模式
  '1234ic' [4][a-z]?

{} :更加明确的表示前面的表达式出现的次数,{0,4}表示0次到4次,{4}一定要出现4次

^ :表示的是以什么开始，或者是取反
  ^[0-9] 以小写字母开始
  [^a-z] 非小写字母
  [^0-9a-zA-z_] 特殊符号

$ :表示的是以什么结束
  
\d :数字中的任意一个

\D :非数字的一个

\s :空白符中的一个

\S :非空白符

\w :非特殊符

\W :特殊符

\b :单词边界，单词后面没单词

\t :
```





















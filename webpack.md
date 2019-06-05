

# webpack

[![img](https://img.shields.io/npm/v/webpack.svg?label=webpack&style=flat-square&maxAge=3600)](https://github.com/webpack/webpack/releases)

> npm install moduleName

```powershell
npm install moduleName # 安装模块到项目目录下
npm install -g moduleName # -g 的意思是将模块安装到全局，具体安装到磁盘哪个位置，要看 npm config prefix 的位置。
npm install --save moduleName # --save 的意思是将模块安装到项目目录下，并在package文件的dependencies节点写入依赖。
npm install --save-dev moduleName # --save-dev 的意思是将模块安装到项目目录下，并在package文件的devDependencies节点写入依赖。
//-S就是--save      //开发
//-D就是--save-dev  //测试
//-O就是--save-optional
//-E就是--save-exact
//-B就是--save-bundle
```

## 安装

```shell
//全局安装
npm install --global webpack
//如果你使用 webpack 4+ 版本，你还需要安装 CLI。
npm install --save-dev webpack-cli
```

## [起步](https://www.webpackjs.com/guides/getting-started/)

```shell
mkdir webpack-demo && cd webpack-demo
npm init -y
npm install webpack webpack-cli --save-dev
```

### **project**

```diff
  webpack-demo
  |- package.json
+ |- index.html
+ |- /src
+   |- index.js
```

### **src/index.js**

```javascript
function component() {
  var element = document.createElement('div');

  // Lodash（目前通过一个 script 脚本引入）对于执行这一行是必需的
  element.innerHTML = _.join(['Hello', 'webpack'], ' ');

  return element;
}

document.body.appendChild(component());
```

### **index.html**

```html
<!doctype html>
<html>
  <head>
    <title>起步</title>
    <script src="https://unpkg.com/lodash@4.16.6"></script>
  </head>
  <body>
    <script src="./src/index.js"></script>
  </body>
</html>
```

### **package.json**

```diff
+   "private": true,
-   "main": "index.js",
```

### 创建一个 bundle 文件

#### **project**

```diff
+ |- /dist
+   |- index.html
- |- index.html
```

要在 `index.js` 中打包 `lodash` 依赖，我们需要在本地安装 library：

```bash
npm install --save lodash
```

#### **src/index.js**

```diff
+ import _ from 'lodash';
-   // Lodash, currently included via a script, is required for this line to work
+   // Lodash, now imported by this script
```

#### **dist/index.html**

```diff
-    <script src="https://unpkg.com/lodash@4.16.6"></script>
-    <script src="./src/index.js"></script>
+    <script src="main.js"></script>
```

#### 执行

```shell
npx webpack
```

在浏览器中打开 `index.html`，如果一切访问都正常，你应该能看到以下文本：'Hello webpack'。

### 模块

 [`import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) 和 [`export`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)

#### 使用一个配置文件

#### **project**

```diff
+ |- webpack.config.js
```

#### **webpack.config.js**

```javascript
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
};
```

#### **dist/index.html**

```diff
-    <script src="main.js"></script>
+    <script src="bundle.js"></script>
```

```shell
//执行
npx webpack --config webpack.config.js
```

### NPM 脚本(NPM Scripts)

考虑到用 CLI 这种方式来运行本地的 webpack 不是特别方便，我们可以设置一个快捷方式。在 *package.json* 添加一个 [npm 脚本(npm script)](https://docs.npmjs.com/misc/scripts)：

#### **package.json**

```diff
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
+     "build": "webpack"
    },
```

现在，可以使用 `npm run build` 命令，来替代我们之前使用的 `npx` 命令。注意，使用 npm 的 `scripts`，我们可以像使用 `npx` 那样通过模块名引用本地安装的 npm 包。这是大多数基于 npm 的项目遵循的标准，因为它允许所有贡献者使用同一组通用脚本（如果必要，每个 flag 都带有 `--config` 标志）。

## 管理资源

### 加载 CSS

```bash
npm install --save-dev style-loader css-loader
```

为了从 JavaScript 模块中 `import` 一个 CSS 文件，你需要在 [`module` 配置中](https://www.webpackjs.com/configuration/module) 安装并添加 [style-loader](https://www.webpackjs.com/loaders/style-loader) 和 [css-loader](https://www.webpackjs.com/loaders/css-loader)：

#### **webpack.config.js**

```diff
 module.exports = {
  	output: {
    },
+   module: {
+     rules: [
+       {
+         test: /\.css$/,
+         use: [
+           'style-loader',
+           'css-loader'
+         ]
+       }
+     ]
+   }
};
```

#### **project**

```diff
  |- /src
+   |- style.css
```

#### **src/style.css**

```css
.hello {
  color: red;
}
```

#### **src/index.js**

```diff
+ import './style.css';
+   element.classList.add('hello');
```

```bash
npm run build
```

### 加载图片

```bash
npm install --save-dev file-loader
```

#### **webpack.config.js**

```diff
    module: {
      rules: [
+       {
+         test: /\.(png|svg|jpg|gif)$/,
+         use: [
+           'file-loader'
+         ]
+       }
 	]
}    
```

#### **project**

```diff
  |- /src
+   |- icon.png
```

#### **src/index.js**

```diff
+ import Icon from './icon.png';
+   // 将图像添加到我们现有的 div。
+   var myIcon = new Image();
+   myIcon.src = Icon;
+
+   element.appendChild(myIcon);
```

#### **src/style.css**

```diff
  .hello {
    color: red;
+   background: url('./icon.png');
  }
```

```bash
npm run build
```

### 加载字体

#### **webpack.config.js**

```diff
    module: {
      rules: [
+       {
+         test: /\.(woff|woff2|eot|ttf|otf)$/,
+         use: [
+           'file-loader'
+         ]
+       }
 	]
} 
```

#### **project**

```diff
  |- /src
+   |- my-font.woff
+   |- my-font.woff2
```

#### **src/style.css**

[字体格式](<http://www.mamicode.com/info-detail-627082.html>)|[字体下载](<http://font.chinaz.com/tag_font/WangYe.html>)

```diff
+ @font-face {
+   font-family: 'MyFont';
+   src:  url('Lazara.ttf') format('truetype');
+   font-weight: 600;
+   font-style: normal;
+ }

  .hello {
    color: red;
+   font-family: 'MyFont';
    background: url('./icon.png');
  }
```

```bash
npm run build
```
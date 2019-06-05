# Webpack4.0

[老马webpack教程笔记]([https://malun666.github.io/aicoder_vip_doc/#/pages/vip_2webpack?id=webpack-%E5%85%A5%E9%97%A8%E6%95%99%E7%A8%8B](https://malun666.github.io/aicoder_vip_doc/#/pages/vip_2webpack?id=webpack-入门教程))

# 安装

> 建议本地安装 , 因为每个项目的版本不一样

```shell
1、npm init -y               || - 初始化项目 - package.json
2、npm i -D webpack          || - 安装开发依赖
3、npm i -D webpack-cli      || - 4.0-以后的版本都要安装webpack-cli
4、创建 webpack.config.js     || - webpack配置项
```

# 处理CSS模块

```shell
1、npm i -D style-loader css-loader   ||- 下载css模块
2、npm i -D sass-looder node-sass     ||- 下载sass模块
4、npm i -D postcss-looder            ||- css3处理
4.1、npm i -D autoprefixer ||- 为css3添加前缀
```

> module > rules

```javascript
 {
     test: /\.(sa|sc|c)ss$/,
         use: [
             'style-loader',
             {
                 loader: 'css-loader',
                 options: {
                     sourceMap: true //开启后,在浏览器可以知道这段css来自哪里
                 }
             },
             {
                 loader: 'postcss-loader',
                 options: {
                     ident: 'postcss',
                     sourceMap: true,
                     plugins: loader => [
                         require('autoprefixer')({ browsers: ['> 0.15% in CN'] }) // 添加前缀
                     ]
                 }
             },
             {
                 loader: 'sass-loader',
                 options: {
                     sourceMap: true
                 }
             }
         ]
 }
```




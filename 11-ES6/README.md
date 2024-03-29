﻿

## 前言

ECMAScript 是 JS 的语言标准。而 ES6 是新的 JS 语法标准。

PS：严格来说，ECMAScript 还包括其他很多语言的语言标准。


很多人在做业务选型的时候，会倾向于选jQuery。其实jQuery的语法是偏向于ES3的。而现在主流的框架 Vue.js 和React.js的语法，是用的ES6。

ES6中增加了很多功能上的不足。比如：**常量、作用域、对象代理、类、继承**等。这些在ES5中想实现，比较复杂，但是ES6对它们进行了封装。

### ECMAScript 发展历史

- 1995年：ECMAScript 诞生。

- 1997年：ECMAScript 标准确立。

- 1999年：ES3 出现，与此同时，IE5 风靡一时。

- 2009年，ES5 出现，例如 foreach、Object.keys、Object.create 和 json 标准。

- 2015年6月，ES6正式发布。

ES6 的目标是：让 JS 语言可以编写复杂的大型应用程序，成为企业级开发语言。

### ECMAScript 的各大版本

- ES5 : 09年发布。

- ES6：ECMAScript 2015年6月

- ES7：ECMAScript 2016

- ES8：ECMAScript 2017



### ES6 的其他优势

- 使用 babel 语法转换器，支持低端浏览器。

- 流行的库基本都是基于 ES6 构建。 React 默认使用 ES6 标准开发。


###

## ES6的环境配置

掌握 ES6 之后，如果要考虑 ES5 的兼容性，可以这样做：写 ES6 语法的 js 代码，然后通过 `Babel`将 ES6 转换为 ES5。

但是，在这之前，我们需要配置一下相关的环境。

### 建立工程目录

（1）先建立一个空的工程目录 `ES6Demo`，并在目录下建立两个文件夹 `src`和 `dist`：

- `src`：书写ES6代码，我们写的 js 程序都放在这里。

- `dist`：利用 Babel 编译生成的 ES5 代码。**我们在 HTML 页面需要引入 dist 里的 js 文件**。

（2）在 src 里新建文件  `index.html`：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <!-- 我们引入 ES5 中的 js 文件，而不是引入 ES6 中的 js 文件。 -->
    <script src="./dist/index.js"></script>
</head>
<body>

</body>
</html>
```

**注意**，上方代码中，我们引入的是`dist`目录下的 js 文件。

然后我们新建文件 `src/index.js`：

```javascript
let a = 'smyhvae';
const b = 'qianguyihao';

console.log(a);
console.log(b);
```


这个文件是一个 ES6语法 的js文件，稍后，我们尝试把这个 ES6 语法的 js 文件转化为 ES5 的 js 文件。

PS：我们在写代码时，能用单引号尽量用单引号，而不是双引号，前者在压缩之后，程序执行会更快。

### 全局安装 Babel-cli

（1）初始化项目：

在安装Babel之前，需要先用 npm init 先初始化我们的项目。打开终端或者通过cmd打开命令行工具，进入项目目录，输入如下命令：


```bash
	npm init -y
```

上方代码中，`-y` 代表全部默认同意，就不用一次次按回车了（稍后再根据需要，在文件中手动修改）。命令执行完成后，会在项目的根目录下生成package.json文件：

```json
{
  "name": "es6demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "smyhvae",
  "license": "ISC"
}

```


PS：VS Code 里打开终端的快捷键是：`Contol +  ~`。

（2）全局安装 Babel-cli：

在终端中输入以下命令：

```bash
	npm install -g babel-cli
```


![](http://img.smyhvae.com/20180304_1305.png)

如果安装比较慢的话，Mac 下可以使用`cnpm`进行安装 ，windows 下可以使用`nrm`切换到 taobao 的镜像。


（3）本地安装 babel-preset-es2015 和 babel-cli：

```bash
	npm install --save-dev babel-preset-es2015 babel-cli
```

![](http://img.smyhvae.com/20180304_1307.png)

安装完成后，会发现`package.json`文件，已经多了 devDependencies 选项：

![](http://img.smyhvae.com/20180304_1308.png)

（4）新建.babelrc：

在根目录下新建文件`.babelrc`，输入如下内容：

```
{
    "presets":[
        "es2015"
    ],
    "plugins":[]
}
```


（5）开始转换：

现在，我们应该可以将 ES6 的文件转化为 ES5 的文件了，命令如下：（此命令略显复杂）

```
	babel src/index.js -o dist/index.js
```

我们可以将上面这个命令进行简化一下。操作如下：

在文件 `package.json` 中修改键 `scripts`中的内容：


```json
  "scripts": {
    "build": "babel src/index.js -o dist/index.js"
  },
```

修改后的效果如下：

![](http://img.smyhvae.com/20180304_1315.png)

目前为止，环境配置好了。以后，我们执行如下命令，即可将`src/index.js`这个 ES6 文件转化为 `dist/index.js`这个 ES5 文件：


```bash
	npm run build
```


我们执行上面的命令之后，会发现， dist目录下会生成 ES5 的 js 文件：

index.js：

```javascript
	'use strict';

	var a = 'smyhvae';
	var b = 'qianguyihao';

	console.log(a);
	console.log(b);

```



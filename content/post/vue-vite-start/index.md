---
title: "Vue & Vite 项目搭建入门"
date: 2021-05-22T18:25:17+08:00
lastmod: 2021-05-22T18:25:17+08:00
image: "cover.png"
description: "学习 Vue 与 Vite 项目搭建的踩坑记录"
categories: ["Code"]
tags: ["Vue", "Vite"]
---

# 学习 Vue 与 Vite 项目搭建的踩坑记录

## Vue 2.0 + Webpack

### Installation

虽然 Vue 的最新版已经是 3.0，而且 Vite 也似乎是大势所趋，不过我还是选择了先从 Vue 2.0 + Webpack 这种比较传统的方式开始学习。主要是因为这几个方面：

- 现在仍有许多项目没有从 Vue 2.0 迁移到 3.0，使用 Vue 2.0 进行开发在实际生产中还是比较常见的，所以以后还可能会用到 2.0；
- Vue 2.0 的资料相对比较多，踩坑时更容易查到资料；
- 学习 2.0 之后再学习 3.0，会对 3.0 的改进有更深的体会，比如能够切身体会到 Vue 3.0 为什么这么那么设计、这样设计有什么好处、它解决了哪些痛处与弊端等；
- 入门曲线相对平缓。

好了，前言就先讲到这里，接下来进入正题。

首先创建项目：

```bash
$ mkdir vue-learning
$ cd vue-learning
$ npm init -y
```

接着安装相关库：
```bash
$ npm install -D webpack webpack-cli webpack-dev-server html-webpack-plugin
$ npm install vue
```

创建相关文件，创建后的目录结构：

```bash
$ tree -I "node_modules"
.
|-- dist
|   `-- main.js
|-- package-lock.json
|-- package.json
|-- public
|   `-- index.html
|-- src
|   `-- index.js
`-- webpack.config.js

3 directories, 6 files
```

`webpack.config.js` :

```js
const { resolve } = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
    mode: "production",
    entry: "./src/index.js",
    output: {
        path: resolve("dist"),
        filename: "[name].js"
    },
    plugins: [ new HtmlWebpackPlugin({
        template: "./public/index.html"
    }) ],
    devtool: "source-map"
}
```

然后基础部分就做完了，接下来开始加入 Vue：

```js
// src/index.js

import Vue from 'vue';

const app = new Vue()
```

打包之后可以看到 `dist/main.js` 中已经引入了 Vue 的 runtime。

### Runtime + Compiler vs. Runtime-only

按照官网文档中的第一个例子，修改 `index.html` 的 `body` 为：

```html
<div id="app">
    {{ message }}
</div>
```

并修改 `index.js` 为以下内容：

```js
import Vue from 'vue';

const app = new Vue({
    el: "#app",
    data: {
        message: "Hello World!"
    }
})
```

然后 `npx webpack serve` ，打开 `http://127.0.0.1:8080`，发现控制台报错：

> You are using the runtime-only build of Vue where the template compiler is not available. Either pre-compile the templates into render functions, or use the compiler-included build.

原因是我们导入 Vue 时使用的语句是 `import Vue from 'vue';`。这行语句默认会导入 `Runtime-only` 版本的 Vue，而这个版本的 Vue 是不含编译器的，因此无法编译像 `{{ message }}` 这种模板语句。

为了解决这个问题，我们可以修改一下 `webpack.config.js` 的内容，向其 `module.exports` assign 以下内容

```js
{
    resolve: {
        alias: {
            'vue$': 'vue/dist/vue.esm.js'
        }
    }
}
```

然后编译，运行，成功！

相关参考资料：

- https://vuejs.org/v2/guide/installation.html#Runtime-Compiler-vs-Runtime-only

### Single File Components

接下来介绍一下单文件组件，这也是我比较推崇的方式。之前我写 React 时，就是采取了 CSS in JS 的方案，而不是 SCSS 或者是 Less；因为如果将`.less` 和 `.jsx` 分割开来的话，后期维护就会变得非常难受了。而单文件组件不仅目录结构看起来更舒服，在可维护性上也更加友好。不过 Vue 的单文件组件相比非单文件组件来说，并不仅仅是以上这些方面，比如说原生的 JavaScript 并不支持 jsx 语法，用模板字符串就显得丑了吧唧的等等。具体可参考[官方文档](https://vuejs.org/v2/guide/single-file-components.html)，这里就不赘述了。

然后正式开始，首先安装 `vue-loader` 和 `vue-template-compiler`:

```bash
$ npm install -D vue-loader vue-template-compiler
```

修改 `webpack.config.js`，将以下内容添加到 `module.exports`:

```js
const { VueLoaderPlugin } = require('vue-loader')

module: {
    rules: [{
        test: /\.vue$/,
        use: "vue-loader"
    }],
    plugins: [
        new VueLoaderPlugin()
    ]
},
```

参考文档：https://vue-loader.vuejs.org/guide/#manual-setup

然后修改`index.html`，`div#app`里面的内容全删掉就可以了，只留下：

```html
<div id="app"></div>
```

创建文件`App.vue`：

```vue
<template>
    <div>
        {{ message }}
    </div>
</template>

<script>
export default {
    data() {
        return {
            message: "Hello World!"
        }
    }
}
</script>
```

修改 `index.js`：

```js
import Vue from 'vue';
import App from './App.vue'

new Vue({
    el: "#app",
    template: "<App />",
    components: { App }
})
```

此时目录结构：

```bash
$ tree -I "node_modules|dist"
.
|-- package-lock.json
|-- package.json
|-- public
|   `-- index.html
|-- src
|   |-- App.vue
|   `-- index.js
`-- webpack.config.js

2 directories, 6 files
```

完成。


## Vue 3.0 + Vite

### Creation

```bash
$ npm init @vitejs/app
√ Project name: · vite-project
√ Select a framework: · vue
√ Select a variant: · JavaScript
$ cd vite-project
$ npm install
```

安装就这么完成了，没啥好说的。

### Simplification

如果正在阅读这篇文章的你是一名初学者的话，可能对这复杂（或许？）的目录结构毫无头绪，而且事实上它也确实用到了许多高级/新特性。所以我们可以试着将它简化并还原一下。

简化后：

`src/components/HelloWorld.vue`

```vue
<template>
  <h1>{{ msg }}</h1>
</template>
```

`src/App.vue`

```vue
<template>
  <HelloWorld msg="Hello Vue 3 + Vite" />
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
export default {
    components: { HelloWorld }
}
</script>
```

## Next

既然基础的项目搭建学会了，接下来就可以直接⏩到实战了（雾）。而且我最近正好要参加一个学校组织的叫什么什么数据库的比赛，或许可以考虑用比赛项目的前端部分来给 Vue 练手一下（）。那么接下来，就继续学习 Vue & Vite 吧！


---
title: "TypeScript & Webpack & Babel & Eslint & React: Not Such a Beautiful Marriage"
date: 2021-04-14T00:00:00+08:00
lastmod: 2021-05-15T00:00:00+08:00
image: "cover.png"
description: "好吧，我承认我有点标题党，主要是因为我最近被这几个工具折腾的不轻，所以想吐槽一下。"
categories: ["Code"]
slug: "not-such-a-beautiful-marriage"
---

好吧，我承认我有点标题党，主要是因为我最近被这几个工具折腾的不轻，所以想吐槽一下。（顺带一提，标题~~抄袭~~引用改编自著名博文[TypeScript with Babel: A Beautiful Marriage](https://medium.com/dailyjs/typescript-babel-ce24f724398)）

## Origin

看了下 cqupt-help-mp 的源码，发现里面用了许多库。感觉自己对这些工具还不太熟练，所以想先趁机练习一下。

对于题目中的五个工具，除去 ESlint之外，其实其他的都多多少少用过一段时间。特别是 Webpack Babel React 这三个，用的时间也算是比较久了。TypeScript 倒是用过，但是感觉 JS 加上类型限制后写起来就不那么自由了，所以尝试用它写了一个模块之后就将它卸载了。但是这些这个工具终究还是太常用，早晚逃不了还是得学，所以就试着用它们做个简单的小项目练练手。毕竟当初我学 Webpack 时也是学得很憋屈，一直不知道这玩意儿打个包为啥还非得用 js 文件做入口，直到后来学了 React 才豁然开朗，所以我至今认为如果是要做跑在浏览器的网页而且不用什么框架的话 Webpack 还真没什么卵用。当然，有了之后自然用处就大了。

不过说了这么多好像有点跑题，先回到正题：让我们来讨论一下究竟怎么把这五个架子给支楞到一块儿去。

## Introductions of the packages

这种东西还用得着在这介绍？官网文档，维基百科，请~

## TSC or Babel, it's a question. 

老实说，单是在两个之间进行选择就够让我想半天了。但是实际上，摆在我面前的包括但不限于：`ts-loader`,`awesome-typescript-loader`, `babel-loader`等一堆loaders/packages。而且由于我开始参考了过时的中文文档，所以先下载了`awesome-typescript-loader`，后来才意识到被坑（😅）。而在剩余的两者之间，参考[TypeScript with Babel: A Beautiful Marriage](https://medium.com/dailyjs/typescript-babel-ce24f724398)后，决定选择了后者。

安装好之后，我又顺着我的思路捋了一遍，发现整个过程中编译是`babel`做的，根本没用到`typescript`什么事儿呀。我于是就想，把它卸载之后会怎么样呢？接着神奇的现象发生了，我卸载了`typescipt`，删除了它的配置文件`tsconfig.json`，结果发现我的用`typescript`写的项目居然还能正常编译并运行（谢谢有被震惊到）。我就想既然如此那我还用ts干啥呀，赶紧卸了得了，可别再让我见着你了（😅）。

## Anti-human ESLint

听说 ESLint 可以用在团队协作中来保证代码风格的一致性？好，名单赶紧安排上，加急。

安装过程中被问了几个问题，回答之后他给我推荐了几个插件。行，听您的。选择 YES，进行安装。看起来一切顺利，然后...果不其然，报错了。报啥呢：

```
Oops! Something went wrong! :(

ESLint: 7.24.0

Error: An error occurred while generating your JavaScript config file. A config file was still generated, but the config file 
itself may not follow your linting rules.
Error: Failed to load plugin '@typescript-eslint' declared in 'BaseConfig': Cannot find module 'typescript'
```

我心想，这不废话嘛，我又没安装 typescipt 您能到哪儿找模块去？不过话又说回来，刚才，它倒是的确问过我这么一句：

```
Does your project use TypeScript? · No / Yes
```

唔，我要用 TypeScript 倒是没错，不过这和我安没安装 TypeScript 的 package 也没什么必然的联系吧。

然后经过摸滚带爬的上下求索，终于意识到 typescript 貌似还是有点儿用的，再加上后面用到的 `@types/react`，`@types/react-dom`，感觉是在类型推断和自动补全方面有些作用（应该）

## Module or CommonJS, it's a question. 

刚才 ESLint 问我用的是 `Module` 还是 `CommonJS`，秉着“啥新用啥，用嘛嘛新”的原则，我当然选择的ES6的`Module`。

然后由于我的`webpack.config.js`用到了`module.exports = {}`，所以 ESLint报错了。

```
'require' is not defined. eslint(no-undef)
'module' is not defined. eslint(no-undef)
```

好，那我改。

我把 `module.exports`改成了`export default`，把`require`改成了`import`。结果又报错了：
```
SyntaxError: Cannot use import statement outside a module
```

好，那我改。
我在`package.json`里加了句 `"type": "module"`。结果又报错了：
```
ReferenceError: __dirname is not defined
```
`__dirname` 用不了可不行，那还是再改回来吧。
好，那我改。
于是回到了原点：

```
'require' is not defined. eslint(no-undef)
'module' is not defined. eslint(no-undef)
```
（闹着玩是吧？😅）

然后在`.eslintrc.js`的`"env"`中加了行`"node": true`，行，终于，不报错了。

## ESLint Comes Again

`App.js`里有这么一段：

```tsx
const Box = ({ title, children }) => (
    <div>
        <h2>{title}</h2>
        {children}
    </div>
)
```
ESLint报错：
```
Missing return type on function. eslint(@typescript-eslint/explicit-module-boundary-types)
```

接着修改代码为：

```tsx
// has imported ReactElement...
const Box = ({ title, children }): ReactElement => (
    <div>
        <h2>{title}</h2>
        {children}
    </div>
)
```

ESLint报错：

```
Object pattern argument should be typed. eslint(@typescript-eslint/explicit-module-boundary-types)
'title' is missing in props validation eslint(react/prop-types)
Binding element 'children' implicitly has an 'any' type. ts(7031)
```

接着修改代码为：

```tsx
// has imported ReactElement...
type BoxProps = {
    title: string,
    children: ReactElement
}

const Box = ({ title, children }: BoxProps): ReactElement => (
    <div>
        <h2>{title}</h2>
        {children}
    </div>
)
```

ESLint在这块儿倒是不报错了，不过当其他地方调用`Box`又报错了：

```
This JSX tag's 'children' prop expects a single child of type 'ReactElement<any, string | JSXElementConstructor<any>>', but multiple children were provided.
```

查了下资料，发现除了`ReactElement`之外原来还有`ReactNode`和`JSX.Element`（参考 [When to use JSX.Element vs ReactNode vs ReactElement?](https://stackoverflow.com/questions/58123398/when-to-use-jsx-element-vs-reactnode-vs-reactelement)）。

简单总结下，原因：拉胯TS的历史遗留问题；解决方法：将`children`类型改成`ReactNode`

## `react-jsx`

在上面的 StackOverflow 里看到了 `create-react-app`，这才想到可以去参考一下它的代码，话不多说，开始安装。

```bash
npm init -y
npm install create-react-app
npx create-react-app ts --template typescript
```

进去之后发现他在`tsconfig.json`里的`"compilerOptions"`写有`"jsx": "react-jsx"`，而我之前一直都是用的`"jsx": "react"`。好奇他们之间的差别，所以去 TypeScript官网文档查了下[资料](https://www.typescriptlang.org/tsconfig/#jsx)：

> Controls how JSX constructs are emitted in JavaScript files. This only affects output of JS files that started in .tsx files.
>
> * react: Emit .js files with JSX changed to the equivalent React.createElement calls
> * react-jsx: Emit .js files with the JSX changed to _jsx calls
> * react-jsxdev: Emit .js files with the JSX to _jsx calls
> * preserve: Emit .jsx files with the JSX unchanged
> * react-native: Emit .js files with the JSX unchanged

Umm，说实话没看懂，继续往下滑，发现一个[链接](https://reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html)，点进去，读了下，发现`react-jsx`相对`react`有许多优点，例如：

> - With the new transform, you can **use JSX without importing React**.
> - Depending on your setup, its compiled output may **slightly improve the bundle size**.
> - It will enable future improvements that **reduce the number of concepts** you need to learn React.

继续秉承着“啥新用啥”的信条，我便将`tsconfig.json`里的`"jsx": "react-jsx"`改成了`"jsx": "react"`。但是过了会儿突然想起来我使用的`babel-loader`编译的，所以写在`tsconfig.json`里其实并没有什么卵用...

所以应该修改`babel.config.js`的`module.exports`，添加以下属性：

```js
module.exports = {
    "presets": [
    	// ...
        [
            "@babel/preset-react", {
            	// ...
                "runtime": "automatic"
            }
        ]
    ]
}
```

不过单单这么改还是会报错，这是因为 ESLint 的缘故，实际上代码是可以正常编译的。不过 ESLint 总是报错看着很不爽呀，说实话我都想把它给卸载了。但是转念又想到将来团队协作肯定是要用到的，只好再硬着头皮继续写了。

修改 ESLint 配置文件：

```js
{
  // ...
  "rules": {
    // ...
    "react/jsx-uses-react": "off",
    "react/react-in-jsx-scope": "off"
  }
}
```


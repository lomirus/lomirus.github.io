---
title: "大一第二学期中期总结与反思 (2021-05)"
date: 2021-05-09T01:00:00+08:00
lastmod: 2021-05-12T18:25:17+08:00
description: "虽然说是期中总结，不过奈何拖延症又犯了，于是乎就又拖了两天，就这么拖过了期中考试。眼看再没两个月估计就又得期末考试了，再拖下去连标题都得改，所以总算决定开始动笔了。"
categories: ["随笔，及其他"]
slug: "2021-05"
---

## 我这半学期学了什么

寒假的时候我一直在写考核作业。但有一说一，虽说我肝得挺厉害（至少自我感觉是），但实际上我感觉我并没学到什么太多的新东西。当然，倒不是说一点也没有，只是感觉太少了。

寒假的时候一直想学下 Webpack 和 React 的相关知识，但苦于没时间，而到了开学之后反倒是有时间了（离谱）。于是乎我便先学习了这两个比较基础且常用的。我之所以将首选框架选为 React 而不是 Vue，主要是因为我跨平台开发框架很感兴趣，而 Flutter 对我这个前端还算比较比较遥远，因此考虑到 React Native 就先学了 React。

### [counter-trainer-deprecated](https://github.com/lomirus/counter-trainer-deprecated)

我第一有关 React 的项目是 counter-trainer （现在更名为 [counter-trainer-deprecated](https://github.com/lomirus/counter-trainer-deprecated)），里面还顺便用到了 Less。这个项目算我前端工程化的开荒，更多的还是为了练习。开发初期的规划是想通过自适应以兼容各种分辨率，结果发现远比自己想象的复杂，加之当时对于组件间通信不甚熟悉，以及审美堪忧，最终导致代码堆成屎山，因此终止了开发。

### [counter-trainer](https://github.com/lomirus/counter-trainer)

紧接着不久我就开始了第二个项目，同时也是前者的续作，也叫做 [counter-trainer](https://github.com/lomirus/counter-trainer)。吸取了上一次的教训，这次只针对了移动平台进行适配，因此我选择了使用 React Native 进行开发。我从这这个项目中学习到的主要有这么几方面：

1. 学习了 Redux，了解了组件间通信、生命周期、Context、Fragment 等相对进阶的内容；
2. 开发的复杂程度不断增加，迫使我去寻找已有的库而不是重复造轮子，而在这个过程中我不得不去直接阅读一些框架的官方英文文档、去 Issues 里寻找 bugs 的解决方案。在此之前我顶多会使用像 Redux，Babel 这种近乎 Official 性质的库，对于那些小众的库还是有些抵触的，不过最后，相比于自己从头开发，直接 import 还是让我直呼真香。比如随便举几个这次用到的一些库：`react-navigation`, ` react-native-tts` 等等，像其中的 tts 这种库，就绝不是现在的我能够徒手撸出来的，所以我也学会了不要去造一些没有必要的轮子。
3. （复习了 Android 开发？（继上一次的 Kotlin 计算器））
4. （直接读英文文档进一步提升了英文水准？）
5. （了解了语义化版本？）

### [sekiro-symbols-generator](https://github.com/lomirus/sekiro-symbols-generator)

这个项目应该是我最近的项目中完成度最高，也是唯一没有弃坑且最终按照原计划圆满完成了的。最终代码量约1000行。下面说一下我从这个项目中学到的东西（或者是这个项目的一些 features）：

1. React Hooks：该项目全部使用函数式组件；同时在组件间通信方案的选择上废弃了 Redux ，改为了使用 React 原生 hooks；
2. Typescript：该项目全部使用 Typescript；
3. ESlint：该项目使用了 ESlint（这个好像不算什么亮点）；
4. CSS in JS：该项目没有采取传统的 `Less` or `SASS`/`SCSS`，而是选择了 css in js 的方案。并且在经过了多方权衡之后，最终决定了采用 `emotion` 作为样式框架
5. Webpack：打包时的一些优化，如 `source-map`, `code splitting`；
6. Github Actions：自己写了个脚本自动部署并发布网站；
7. Other: 如 `React automatic runtime`, `Forwarding Refs`,`.gitattributes` 文件的使用等。

### Deno

学了点 Deno，不过生态还是太匮乏，暂时还没什么卵用。

有个叫 `Aleph` 的框架可以用 `React`， 不过是 `SSR`, 不太符合我口味。

### Rust

这学期还学习了一门新语言，那就是 Rust. 打老早之前就见到很多人都在吹它，而且后来又看到 Rust 大佬 @ahabhgk 的身体力行，以及 Rust 在 WebAssembly 上无可比拟的优势以及其他各种优秀特性等等。所以就开始学习这门语言了。

目前基础语法方面已经学完了，最近在学一些智能指针、并发等相关操作。而且开发的过程中甚至遇到了一些语言本身的 "feature"，以至于我还专门跑去官方仓库的 Issues 寻找解决方案，官方则表示只能暂时使用 unstable 的 nightly 特性作为临时解决方案，说明 Rust 还是挺早期的，还有很多坑都没填...

### Vue

今天才刚刚开始学 Vue/Vite，所以暂时还没什么好说的。

~~（不过我还是要吐槽一下 Vue 文档辣鸡（小声））~~

## 接下来要做什么

~~😅说了这么多，还是得先把课内各种 damn courses 给解决掉，虽然有些水课设立就是为了恶心人的，但是我们也无可奈何。~~

立几个 flag，不过不要求要在本学期内完成，也不一定完全按照它执行，只是当作一个 Todo List 而已：

### Web

- Vue & Vite；
- React/Vue Router；
- WebAssembly;
- Next.js (SSR);
- WebRTC

### Hybrid

- Dart & Flutter

### Game

- Bevy/Godot;

### Other

- Decentralization, Blockchain
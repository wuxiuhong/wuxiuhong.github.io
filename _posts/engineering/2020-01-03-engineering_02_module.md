---
title: "模块化编程"
subtitle: "「前端工程化」- 模块化编程"
header-img: "img/post-bg-halting.jpg"
header-mask: 0.2
layout: post
author: "wendy"
tags:
  - 前端工程化
  - 模块化
---

- 怎么理解模块化编程
- 模块化规范
- 模块加载的运行原理

怎么理解模块化编程
----------------------
> 模块化是一种处理复杂系统分解成为更好的可管理模块的方式，可以把系统代码划分为一系列职责单一，高度解耦且可替换的模块。或者说有组织把一个大文件拆分独立并互相依赖的多个小模块、依赖管理、一个文件对应一个模块

为什么需要模块化？

> 模块化降低可维护性，负责功能解耦拆分独立相互依赖的小模块，可维护性、命名空间（作用域）、依赖管理（早期用script引入，会有先后顺序）、可复用性。

- 很多主流的模块化解决方案通过JavaScript运行时来支持“匿名闭包”、“依赖分析”和“模块加载”等功能。
- 依赖分析需要在JavaScript运行时通过正则匹配到模块的依赖关系；
- 顺着依赖链（顺着模块声明的依赖层层进入，直到没有依赖为止）；
- 把所有需要加载的模块按顺序一一加载完毕。

> 模块多、依赖关系负责的情况会严重影响页面性能。

模块化规范
----------------------
- `CommonJS`;
- `AMD` 用define定义模块，用require()加载模块;
- `CMD` 用define(factory)
- `ES6模块`： 编译时确定模块的依赖关系；加载模块存储的是值的引用，所以全局只有一份；加载模块也是异步的

有CommoneJS规范，最典型的实践就是Node.js，主要使用在服务器端，`同步`加载模块；<br/>
有AMD，最典型的实践就是RequireJS，依赖前置，主要使用在浏览器端，`异步`加载模块。<br/>
有CMD，最典型的实践就是sea.js，依赖就近，主要使用在浏览器端，`异步`加载模块。<br/>
有ES6的Module，在语言层面定义了模块，通过export和import，吸收了CommoneJS和AMD两者的优点，兼容两标准的规范

### 模块发展历程

1 函数封装 function foo(){//...}    =>  `全局污染` <br/>
2 对象namespace var obj = {count:1, foo:function(){}}   => `私有成员暴露，内部状态可被外部改写`<br/>
3 立即执行函数 (function(root){})(this)   => `提供逻辑划分，不解决代码本身的划分`

前端模块化框架
----------------------

一、CommonJS
> 每一个文件都是一个模块，都有自己的作用域。变量、函数、类都是私有，不被其他模块取读、修改，除非用exports进行暴露，require加载模块

三部分： module（模块标识）、 require（模块引用） exports（模块定义）
- `module`变量在每个模块内部，就代表当前模块；
- `exports`属性是对外的接口，用于导出当前模块的方法或变量；
- `require()`用来加载外部模块，读取并执行js文件，返回该模块的exports对象；

`思考`：
* 怎么定义一个模块？
* 如何优雅的把模块API暴露出去？ exports
* 如何方便使用依赖的模块？require


二、AMD
> amd就是异步模块定义，采用异步方式加载模块。通过define方法定义模块，require方法加载模块。

与Commonjs区别：
1 依赖前置

如果这个模块还需要依赖其他模块，那么define函数第一个参数，是一个数组，指该模块的依赖。<br/>
define([tools], function(){});  define(['a.js','b,js'], function(a, b){});<br/>
若a.js未使用，如何处理<br/>
参数module数组，要加载的模块。 callback加载成功之后的回调函数<br/>
require([module], callback)<br/>

`思考`：
- `依赖前置` => 懒加载的书写方式，用到了谁就加载谁， 异步加载
- 尽量避免循环引用模块，CommonJS、AMD、es6模块目前没有很好的解决

`一体化`的模块化实践方案
----------------------

> 前端模块：Template模块、JS模块、css模块

解决的问题：
- 模块静态资源管理，一般包含JavaScript\css等其他静态资源，需要记录和管理这些静态资源；
- 模块依赖管理，模块间存在各种依赖关系，在加载模块时候，需要处理好这些依赖关系；
- 模块加载： 在模块初始化之前需要将模块的静态资源以及所依赖的模块加载并准备好；
- 模块沙箱（模块闭包），在JavaScript模块中我们需要主动对模块添加闭包用于解决作用域问题；

编译工具管理模块
- 通过编译工具（自动化工具）对模块进行编译处理，包括对静态资源进行预处理（对javascript模块添加闭包，对css进行less预处理等);
- 记录每个静态的部署路径以及依赖关系并生成资源表(resource map)。
- 我们通过编译工具托管所有静态资源，帮我们解决模块静态资源管理、模块依赖关系、模块沙箱问题

使用静态资源加载框架加载模块<br/>
主要包含前端模块加载框架，用于javascript模块支持，控制资源的异步加载<br/>
静态资源加载框架可以用于对页面进行持续的自适应的前端性能优化，自动对页面不同情况投递不同的资源加载方案

.. sectnum::
   :start: 1

#################
Angular概述
#################

什么是Angular
=================
Angular是由Google公司开发的一款前端开源框架，也是SPA（single-page-application，单页应用程序）框架。所谓SPA是指只有一个Web页面的应用程序，它是通过动态重写当前页面而不是从服务器加载整个新页面来与用户交互的Web应用程序或网站。SPA的优势就是为用户提供了一个更接近本地移动或桌面应用程序的体验。

Angular是基于客户端TypeScript的框架，用于构建动态的移动和桌面Web应用程序。它充分利用现代Web平台的各种能力，提供了创建Web应用程序的强大工具，帮助开发者快速从事Web开发。使用Angular可以极大地简化前端开发的负担，并为用户提供良好的体验。

为什么要用Angular
====================
Angular很强大，用户选择Angular的理由有很多，下面列举了其中几种：

速度和性能
**************
速度和性能是选择Angular的重要原因之一。通过Web Worker和服务端渲染技术，Angular的强大渲染引擎在发布应用程序的时候能把APP压缩到是原来的60%左右，达到在如今的Web平台上所能达到的最高速度。

跨平台运行
**************
Angular的模板编译是跨平台的，能同时支持移动端和桌面端，即一套框架，多种平台。让用户界面能更好的的呈现在用户面前。

可伸缩性的设计
**************
Angular的模块化，组件化的设计能让用户有效的掌控可伸缩性，提高程序员的开发速度，很容易的编写保持一致风格化和更具备可伸缩性的代码。

稳定性
**************
Angular从一开始构建的时候就非常注重稳定性。在Google内部，当一个工程师改了一行Angular代码的时候有成千上万的单元测试都会被运行，因此Angular是一个稳定的平台，新出的版本不会破坏以前现有产品的开发。

Google和微软公司的支持
**************************
Google公司在2017年的开发者大会上，确认将长期支持Angular。许多开发人员认为Google支持Angular，使该平台值得信赖。同时，Angular框架选择使用TypeScript语言开发，TypeScript编程语言是微软公司的产品，因此Angular背后有Google和微软两大公司的支持。

强大的生态系统
**************
Angular强大的第三方组件生态系统。 Angular的流行导致出现了数以千计的可用于Angular应用程序的其他工具和组件。用户可以直接复用这些组件和工具，如：AOT、Angular Universal和Angular CLI，使用这些工具有助于用户快速开发项目。

Angular的版本
======================
Angular的历史可追溯到2009年，当时Misko Hevery和Google工程师Adam Abrons开发了后来被称为AngularJS的框架，并于2010年正式发布。AngularJS的主要优势是它可以将基于HTML的文档转换为动态内容。 在AngularJS出现之前，Web标记语言HTML始终是静态的，这意味着用户无法主动与HTML页面上的接口进行交互。 有一些方法可以构建动态的单页面应用程序（SPA），但它们太复杂。 AngularJS减少了创建动态内容的开发工作量，用户可以获得具有动态表单和元素的网页。

2016年9月，Google发布了Angular 2。它是由 AngularJS 的同一个开发团队完全重写的，与网络日益现代的需求相匹配。现在人们常说的Angular（后面没有js）框架泛指从2到最新版本8的Angular框架。

Angular的版本号由3部分组成：[主要版本号].[小版本号].[补丁版本号]

* 主要版本号的变化表明软件中主要的功能接口发生了变化，可能不再兼容低版本的代码，因为API已经改变了。如Angular从7变成8时，这是主版本号的变化。
* 小版本号的变更表示功能更新，如增加了新功能。
* 补丁版本号是用来修复bug的。

Angular 8于2019年5月28日发布，目前最新版本是8.2.15，本书讲解及示例都基于此版本。



Angular的核心概念
====================

组件
********
组件是构成Angular应用程序的基础和核心，它是一个模板的控制类，应用程序使用组件处理页面逻辑及视图显示。组件知道如何渲染自己及配置依赖注入，并通过一些由属性和方法组成的API与视图进行交互，每个组件都能独立完成各自的功能。

在基于Angular组件的体系结构中，应用程序把逻辑功能和组件分开。这些组件可以轻松替换和解耦，并可以在应用程序的其他部分中重复使用。此外，组件独立性不仅使测试Web应用程序变得容易，而且还能确保每个组件都能无缝运行。

模板和数据绑定
****************
使用组件时，Angular是通过模板渲染来显示组件内容。组件模板通过数据绑定来动态设置 DOM（文档对象模型）的值。如：实现把组件数据映射到模板上，或者从模板（如input控件）中取回数据到组件中。

服务
****************
Angular把组件和服务区分开，以提高模块性和复用性。 通过把组件中和视图有关的功能与其他类型的处理分离开，你可以让组件类更加精简、高效。

组件聚焦于展示数据，而把数据访问的职责委托给某个服务。因此服务是实现单一目的的业务逻辑单元，封装了某一特定功能，诸如从服务器获取数据、验证用户输入或直接往控制台中写日志等工作。服务是可以通过注入的方式供他人使用的独立模块。

依赖注入
****************
依赖性注入（Dependency Injection）其实不是Angular独有的概念，这是一个已经存在很长时间的设计模式，也可以叫做控制反转 （Inverse of Control）。 这种模式对于熟悉 Java 和 .Net 的读者不会陌生，Java的Spring框架里的IOC就是一个这样的设计模式。

Angular也提供了依赖注入。因为组件是用TypeScript语言写的类，所以依赖关系通常通过使用构造函数进行注入。在Angular中，可以创建一个可重用的软件对象来处理与服务器的通信，通过构造函数将它注入给每个需要它的对象(类)。然后，在类中就有了与服务器通信的现成方法。

依赖注入的好处是编写一次代码（如处理与服务器的通信服务），在许多地方可以多次使用它。

指令
********
Angular模板是动态的。当Angular渲染它们时，它会根据指令对DOM进行修改。
在Angular中包含以下三种类型的指令：

#. 属性指令：以元素的属性形式来使用的指令。
#. 结构指令：用来改变DOM树的结构
#. 组件：作为指令的一个重要子类，组件本质上可以看作是一个带有模板的指令。

管道
********
Angular管道的作用是把数据作为输入，然后转换它，给出期望的输出。 常见的有日期管道负责转换日期为友好的本地格式，货币管道负责转换货币格式，异步管道实时订阅数据等。

模块
********
Angular的模块作用是把组件、指令、服务等打包成内聚的功能块，封装和暴露相应的功能，从而达到模块间的解耦，高度自治的一种程序设计模式。 换句话说，模块对应的是业务和功能，组件对应的才是页面展示和交互。



Angular的运行
=================
Angular应用程序代码是用TypeScript语言编写的。TypeScript扩展了JavaScript语法，任何已经存在的JavaScript程序，可以不加任何改动，在TypeScript环境下运行。TypeScript只是向JavaScript添加了一些新的遵循ES6规范的语法，以及基于类的面向对象编程的这种特性。

ES6规范是在2015年发布的，而目前所有主流的浏览器并没有完全支持ES6规范，所以用ES6写的东西并不能直接在浏览器中运行。因此，要想使Angular应用程序代码运行在浏览器中，则需要先将TypeScript代码编译为JavaScript代码。

Angular提供了一个Angular CLI命令行界面工具，该工具可用于初始化、开发、构建和维护Angular应用程序。用户可以直接使用Angular CLI工具来构建并运行Angular应用程序。无论是Angular CLI命令，还是TypeScript语言运行环境，都需要在Node.js的环境中运行，因此，我们将会准备一个Node.js的环境。

使用Angular框架开发的应用程序最终是被转换为JavaScript语言代码的应用程序，它能直接运行在Web浏览器中。

小结
===========
本章主要是带领读者对Angular有个主观的认识，分别介绍了什么是Angular，学习Angular的原因，包括它的核心概念以及Angular的版本历史。
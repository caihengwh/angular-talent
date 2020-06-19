.. sectnum::
   :start: 9

#####################
学习Angular模块知识
#####################

模块是包含一个或多个功能的软件组件或程序的一部分。一个或多个独立开发的模块组成一个完整的应用程序。企业级软件应用程序可能包含几个不同的模块，并且每个模块都提供唯一且独立的业务操作。模块之间没有父子关系，只能相互之间引用。模块是组织应用程序和使用使用外部程序库的最佳途径。


什么是Angular模块
=============================
Angular应用是模块化的，它拥有自己的模块化系统，称作NgModule类。一个NgModule类就是一个容器，用于存放一些内聚的代码块，这些代码块专注于某个应用领域、某个工作流或一组紧密相关的功能。它可以包含一些组件、服务或其它代码文件，其作用域由包含它们的NgModule类定义。它还可以导入一些由其它模块中的功能，并导出一些指定的功能供其它NgModule类使用。

Angular模块系统是Angular将代码捆绑成可重用模块的方式，Angular系统代码本身使用此模块系统进行模块化。许多第三方软件为Angular使用模块提供了额外的功能，用户可以轻松地将这些模块包含在应用程序中。

Angular模块是带有@NgModule()装饰器声明的类，Angular模块的主要作用是管理指令、管道、组件。

Angular根模块介绍
********************

每个Angula应用都至少有一个NgModule类，也就是根模块，默认命名为AppModule，并位于一个名叫app.module.ts的文件中。引导这个根模块就可以启动Angular应用。

Angula应用是通过引导根模块AppModule来启动的，引导过程还会创建bootstrap数组中列出的组件，并把它们逐个插入到浏览器的DOM中。
每个被引导的组件都是它自己的组件树的根。插入一个被引导的组件通常触发一系列组件的创建并形成组件树。
虽然也可以在主页面中放多个组件，但是大多数应用只有一个组件树，并且只从一个根组件开始引导。
这个根组件默认为AppComponent，并且位于根模块的bootstrap数组中。

NgModule类是一个带有@NgModule()装饰器的类，也称为Angular模块。
NgModule类把组件、指令和管道打包成内聚的功能块，每个模块聚焦于一个特性区域、业务领域、工作流或通用工具。
模块还可以把服务加到应用中。这些服务可能是内部开发的（比如用户自己写的），或者来自外部的（比如Angular的路由和HTTP客户端）。

@NgModule()装饰器是一个函数，它接受一个元数据对象，该对象的属性用来描述这个模块。其中最重要的属性如下：

* declarations属性：把属于该模块的组件、指令或管道定义在这个属性中，属性列表中的内容一般都是是用户自己创建的。
* exports属性：导出某些类，以便其他的组件模块可以使用它们；
* imports属性：导入其他模块，导入的模块都是使用@NgModule装饰器声明的。如Angular内部模块BrowserModule或第三方的NgModule类；
* providers属性：把属于应用程序级提供服务的提供商定义在这个属性中，提供商负责创建对应的服务，以便应用中的任何组件都能使用它；
* bootstrap属性：应用的主视图，称为根组件。只有根模块才应该设置这个bootstrap属性。

我们打开上一章中[示例 directive-ex600] 中的AppModule模块类代码，对照上面的描述来理解这些元数据：

 .. code-block:: TypeScript

   import { BrowserModule } from '@angular/platform-browser';
   import { NgModule } from '@angular/core';

   import { AppComponent } from './app.component';
   import { SizerDirective } from './sizer.directive'; // 使用Angular CLI命令添加指令时，自动导入

   @NgModule({
   declarations: [
      AppComponent,
      SizerDirective // 使用Angular CLI命令添加指令时，自动添加
   ],
   imports: [
      BrowserModule
   ],
   providers: [],
   bootstrap: [AppComponent]
   })
   export class AppModule { }


[示例 directive-ex600] 中的AppModule模块类中，完成了以下内容：

* @NgModule()装饰器声明在AppModule类上，它接受一个元数据对象；
* declarations属性包含了AppComponent类（根组件类）和SizerDirective类（用户自定义创建的指令类）；
* imports属性中包含了Angular的内部BrowserModule模块，该模块也是一个NgModule类；
* AppModule是根模块，同时也是启动模块，因此bootstrap属性中包含的是启动根组件AppComponent；
* AppModule类是初始化工程时自动生成的，其中@NgModule()装饰器包含的元数据信息是在使用Angular CLI命令时，Angular自动将其放入对应的属性列表中的，如使用Angular CLI命令（ ``ng g d sizer`` ）添加指令时，它会自动导入指令并将其添加到declarations属性列表中；


Angular特性模块介绍
************************
Angular中除了根模块外，其他模块从技术上来讲都是特性模块。特性模块是用来对代码进行组织的模块。随着应用的增长，可能需要组织与特定应用有关的代码，这将需要把特性划出清晰的边界。使用特性模块，可以把与特定的功能或特性有关的代码从其它代码中分离出来，为应用勾勒出清晰的边界，有助于开发人员之间、团队之间的协作，有助于分离各个指令，并帮助管理根模块的大小。

特性模块提供了聚焦于特定应用需求的一组功能，比如用户工作流、路由或表单。虽然也可以用根模块做完所有事情，不过特性模块可以把应用划分成一些聚焦的功能区。特性模块通过它提供的服务以及共享出的组件、指令和管道来与根模块和其它模块合作。

特性模块具有以下特征：

* 与根模块一样，特性模块必须在declarations属性列表中声明所需的所有组件，指令和管道；
* 特性模块不需要导入BrowserModule模块，而一般导入CommonModule模块，该模块包含Angular的通用指令，例如ngIf，ngFor，ngClass；
* 特性模块也不需要配置bootstrap属性。

根模块是初始化工程时自动生成的，特性模块可以使用如下命令创建：

 .. code-block:: sh

   ng g module name # 创建特性模块
   ng g module name --routing # 创建带路由的特性模块

.. admonition:: 提示

   关于路由的知识，本书后续章节将会进行详细介绍。



常用内置模块介绍
============================
上面我们介绍@NgModule()装饰器中的imports属性，里面包含了BrowserModule模块，它是Angular内置模块，下面例举了一些常用的Angular内置模块，见表9-1所示：

.. list-table:: 表9-1 常用的Angular内置模块
   :widths: 8 10 20
   :header-rows: 1

   * - NgModule类
     - 导入包的路径
     - 模块介绍
   * - BrowserModule
     - @angular/platform-browser
     - 默认导入，这是在浏览器中运行该应用程序所必需的
   * - CommonModule
     - @angular/common
     - 包含内置指令，如：NgIf和NgFor等
   * - FormsModule
     - @angular/forms
     - 当要构建模板驱动表单时（它包含 NgModel ）
   * - ReactiveFormsModule
     - @angular/forms
     - 当要构建响应式表单时
   * - RouterModule
     - @angular/router
     - 要使用路由功能，并且要用到 RouterLink的forRoot()和forChild()方法
   * - HttpClientModule
     - @angular/common/http
     - 当需要访问HTTP服务时

无论是根模块还是特性模块，其实都可以引用这些内置模块。换句话讲，表9-1所示的模块，根据需要都可以导入到@NgModule()装饰器中的imports属性列表中。


Angular模块业务上的分类
=========================
如前所述，从技术上来讲，Angular模块中除了根模块外，就是特性模块。从用户角度来讲，AppModule是系统默认生成的，而特性模块是由用户在开发过程中逐个的增加的。随着应用程序的增长，可以将根模块重构为代表相关功能集合的特性模块。然后，将这些特性模块导入到根模块中。

在实际开发过程中，我们常常遇到这样的场景：

  项目刚开始起步时，应用程序功能单一，代码简单，默认使用一个根模块是可行的，随着功能数量的增长，用户创建的组件，指令和管道越来越多，由于它们必须要被导入到根模块中，因此根模块会越来越大，代码开始变得混乱，难以阅读。这时，用户必然而然的想到，创建一些特性模块。随着特性模块的增加，不同的功能之间没有明确的界限，这不仅难以理解应用程序的结构，而且对团队承担不同的责任变得更加困难。代码的冗余，功能的重复，组件的命名冲突等这些问题的存在，使得项目越来越难以维护。


一般解决上述问题的策略是：不仅从技术上将Angular模块分类，还需要从业务上对模块进一步分割。这里引出了常规业务上分类原则：Angular模块从业务上可以分为根模块（AppModule）、核心模块（CoreModule）、共享模块（SharedModule）和其他特性模块。

理解核心模块
**************
核心模块（CoreModule）的定位是应该仅包含服务，并且仅被根模块AppModule导入。从技术角度分析核心模块，它遵循下面的准则：

#. 核心模块中包含使用应用程序启动时加载的单例服务（全局中仅存在一个实例的服务）；
#. 核心模块是仅在AppModule中导入一次，而在其他模块中不再导入的模块；
#. 核心模块中@NgModule()装饰器的declarations属性列表和exports属性列表均保持为空。

核心模块是仅在AppModule中导入一次，因此核心模块中最清晰的是放置全局的HTTP服务，这样做的目的是确保在整个应用程序中仅创建这些服务的一个实例。

关于Angular中的单例服务是这么定义的：把该服务包含在 AppModule 或某个只会被 AppModule 导入的模块中。而核心模块的定义就是仅被根模块AppModule导入的，因此定义在核心模块中的服务就是单例服务。

在开发实践中，有些全局性的类服务也需要放置在核心模块中。比如，有一个UserModule（用户模块），其中包含SignUpService（注册服务），SignInService（登录服务），SocialAuthService（认证服务）和UserProfileService（查询个人信息服务）之类的服务。如果在核心模块中导入UserModule，那么UserModule的所有服务都将在整个应用程序范围内可用。根据上面的准则，UserModule不应该具有声明（declarations）或导出（exports），而应该只有providers（服务提供者）。

综上所述，核心模块可以称之为核心服务模块。

防止重复导入模块
****************************
只有根模块AppModule才能导入核心模块（CoreModule）。如果一个其他特性模块也导入了它，该应用就会为服务生成多个实例。
要想防止其他特性模块重复导入核心模块，可以在该核心模块中添加如下的构造函数：

 .. code-block:: TypeScript

  constructor (@Optional() @SkipSelf() parentModule: CoreModule) {
    if (parentModule) {
      throw new Error(
        'CoreModule已加载过了，它仅可以被导入AppModule');
    }
  }

上述代码中的CoreModule替换为具体的核心模块。该构造函数要求Angular把 CoreModule注入它自己。如果Angular在当前注入器中查找CoreModule，这次注入就会导致死循环，但是@SkipSelf() 装饰器的意思是：在注入器树中层次高于自己的注入器中查找CoreModule。

正常情况下，该核心模块是第一次被导入AppModule中并加载，肯定找不到任何已经注册过的CoreModule实例，默认情况下，当注入器找不到服务时，会抛出一个错误。 但@Optional() 装饰器表示找不到该服务也无所谓。于是注入器会返回 null，parentModule参数也就被赋成了空值，而构造函数中的if()方法就不会执行。
如果在AppModule层次中找到了CoreModule实例，那么parentModule对象为true，接着就会抛出一个错误信息。



理解共享模块
****************
创建共享模块（SharedModule）的目的是为了更好地组织和梳理代码。可以把常用的指令、管道和组件放进共享模块中，然后在应用中其它需要这些的地方导入该模块。从技术角度分析共享模块，它遵循下面的准则：

#. 把在应用程序中各处重复使用的组件，指令和管道集中放进一个SharedModule。此模块应完全由声明组成，并且其中大多数被重新导出。
#. SharedModule可能会重新导出Widget小部件（可以理解为简单的组件，指令和管道的组合），例如CommonModule，FormsModule和其他的UI模块。
#. SharedModule不应该具有providers（服务提供者）。它的任何导入或再导出模块都不应具有providers。
#. SharedModule仅被需要的特性模块导入，包括在应用程序启动时加载的模块和以后懒加载的模块。

例如，有一个UIModule模块，其中包含ButtonComponent组件, NavComponent组件, SlideshowComponent组件, HighlightLinkDirective指令和CtaPipe管道。根据上面的规则，UIModule模块中包含的组件、指令和管道需要再次导出，然后在需要使用它的特性模块中导入UIModule模块，就可以使用其中的一个或者全部 的Widget小部件。

简单的说，共享模块里面仅包含Widget小部件，在被特性模块导入后，可以直接在特性模块中使用这些Widget小部件。


如何正确的分割模块
=========================
模块化的关键问题是如何分割模块和如何设计系统的模块结构。为简单起见，CoreModule是Service模块，而SharedModule是Widget模块。CoreModule只在AppModule中导入一次，SharedModule在需要它的所有特性模块中导入。

在实际开发过程中，经常看到用户对CoreModule和SharedModule的名字产生困惑，他们经常会认为程序核心的所有内容（比如用户信息、页面导航栏、Header头，页脚等）都应该进入CoreModule，而跨多个特性模块且共享的所有服务都将进入SharedModule。这实际上是不对的，并且会产生误导，因为所有服务都是“天生”在所有模块之间共享的，并且SharedModule中不应包含任何服务，虽然页面导航栏功能是应用程序核心的一部分，但是在CoreModule中确实不应包含任何组件。我们只需记住在Angular中服务是通过依赖注入的方式共享的，SharedModule中不应包含任何服务。

在实践过程中，准则有时候并不是万能的，在分割模块时，如果没有充分的理由打破上面的准则，那么无论如何建议读者遵循上面的建议。

小结
==============

本章涵盖了模块的相关知识，从根模块到特性模块，然后从实践经验的角度阐述了核心模块和共享模块的区别，最后总结了如何正确的分割模块的经验。其实关于模块的知识还有几个重点的概念，比如什么是延迟加载模块，为什么要延迟加载模块等，这些知识需要我们了解路由知识后，才能更好的理解他们。
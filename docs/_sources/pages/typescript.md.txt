.. sectnum::
   :start: 4

########################
学习TypeScript知识
########################

Angular是一个完全用TypeScript构建的现代框架，而且还将TypeScript作为其主要开发语言。本章带领读者学习入门TypeScript的知识。

什么是TypeScript
======================
学习TypeScript之前，我们先了解下JavaScript语言的发展历史。ECMAScript是一种由ECMA的组织（前身为欧洲计算机制造商协会）于1997年发布的MCMA-262的标准，该标准制定了MCMAscript语言规范(ECMAScript Language Specification)，简称ES规范。ES规范里包含了多种语言技术，JavaScript语言是其中的一种，也是最出名的一种。

2015年6月，ES规范发展到了第6个版本，即ECMAScript 2015（ES2015），人们通俗的称之为“ES6”。在这之前的版本是“ES5”，也就是大家熟知的JavaScript。ES6相比ES5来讲，增强了很多新特性。

TypeScript是由Microsoft开发的一种开源编程语言，它是“ES6”的一个超集，扩展了“ES6”的语法。TypeScript的第一个版本于2012年10月发布，经历了多次更新后，现在已成为前端社区中不可忽视的力量，不仅在Microsoft内部得到广泛运用，而且Google的Angular也使用了TypeScript作为主要开发语言。这意味着，TypeScript背后至少有Microsoft和Google两大公司的支持。

TypeScript代码文件约定以“.ts”后缀命名。现如今支持ES6的浏览器还比较少，更不用说TypeScript了。这个问题需要用转译器来解决，TypeScript转译器能把TypeScript代码转换为几乎所有浏览器都支持的“ES5”代码。关于TypeScript、ES5和ES6他们之间的关系如图4-1所示：


 .. image:: images/Figure4-1.png
   :width: 250px

图4-1 TypeScript、ES5和ES6之间的关系

TypeScript是ES6的超集，所有的ES6代码都是完全有效且可编译的TypeScript代码。我们把TypeScript 与 JavaScript 常见的区别整理在一张表中，方便读者比较。详细见表4-1所示：

 .. list-table:: 表4-1 TypeScript与JavaScript的区别
   :widths: 5 5

   * - TypeScript
     - JavaScript
   * - JavaScript 的超集，用于解决大型项目的代码复杂性
     - 一种脚本语言，用于创建动态网页
   * - 可以在编译期间发现并纠正错误
     - 作为一种解释型语言，只能在运行时发现错误
   * - 强类型，支持静态和动态类型
     - 弱类型，没有静态类型选项
   * - 最终编译成 JavaScript 代码
     - 可以直接在浏览器中使用
   * - 支持模块、泛型和接口
     - 不支持模块，泛型或接口
   * - 支持 ES5 和 ES6 等
     - 不支持 ES5 和 ES6 等

另外，TypeScript为JavaScript语言提供了更多功能。TypeScript可以与大多数使用的JavaScript库很好地集成。

JavaScript程序也是一个有效的TypeScript程序，一个TypeScript程序可以无缝地使用JavaScript代码，因此可以直接将扩展名为“.js”的文件更改为“.ts”文件。

进行Angular开发时，开发人员不需要单独下载或安装TypeScript，Angular工程已经自动为项目设置成了TypeScript的语言环境。


快速上手TypeScript
==========================

快速上手TypeScript之前，需要先学会如何安装它，稍后需要准备一个能编辑和解析TypeScript语言的环境。在安装完成TypeScript之后，就可以使用它的转译器工具（tsc）将TypeScript代码转译为JavaScript代码了。

安装TypeScript
***********************

使用npm包管理工具下载TypeScript包并在全局环境下安装，命令如下::

    npm install -g typescript

安装成功后，可以通过 tsc -v 命令查看当前TypeScript版本。

::

    $ tsc -v
    Version 3.7.5

上述命令执行结果表示：当前TypeScript的版本是：3.7.5。


转译TypeScript
***********************
通过tsc命令转译TypeScript源码，tsc命令后接TypeScript文件名，如：

::

    tsc hello.ts

上述命令执行完成后，将会在当前目录下生成一个同文件名的JavaScript文件：hello.js。

下面通过示例演示搭建一个简单的TypeScript工程的开发环境。

[示例 tsc-ex100] 开始第一个TypeScript工程示例
************************************************************

1. 打开终端，新建一个文件夹并进入目录下，运行 npm init 命令初始化Node项目：

 .. code-block:: sh

    $ mkdir mytsc && cd mytsc # 新建mytsc文件夹，并进入文件夹目录中
    $ npm init # 初始化项目

 npm init 命令将会要求按照提示输入项目的一些相关信息，也可以使用 ``npm init -y`` 命令默认选择信息，这些信息后续可以在项目中更新。

2. 添加Typescript开发依赖包，执行命令：

 .. code-block:: sh

    $ npm i typescript --save-dev

3. 初始化Typescript项目信息，执行命令：

 .. code-block:: sh

    $ tsc --init

 上述命令执行完成后，在工程目录下，将会生成一个tsconfig.json文件，该文件用来配置Typescript的环境信息。

4. 打开VSCode编辑器，导入工程，或者直接在终端输入命令“code .”，该命令会直接打开VSCode编辑器，并自动导入当前工程。
5. 编辑tsconfig.json文件，找到outDir和rootDir节点注释，并修改内容如下：

 .. code-block:: TypeScript

    "outDir": "dist",     // 定义输出文件夹路径
    "rootDir": "src",     // 定义源文件夹路径

6. 创建一个TypeScript类文件。创建src/index.ts文件，并修改内容如下：

 .. code-block:: TypeScript

    function greeter(name: string) { // 定义变量name的类型为字符串类型
        return "Hello, " + name;
    }

    let user = "Murphy"; // let 是TypeScript语言的变量声明符

    console.log(greeter(user))

7. 编辑package.json文件，并修改内容如下：

 .. code-block:: json

    {
    "name": "mytsc",
    "version": "1.0.0",
    "description": "",
    "main": "src/index.ts",
    "scripts": {
        "start": "tsc",
        "test": "echo \"Error: no test specified\" && exit 1"
        },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "devDependencies": {
        "typescript": "^3.8.3"
        }
    }



8. 编译TypeScript类文件，执行start命令：

 .. code-block:: sh

    $ npm start

 上述命令执行完成后，在工程dist目录下，将会生成一个index.js文件。

9. 执行JavaScript类文件，命令如下：

 .. code-block:: sh

    node dist/hello.js

 上述命令执行完成后，控制台将打印信息：”Hello, Murphy“。

在上面的示例tsc-ex100中，完成了以下内容：

1. 步骤1中，完成了创建及初始化Node.js工程；
2. 步骤2中，完成了初始化TypeScript开发环境，TypeScript开发环境依赖Node.js；
3. 步骤5中，通过Typescript的配置文件配置了源代码和编译后输出文件的路径；
4. 步骤7中，在项目的配置文件中，增加了start的执行命令，该命令执行tsc命令，将TypeScript类文件编译后生成JavaScript类文件；
5. 对比源文件hello.ts和目标文件hello.js的内容：

    * 生成的目标文件中自动添加了"use strict"声明， "use strict"的目的是指定代码在严格条件下执行；
    * 源文件中的TypeScript类型声明自动在目标文件中省略了；
    * 源文件user变量的修饰符在生成的文件中由let变成了var声明；
    * 生成后的hello.js文件比源文件少了空行。

生成的hello.js文件，可以在任何JavaScript环境下运行了。


认识TypeScript的数据类型
============================
通常，可以把数据类型看作是是一组值的集合。TypeScript是ES5(JavaScript)的超集，意味着，ES5中的类型在TypeScript中同样有效。
如上所述，TypeScript是强类型的语言，支持静态和动态类型，而且数据类型这块的知识是掌握TypeScript知识的基础，对初学者来说至关重要，下面对这些内容进行详细讲解。

TypeScript的类型注解
***************************
TypeScript是类型化语言，因此可以通过类型注解指定变量（也可以是函数参数或对象属性等）的类型。
类型注解在TypeScript中不是强制的。它的作用是帮助编译器检查变量的类型，并在处理数据类型时避免错误，也就是描述变量可以接受的值。

类型注解的格式是变量之后使用冒号进行类型标识（:type），冒号和变量名之间可以有空格。例如以下代码告诉TypeScript变量“age”只能存储数字：

 .. code-block:: TypeScript

    let age: number;


.. admonition:: 注意

    类型注解是TypeScript语言的特性，因此，上面的代码仅在支持TypeScript的环境下运行，不能在JavaScript下运行。

下面的代码演示的是对函数参数进行类型注解：

 .. code-block:: TypeScript

    display(id: number, name: string) {  
        console.log("Id = " + id + ", Name = " + name);  
    }  

    display(1, "Murphy"); 


如果声明变量时没有使用类型注解，TypeScript可以对变量的类型进行推断。例如下面这行代码：

 .. code-block:: TypeScript

    let a = 11;

TypeScript会推断出变量a的类型是数值类型（number）。

TypeScript有哪些基础类型
***************************

在TypeScript中主要有以下几种类型：

* 布尔基本类型（boolean）（小写字母b）：具有两个元素true和false的集合。

 .. code-block:: TypeScript

    let a: boolean = false;
    // ES5：var a = false;

* 布尔对象类型（Boolean）（大写字母B）：布尔对象类型是布尔基本类型的包装对象。

 .. code-block:: TypeScript

    // 使用Boolean()函数将字符串转换为布尔值。 
    let hi: Boolean = Boolean('Hi'); 

    console.log(hi); // 由于字符串不为空，因此返回true。控制台打印：true
    console.log(typeof hi); // 控制台打印：boolean

    let hi = new Boolean(false);

    console.log(hi); // 控制台打印：[Boolean: false]
    console.log(typeof hi); // 控制台打印：object

* 数值类型（number）：所有数字的集合。

 .. code-block:: TypeScript

    let count: number = 5;
    // ES5：var count = 5;

* 字符串类型（string）：所有字符串的集合。

 .. code-block:: TypeScript

    let name: string = "Murphy";
    // ES5：var name = "Murphy";

* 数组类型（array）：存储一系列的值的集合。

 .. code-block:: TypeScript

    let list: number[] = [1, 2, 3];
    // ES5：var list = [1, 2, 3];

* 枚举类型：所有对象的集合（包括函数和数组）。

 .. code-block:: TypeScript

    // 常量枚举
    enum Color { Red, Green, Blue } // 等同于：enum Color { Red = 1, Green, Blue }
    let c: Color = Color.Green;
    console.log(c); // 控制台打印：1

    let colorName: string = Color[2];
    console.log(colorName); // 控制台打印：Blue

    // 字符串枚举
    enum Color { Red = 'Red', Green = 'Green', Blue = 'Blue' }
    let c: Color = Color.Green;
    console.log(c); // 控制台打印：Green

    let colorName: string = Color['Green'];
    console.log(colorName); // 控制台打印：Green

* object类型（小写字母o）：所有对象的集合，它用于表示非原始类型，包括函数和数组，不包含number，string，boolean，bigint，symbol，null和undefined类型。

 .. code-block:: TypeScript

    declare function create(o: object | null): void;

    create({ prop: 0 }); // 正确
    create(null); // 正确
    create({}); // 正确

    create(42); // 错误
    create("string"); // 错误
    create(false); // 错误
    create(undefined); // 错误

* Object类型（大写字母O）：Object类型是所有类的实例的类型。

* {}类型：表示空对象类型。

 .. code-block:: TypeScript

    // 在ES5的环境下，下面的代码是正常的：
    const es = {}; 
    es.x = 3; 
    es.y = 4;
    console.log(es) // 控制台打印：{ x: 3, y: 4 }

 然而，在TypeScript的环境下，上面的代码是错误的，将会提示下面的错误信息：

 .. code-block:: TypeScript

    const es = {}; // (A)
    // Property 'x' does not exist on type '{}'
    es.x = 3; // Error
    // Property 'y' does not exist on type '{}'
    es.y = 4; // Error

 在TypeScript的环境下，我们需要显示的初始化es对象，例如下面代码这样：

 .. code-block:: TypeScript

    const es = {
        x: 3,
        y: 4,
    };
    es.x = 5;
    console.log(es) // 控制台打印：{ x: 5, y: 4 }

* null和undefined类型：具有唯一元素“null”或“undefined”的集合。

 .. code-block:: TypeScript

    let u: undefined = undefined;
    let n: null = null;

* any类型：任意值类型，存储任意值的集合。TypeScript 允许对 any 类型的值执行任何操作，而无需事先执行任何形式的检查，例如下面的代码：

 .. code-block:: TypeScript

    let value: any;

    value.foo.bar; // OK
    value.trim(); // OK
    value(); // OK
    new value(); // OK
    value[0][1]; // OK

 在许多场景下，这太宽松了。使用 any 类型，可以很容易地编写类型正确但在运行时有问题的代码。如果使用 any 类型，就无法使用 TypeScript 提供的大量的保护机制。为了解决 any 带来的问题，TypeScript 3.0+ 引入了 unknown 类型。

* unknown类型：同any类型一样，所有类型也都可以赋值给 unknown类型，但是对类型为 unknown 的值执行操作时，会发生编译错误：

 .. code-block:: TypeScript

    let value: unknown;

    value.foo.bar; // Error
    value.trim(); // Error
    value(); // Error
    new value(); // Error
    value[0][1]; // Error

 unknown 类型只能被赋值给 any 类型和 unknown 类型本身，它一般用在那些将取得任意值，但不知类型的地方。如下面方法中的参数，request是来自客户端的请求，我们不清楚它具体的类型：
 
 .. code-block:: TypeScript

    intercept(request: HttpRequest<unknown>, next: HttpHandler): Observable<HttpEvent<unknown>> {
        console.log(JSON.stringify(request)); // 打印请求信息
        return next.handle(request);
    }

 
 使用unknown类型将会进行类型检查，因此它相比any类型更加安全。在没有先断言或指定到更具体类型的情况下，不允许对 unknown 进行任何操作。

 .. code-block:: TypeScript

    const value: unknown = "Murphy";
    const onevalue: string = value as string; // 定义类型断言
    const otherString = onevalue.toUpperCase();
    console.log(otherString); // 控制台打印： "MURPHY"

* never类型：当函数永远不会返回值时，可以用never类型表示。

 .. code-block:: TypeScript

    // 抛出异常
    function error(message: string): never {
    throw new Error(message);
    }

    // 永远不能返回
    function continuousProcess(): never {
    while (true) {
        // ...
    }
    }


* 元组（Tuple）类型：类似数组的结构，里面存储不同类型的值。

 .. code-block:: TypeScript

    let tupleType: [string, boolean];
    tupleType = ["Murphy", true];
    console.log(tupleType[0]); // 控制台打印： Murphy
    console.log(tupleType[1]); // 控制台打印： true


* void类型：与 any 类型相反，它表示没有任何类型。

 .. code-block:: TypeScript

    // 声明函数返回值为 void 类型
    function getName(): void {
        console.log("This is my name");
    }



TypeScript中的类型转换
************************
类型转换是将一种数据类型转换为另一种数据类型的能力。TypeScript提供了执行类型转换的内置函数。

字符串转换为数字
-----------------
TypeScript中常见的有4种方式，将字符串转换为数字：

#. 使用Number()方法；
#. 使用parseInt()方法；
#. 使用parseFloat()方法；
#. 使用“+”号操作符。

TypeScript提供了Number函数来将数字格式的字符串转换为number型：

 .. code-block:: TypeScript

    let str: string = '10';

    let a = Number(str)
    console.log(typeof a, a);  // 控制台打印：number 10

    let b = parseInt(str)
    console.log(typeof b, b);  // 控制台打印：number 10

    let c = parseFloat(str)
    console.log(typeof c, c);  // 控制台打印：number 10

    let d = +str
    console.log(typeof d, d);  // 控制台打印：number 10



数字转换为字符串
-----------------
TypeScript中常见的有3种方式，将数字转换为字符串：

#. 附加空字符串的方式；
#. 使用toString()方法；
#. 使用String()方法。

 .. code-block:: TypeScript

    let num: number = 10;
    console.log(num.toString());  // 控制台打印：10
    console.log(num.toString(2));  // 控制台打印2进制数值格式的字符串：1010
    console.log(num.toString(8)); // 控制台打印8进制数值格式的字符串：12

    let a = num + ''
    console.log(typeof a, a);  // 控制台打印：string 10

    let b = String(num)
    console.log(typeof b, b);  // 控制台打印：string 10



什么是TypeScript类型断言
***************************
TypeScript 允许我们以任何方式覆盖原先推断的类型。当我们比编译器本身能更好地理解变量类型时，通过类型断言这种方式可以告诉编译器确定的类型。类型断言好比其他语言里的类型转换，但是不进行特殊的数据检查和解构。它没有运行时的影响，只是在编译阶段起作用。

类型断言有两种形式： “<>” 尖括号语法和“as”语法。

 .. code-block:: TypeScript

    let one: any = "this is a string";
    let two: number = (<string>one).length;
    let three: number = (one as string).length;

假设，我们想定义一个{}类型对象，但是事先不清楚该对象里面的元素类型：

 .. code-block:: TypeScript

    const friend = {};
    friend.name = 'Murphy';  // Error! Property 'name' does not exist on type '{}'.ts(2339)

    interface Person {
    name: string;
    age: number;
    }

    const person = {} as Person;
    person.name = 'Murphy';  // 正确

什么是TypeScript类型保护
***************************
类型保护是可执行运行时检查的一种表达式，用于确保该类型在一定的范围内。换句话说，类型保护可以保证一个字符串是一个string类型，尽管它的值也可以是一个数值。目前主要有3种的方式来实现类型保护：

使用 in 操作符进行类型保护
-------------------------------
in 操作符会检查一个属性在某对象上是否存在，例如下面的示例代码，将检查person对象里面是否有name属性：

 .. code-block:: TypeScript

    interface Person {
        name: string;
        age: number;
    }
    const person: Person = {
        name: 'Murphy',
        age: 37,
    };

    const checkForName = 'name' in person; // 检查person对象里面是否有name属性，返回 true


使用 typeof 操作符进行类型保护
-------------------------------
typeof操作符返回一个字符串，表示接收参数的类型。当将typeof操作符接收包含布尔值的变量时，将获得字符串“boolean”，如下例所示：

 .. code-block:: TypeScript

    let isPending = false;
    let isDone = true;

    console.log(typeof isPending); //  控制台打印：boolean
    console.log(typeof isDone); // 控制台打印：boolean

使用 instanceof 操作符进行类型保护
-----------------------------------------------
JavaScript中还有个instanceof操作符也可以用来识别对象类型，如接收的参数是指定的对象类型，则返回true，反之返回false。下面示例用instanceof操作符来识别变量类型：

 .. code-block:: TypeScript

    console.log(foo instanceof Boolean); // 控制台打印：false
    console.log(object instanceof Boolean); // 控制台打印：true



什么是TypeScript的联合类型
************************************************

联合类型（Union Types）表示取值可以为多种类型中的一种。有时候期望变量是几种类型之中的一种，要描述这些变量，可以使用联合类型。例如，在下面的代码中，变量a即可以是null类型也可以是number类型：

 .. code-block:: TypeScript

    let a: null|number = null;
    a = 11;

当TypeScript不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性或方法：

 .. code-block:: TypeScript

    let a: string | number  = 'hello';
    console.log(a.toString()); // toString()方法是联合类型变量a的公共方法

什么是TypeScript的类型别名
************************************************

类型别名用来给一个类型起个新名字。例如下面的代码，我们给联合类型重新定义了别名“Message”：

 .. code-block:: TypeScript

    type Message = string | string[]; 

    let greet = (message: Message) => {
        // ...
    };

什么是TypeScript的交叉类型
************************************************
交叉类型是将多种类型叠加到一起成为一种类型，新类型包含了所需的所有类型的特征。例如下面的代码，我们将Person和Worker类型叠加到一起成为一种新的Employee类型：

 .. code-block:: TypeScript

    interface Person {
        name: string;
        age: number;
    }
    interface Worker {
        companyId: string;
    }
    type Employee = Person & Worker;

    const staff: Employee = {
        id: '010100',
        age: 37,
        companyId: 'ABC'
    };

 上述代码中，定义staff对象的类似为Employee类型，这样staff对象同时拥有Person和Worker类型的特征。

学习TypeScript的函数
=============================

TypeScript中的函数知识包含这些内容：箭头函数、函数类型、可选参数、默认参数和剩余参数。下面我们对它们进行分别介绍。


什么是箭头函数
***************************

ES6中箭头函数的基本语法如下：

 .. code-block:: TypeScript

    let func = value => value;
    // 或对参数进行类型声明
    let func = (value: string) => value;

上面的代码相当于：

 .. code-block:: TypeScript

    let func = function (value) {
        return value
    }

如果需要传入多个参数：

 .. code-block:: TypeScript

    let func = (value1, value2) => value1 * value2;

上面箭头函数例子中都省略了return关键字和花括号，在箭头函数中如果方法体中只有一行代码，可以省略return关键字和方法体的花括号，直接简化成value => value。如果函数的代码块有多条语句，则不能省略return关键字和方法体的花括号：

 .. code-block:: TypeScript

    let func = (value1, value2) => {
        value1 = value1 + 10;
        return value1 * value2;
    }

如果需要返回一个对象，箭头函数的方法体必须放在括号中，这样做的原因是：没有括号，JS引擎没办分区分是正常定义一个对象还是一个箭头函数体：

 .. code-block:: TypeScript

    let func = (value1, value2) => ({ value1: value1, value2: value2 }); //正确写法
    let func = (value1, value2) => { value1: value1, value2: value2 }; //会报错


TypeScript的函数类型
**************************

函数类型也是Object类型的一种。函数类型顾名思义就是对函数进行类型声明。

下面看一个函数类型的例子：

 .. code-block:: TypeScript

    type Add = (name: string) => string; // 定义函数类型
    let myAdd: Add = function (a: string): string {
        return 'Hello' + a;
    }

其中Add就是定义的一种函数类型，它接受一个字符类型参数并且返回值为字符串，myAdd函数使用Add函数类型作为它的类型注解。

上面TypeScript代码经过编译后，生成下面的JavaScript代码：

 .. code-block:: TypeScript

    var myAdd = function (a) {
        return 'Hello' + a;
    };


可以通过下面的方式调用myAdd函数：

 .. code-block:: TypeScript

    myAdd('Murphy');




函数中的可选参数
**************************
标识符后面的问号表示该参数是可选的。例如下面函数中第二个参数lastName就是一个可选参数：

 .. code-block:: TypeScript

    //这是一个拼接名字的函数：
    function buildName(firstName: string, lastName?: string) {
        if (lastName) {
            return firstName + " " + lastName;
        } else {
            return firstName;
        }
    }

    let result = buildName("Murphy");  // 省略了第2个参数
    console.log(result); // 控制台打印：Murphy

由于lastName参数是可选的，因此调用buildName函数时，可以选择传1个或者2个参数。

函数中的默认参数
*************************
默认参数的作用使用户调用函数时，可以选择忽略该参数。TypeScript会将添加了默认值的参数识别为可选参数。
默认参数通常可以省略类型注解，TypeScript会主动为我们推断出参数的类型，也就是默认值的类型，如果给可选参数传值就必须对应默认参数的类型。

下面的示例里，在定义函数类型时，给参数b设定了默认参数：'China'：

 .. code-block:: TypeScript

    function hi (a: string, b = 'China'): void{
        console.log(a + ':' + b);
    }
    hi('hello'); // 控制台打印：hello:China


函数中的剩余参数
******************************
当函数中最后一个参数是数组时，可以使用 ...rest 的方式设置函数中的剩余参数（rest参数）：

 .. code-block:: TypeScript

    function push(array, ...items) {
        items.forEach(function (item) {
            array.push(item);
        });
    }

    let arr = [];
    push(arr, 5, 6, 7);


上面示例中，items 是一个数组，因此可以用数组的类型来定义它：

 .. code-block:: TypeScript

    function push(array, ...items: number[]) {
        items.forEach(function (item) {
            array.push(item);
        });
    }


学习TypeScript的数组
======================
学习TypeScript的数组知识，我们从定义数组类型开始，然后介绍几种常见操作数组的方法。

TypeScript的数组类型
**************************
数组类型也是Object类型的一种。TypeScript的数组类型有两种表示形式，一是作为列表形式，另一种是作为元组形式。

数组作为列表时，有两种表示方式：

 .. code-block:: TypeScript

    let arr: number[] = [];
    let arr: Array<number> = [];

上面arr数组中所有元素都具有相同的类型（number类型），数组中的元素个数不固定。
如果元素个数不固定且类型未知，这种情况可直接声明成any类型：

 .. code-block:: TypeScript

    let arr: any[] = [];

如果想让数组中元素的个数固定，可以使用TypeScript的元组类型：

 .. code-block:: TypeScript

    let address: [string, number] = ['Wuhan', 27];


使用TypeScript数组的查找和检索方法
*********************************************************

下面介绍关于数组的2个常用方法：

#. find()方法返回数组元素中的第一个数匹配的值。
#. findIndex()方法返回数组元素中的第一个数匹配的元素的索引。

下面示例查找（返回值）第一个大于18的元素及索引：

 .. code-block:: TypeScript

    const numbers: number[] = [4, 9, 16, 35, 49];
    function myFunction(value, index, array) {
        return value > 18;
    }
    let first = numbers.find(myFunction);
    let firstIndex = numbers.findIndex(myFunction);

    console.log(first); // 控制台打印：35
    console.log(firstIndex); // 控制台打印：3



学习TypeScript接口
=========================
接口是一系列抽象方法的声明，是一些方法特征的集合，这些方法都应该是抽象的，需要由具体的类去实现，然后第三方就可以通过这组抽象方法调用，让具体的类执行具体的方法。

TypeScript接口定义如下：

 .. code-block:: TypeScript

    interface name { 
    }

可以将接口看作是做某事的规范（如以某种方式实现函数）或存储特定数据（如属性、数组）。TypeScript接口可以应用于函数，例如下面的代码：

 .. code-block:: TypeScript

    interface MyFind {
        (source: string, subString: string): boolean;
    }
    let mySearch: MyFind; // 接口应用于函数
    mySearch = function (source: string, subString: string) {
        let result = source.search(subString);
        if (result == -1) {
            return false;
        }
        else {
            return true;
        }
    }

上述示例中：

#. 定义了MyFind接口，该接口里包含了2个字符串参数，接口返回布尔类型（boolean）；
#. mySearch函数的类型注解是MyFind接口。

TypeScript接口也可以应用于属性。接口可以具有强制执行属性，但也可以具有可选属性，例如下面的代码：

 .. code-block:: TypeScript

    interface Clothing {
        label: string;
        size: number;
        color?: string;
    }
    function printLabel(labelled: Clothing) {  // 接口应用于属性
        console.log(labelled.label + " " + labelled.size);
    }
    let myObj = { size: 10, label: "衣服" };
    printLabel(myObj); // 控制台打印：衣服 10

上述示例中：

#. 定义了Clothing接口，该接口里包含了3个属性，其中color是可选属性；
#. printLabel函数中的labelled参数的类型注解是Clothing接口；
#. 定义了myObj对象，该对象里的属性名与Clothing接口中的属性名一致；
#. 调用printLabel函数时，传递的是myObj对象参数。

Typescript接口也可以应用于数组，例如下面的代码：

 .. code-block:: TypeScript

    interface StringArray {
        [index: number]: string;
    }
    let myArray: StringArray;
    myArray = ["Murphy", "Jack"];

上述示例中：

#. 定义了StringArray接口，该接口表示索引的类型是数字，值的类型是字符串；
#. 定义了myArray变量，该变量的类型注解是StringArray接口；

学习TypeScript类
=========================
类和接口一样，都是是一堆抽象概念的集合，他们描述的是对象相关的属性和方法。可以从类中创建出享有共同属性和方法的实例对象。类与接口的区别在于：接口仅提供描述，并不提供具体创建此对象实例的方法。

TypeScript是面向对象的语言，面向对象主要有3大特性：封装、继承、多态。从ES6开始，JavaScript程序员将能够使用基于类的面向对象的方式。TypeScript支持面向对象的所有特性，并且编译后的JavaScript可以在所有主流浏览器和平台上运行。

TypeScript 类定义方式如下：

 .. code-block:: TypeScript

    class class_name { 
        // 类作用域
    }

类的构造函数
*****************
TypeScript使用constructor关键字而不是类名来声明构造函数。在constructor方法中可以通过this关键字来访问当前类体中的属性和方法。下面的例子定义了一个Student类：

 .. code-block:: TypeScript

    class Student {  // 定义Student类
        name: string;  // 定义类的属性name
        constructor(myname: string) { // 定义构造函数
            this.name = myname;
        }
        study() { //定义类的方法
            //定义该方法所要实现的功能
        }
    }

构造函数就是在new一个类的时候，调用的方法。如：

 .. code-block:: TypeScript

    let s = new Student('Murphy');

上述代码实际上调用的就是Student类的构造函数，该构造函数接收一个字符串参数。

在TypeScript的类中不写constructor不代表没有constructor，意思是他会有一个默认的空constructor，如下代码：

 .. code-block:: TypeScript

    constructor() {} // 无参数，无实现内容


构造函数还有另一个用途：TypeScript自动将构造函数的参数赋值为属性。即不需要在构造函数中分配实例变量，TypeScript已经完成了此工作。
例如下面的类：

 .. code-block:: TypeScript

    // 推荐写法
    class Person {
        constructor(public firstName: string, public lastName: string) { // 无实现
        }
    }

等同于下面的写法：

 .. code-block:: TypeScript

    class Person {
        public firstName: string;
        public lastName: string;
        constructor(firstName: string, lastName: string) {
            this.firstName = firstName;
            this.lastName = lastName;
        }
    }

类成员属性的访问范围修饰符一般有3种：

* 私有（private）：声明访问范围不能在类的外部访问
* 受保护（protected）：声明访问范围在派生类中仍然可以访问。
* 公共（public）：声明访问范围不做任何限制。

如果类成员属性前无修饰符，TypeScript默认为public修饰符。

类方法和属性
******************
* 方法：JavaScript中，统称函数，但是有了类之后，类中的函数成员，统称方法。
* 只读属性：可以使用readonly关键字将属性设置成只读，相当于类字段的const。如下面的代码：

 .. code-block:: TypeScript

    readonly name: string = 'Murphy'; // name变量只读

* 静态方法：使用static修饰符修饰的方法称为静态方法，它们不需要实例化，而是直接通过类来调用：

 .. code-block:: TypeScript

    class Animal {
        constructor(private name: string) { }
        static isAnimal(animal) {
            return animal instanceof Animal;
        }
    }

    let dog = new Animal('Dog');
    Animal.isAnimal(dog); // 控制台打印：true

* 静态属性：同样，使用static修饰符修饰的属性称为静态属性。


类的继承
************
TypeScript使用extends关键字实现继承，子类中使用super关键字来调用父类的构造函数和方法：

 .. code-block:: TypeScript
 
    class Animal {
        name: string;
        constructor(theName: string) { this.name = theName; }
        move(meters: number = 0) {
            console.log(this.name + " moved " + meters + "m.");
        }
    }

    class Snake extends Animal {
        constructor(name: string) {
            super(name);  // 子类调用父类的constructor方法
        }
        move(meters = 5) {
            console.log("Snake...");
            super.move(meters); // 子类调用父类的move方法
        }
    }

    class Horse extends Animal {
        constructor(name: string) {
            super(name);  // 子类调用父类的constructor方法
        }
        move(meters = 45) {
            console.log("Horse...");
            super.move(meters); // 子类调用父类的move方法
        }
    }

类可以实现接口，TypeScript使用implements关键字实现接口：

 .. code-block:: TypeScript

    interface ClockInterface {
        currentTime: Date;
        setTime(d: Date);
    }

    class Clock implements ClockInterface {
        currentTime: Date;
        setTime(d: Date) {
            this.currentTime = d;
        }
        constructor(h: number, m: number) { }
    }


类的存取器方法
********************
TypeScript提供了get和set方法，俗称存取器方法，存取器方法可以像Java语法那样使用“.”符号进行调用：

 .. code-block:: TypeScript

    class Foo {
        private _bar: boolean = false;
        get bar(): boolean { // get方法
            return this._bar;
        }

        set bar(theBar: boolean) { // set方法
            this._bar = theBar;
        }
    }
    let myFoo = new Foo();
    let myBar = myFoo.bar; // 实际调用的是get bar()方法
    myFoo.bar = true;  // 实际调用的是set bar()方法



学习TypeScript的映射类型
===================================
所谓映射类型，就是通过在属性类型上建立映射，从现有的类型创建出新的类型。已知类型的每个属性都会根据指定的映射类型进行转换。TypeScript 内置了一些常用的映射类型，比如Partial、Required、Readonly、Exclude、Extract、Record 和 ReturnType 等。出于篇幅考虑，本章只简单介绍其中几种映射类型。

Partial 映射类型
*********************
Partial映射类型的作用就是将已知类型的属性全部变为可选项，即在属性后添加“？”操作符。Partial映射类型在源码中的定义如下：

 .. code-block:: TypeScript

    type Partial<T> = {
    [P in keyof T]?: T[P];
    };

在代码中，首先通过 keyof 拿到 T 的所有属性名，然后使用 in 进行遍历，将值赋给 P，最后通过 T[P] 取得相应的属性值。中间的 ? 号，用于将所有属性变为可选。

请看Partial映射类型的示例：

 .. code-block:: TypeScript

    type Person = {
        name: string;
        age: number;
    }

    let murphy: Person = { // 2个属性缺一不可
        name: 'Murphy',
        age: 37
    };

    type PartialPerson = Partial<Person>;

    let partialPerson: PartialPerson = {
        name: 'Murphy' // 属性变为可选项了，因此可以仅有一个属性
    };

上述代码中，使用Partial类型将原先的Person类型映射成了新类型PartialPerson，其里面的属性都成为了可选项。

Readonly (只读)映射类型
***************************
TypeScript 中可以创建只读属性。 Readonly 类型接受一个类型 T，并将其所有属性设置为只读。Readonly映射类型在源码中的定义如下：

 .. code-block:: TypeScript

    type Readonly<T> = {
    readonly [P in keyof T]: T[P];
    };

在代码中，首先通过 keyof 拿到 T 的所有属性名，然后使用 in 进行遍历，将值赋给 P，最后通过 T[P] 取得相应的属性值。前面的readonly关键字，用于将所有属性变为只读。

Exclude映射类型
***************************
Exclude映射类型将某个类型中属于另一个的类型移除掉。Exclude映射类型在源码中的定义如下：

 .. code-block:: TypeScript

    type Exclude<T, U> = T extends U ? never : T;

上述代码的意思是：如果 T 能赋值给 U 类型的话，那么就会返回 never 类型，否则返回 T，最终结果是将 T 中的某些属于 U 的类型移除掉，举个例子：

 .. code-block:: TypeScript

    type T00 = Exclude<"a" | "b" | "c" | "d", "a" | "c" | "f">;  // "b" | "d"

上述代码的意思是把第一组的字符（"a" | "b" | "c" | "d"）属于第二组的字符（"a" | "c" | "f"）移除掉，最后变为了字符（"b" | "d"）。

识别TypeScript的相等性判断
================================
在进行相等性判断时，一般有两种比较符：非严格相等比较和严格相等比较。

非严格相等比较 (==)
********************

在JavaScript里，两个等号的判断会进行隐式的类型转换，如：

 .. code-block:: TypeScript

    console.log(2 == "2"); // 控制台打印：true 
    console.log(0 == ""); // 控制台打印：true

在TypeScript中，因为有了类型声明，因此这两个结果在TypeScript中结果都为false，并且还会在编译阶段有报错。但是null和undefined的相等性判断与JavaScript里相同：

 .. code-block:: TypeScript

    console.log(0 == undefined); // 控制台打印：false
    console.log('' == undefined); // 控制台打印：false
    console.log(false == undefined); // 控制台打印：false
    console.log(undefined == undefined); // 控制台打印：true
    console.log(null == undefined); // 控制台打印：true

在TypeScript中，做相等性判断时要避免两个不同类型的比较，另一方面使用严格相等（===，3个等号）来代替两个等号，保证在编译期和运行期具有相同的语义。

严格相等比较 (===)
********************
严格相等比较操作符比较两个值是否相等，两个被比较的值在比较前都不进行隐式转换。

* 如果两个被比较的值具有不同的类型，这两个值是不严格相等的。
* 如果两个值的类型相同，值也相同，并且都不是number类型时，两个值严格相等。
* 如果两个值都是number类型，当两个都不是NaN，并且数值相同，或是两个值分别为 +0和-0时，两个值被认为是严格相等的。

 .. code-block:: TypeScript

    let num = 0;
    let obj = new String("0");
    let str = "0";

    console.log(num === obj); // 控制台打印：false
    console.log(num === str); // 控制台打印：false
    console.log(obj === str); // 控制台打印：false
    console.log(+0 === -0);   // 控制台打印：true



学习TypeScript析构表达式
===============================
TypeScript析构表达式的作用是：通过表达式将对象和数组拆解成为任意数量的变量。这里对它们进行分别介绍。

对象的析构表达式
******************
对象的析构表达式使用大括号进行拆解。

 .. code-block:: TypeScript

    function getInfo() {
        return {
            myAge: 30,
            myName: 'Murphy'
        }
    }

    let { myAge, myName } = getInfo();
    console.log(myAge); // 控制台打印：30
    console.log(myName); // 控制台打印：Murphy

对象的析构表达式还有一个使用场景，这个场景用在页面返回user信息时，屏蔽掉敏感信息。

 .. code-block:: TypeScript

    const user = await this.usersService.findOne(username);
    if (user && user.password === pass) {
        const { password, ...result } = user; // 将user类的属性进行拆解
        return result;
    }

上述代码中，将user类的属性进行拆解，拆解分为两个变量：password和result，其中password对应user实例中的同名属性的值，result对应其他的所有属性，相当于将user实例中剥离了password属性。


数组的析构表达式
******************
数组的析构表达式使用中括号进行拆解。

 .. code-block:: TypeScript
    
    let array = [1, 2, 3, 4];

    let [number1, number2] = array;
    console.log(number1); // 控制台打印：1
    console.log(number2); // 控制台打印：2

    let [num1, , , num2] = array;
    console.log(num1); // 控制台打印：1
    console.log(num2); // 控制台打印：4

    let [, , num3, num4] = array;
    console.log(num3); // 控制台打印：3
    console.log(num4); // 控制台打印：4


    let [no1, no2, ...others] = array;
    console.log(no1); // 控制台打印：1
    console.log(no2); // 控制台打印：2
    console.log(others); // 控制台打印：[3, 4]

无论对象或者数组的析构表达式，注意的地方就是只能在末尾参数的位置使用rest参数（...操作符）。

TypeScript模块
=================

模块是ES6中引入了的概念，TypeScript也沿用这个概念。模块使用模块加载器去导入其它的模块。 在运行时，模块加载器的作用是在执行此模块代码前去查找并执行这个模块的所有依赖。

模块在其自身的作用域里执行，而不是在全局作用域里；这意味着定义在一个模块里的变量，函数，类等等在模块外部是不可见的，除非你明确地导出（export）它们。 相反，如果想使用其它模块导出的变量，函数，类，接口等的时候，你必须要导入（import）它们。

模块是自声明的；两个模块之间的关系是通过在文件级别上使用imports和exports建立的。TypeScript与ES6一样，任何包含顶级import或者export的文件都被当成一个模块。

导出声明
***********
任何声明（比如变量、函数、方法、类、类型别名或接口）都能够通过添加export关键字来导出。例如下面的代码：

 .. code-block:: TypeScript

    export interface StringValidator { // 导出接口
        isAcceptable(s: string): boolean;
    }
    export const numberRegexp = /^[0-9]+$/;  // 导出常量
    export class ZipCodeValidator implements StringValidator { // 导出类
        isAcceptable(s: string) {
            return s.length === 5 && numberRegexp.test(s);
        }
    }

导出语句
*********
导出语句很便利，也可以对导出的部分重命名，参照下面的示例：

 .. code-block:: TypeScript

    class ZipCodeValidator implements StringValidator {
        isAcceptable(s: string) {
            return s.length === 5 && numberRegexp.test(s);
        }
    }
    export { ZipCodeValidator }; // 导出语句
    export { ZipCodeValidator as mainValidator }; // 导出并重命名

导出语句使export语句集中写在一起，方便阅读，因此这也是常规的写法。

默认导出
***********
每个模块都可以有一个默认导出。 默认导出使用default关键字标记；并且一个模块只能有一个default导出。例如下面代码：

 .. code-block:: TypeScript

    const numberRegexp = /^[0-9]+$/;

    export default function (s: string) { // 默认导出
        return s.length === 5 && numberRegexp.test(s);
    }


导入内容
***********
模块的导入操作与导出一样简单，使用import关键字来导入其它模块中的导出内容。例如下面导入另一个模块中的某个导出内容：

 .. code-block:: TypeScript

    import { ZipCodeValidator } from "./ZipCodeValidator";
    let myValidator = new ZipCodeValidator();


上述代码中，ZipCodeValidator导入的类ZipCodeValidator类与当前类不在一个文件里，它位于当前类的同级目录下。也可以将整个模块导入到一个变量，并通过它来访问模块的导出部分：

 .. code-block:: TypeScript

    import * as validator from "./ZipCodeValidator";
    let myValidator = new validator.ZipCodeValidator();

上述代码中导入了整个文件"./ZipCodeValidator"里面的所有内容，并赋值给变量validator，然后使用“.”操作符访问它里面的内容。

导入默认导出时，import后面导入的内容（类、函数或者其他）是可以省略的。如导入默认的导出内容：

 .. code-block:: TypeScript

    // 导出类，文件名为：abc.ts
    const numberRegexp = /^[0-9]+$/;

    export default function (s: string) { // 默认导出
        return s.length === 5 && numberRegexp.test(s);
    }

    // 导入类
    import validate from "./abc"; // 导入abc模块

    let strings = ["Hello", "98052", "101"];

    // Use function validate
    strings.forEach(s => {
        console.log(`"${s}" ${validate(s) ? " matches" : " does not match"}`);
    });


小结
========
TypeScript发展至今，已经成为大型项目的标配，其提供的静态类型系统，大大增强了代码的可读性以及可维护性；同时，它提供最新和不断发展的JavaScript特性，帮助开发人员快速实现更强大的功能。本章的篇幅较长，主要宗旨是对TypeScript语言的知识进行归纳性的总结。对于初学者，可作为大纲来指导式的学习；对于有经验的读者，可结合本章的内容对现有知识进行查缺补漏。总之，本章提供了一站式的学习TypeScript的捷径。

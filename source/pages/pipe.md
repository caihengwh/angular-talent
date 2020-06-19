.. sectnum::
   :start: 15

##########################
深入学习Angular管道知识
##########################

本书在介绍模板表达式中有哪些运算符时，简单的介绍了管道运算符（简称管道）。管道运算符和指令一样，在Angular中到处存在，管道运算符的作用是对表达式的结果进行一些转换，如，将文本更改为大写，可以格式化日期显示，可以将数字显示为本地货币格式，或过滤列表并对其进行排序等。它们在整个应用程序中获取数据、转换数据，然后把这些数据显示给用户。

Angular管道的使用方法
===============================
Angular包含了一些内置管道，类似指令一样，不需要额外做些操作，只需直接在模板中直接使用它们。请参阅下面的示例：

 .. code-block:: TypeScript

   import { Component } from '@angular/core';

   @Component({
   selector: 'app-root',
   template: `
      <p>当前日期: {{ birthday | date }}  </p>
      <p>指定日期格式: {{ birthday | date:'yyyy-MM-dd' }} </p>
   `,
   styles: []
   })
   export class AppComponent {
   birthday = Date.now();
   }

上述代码中，在组件模板表达式中，管道操作符(“|”)将左侧的birthday的值输入到右侧的date管道函数中，最终界面打印出以下内容：

 .. code-block:: text
   
   当前日期: Apr 24, 2020

   指定日期格式: 2020-04-24


使用Angular的内置管道
=========================
Angular内置了一些管道，比如异步管道、金额管道、百分比管道、JSON管道、大小写转换管道、日期格式转换管道等，下面详细介绍其中的几种。

使用async管道
*************************
async 管道即异步管道，它会订阅一个 Observable 或 Promise值对象，并返回它发出的最近一个值。当新值到来时，async 管道就会把该组件标记为需要进行变更检测。当组件被销毁时，async 管道就会自动取消订阅，以消除潜在的内存泄露问题。async 管道的使用场景，请参阅下面的示例：

 .. code-block:: TypeScript

   import { Component } from '@angular/core';
   import { interval, Observable, of } from 'rxjs';

   @Component({
   selector: 'app-root',
   template: `
      <p>当前日期: {{ currentTime$ | async }}  </p>
      <p>指定日期格式: {{ currentTime$ | async| date:'yyyy-MM-dd HH:MM:ss' }} </p>
   `,
   styles: []
   })
   export class AppComponent {
   birthday = Date.now();
   currentTime$: Observable<number>;

   constructor() {
      interval(1000) // 每1秒依次发出当前时间
         .subscribe(()=>this.currentTime$ = of(Date.now()))
   }

   }

上述代码中，在类的构造方法中通过interval创建符每秒钟发射一个当前的时间戳值，赋值给Observable类型的变量currentTime$，在模板中使用async管道符获取变量currentTime$的最新值，同时使用链式管道转换时间戳值为可读格式的日期。

使用currency管道
*************************
currency管道负责把数字转换成金额，currency管道相对其他管道比较灵活，它可以根据配置项进行灵活的格式化，currency管道的使用语法如下：

 .. code-block:: scss

   {{ 数值表达式 | currency [ : currencyCode [ : display [ : digitsInfo [ : locale ] ] ] ] }}

上述参数详情描述见下表：

 .. list-table:: currency管道配置选项
   :widths: 5 5 20

   * - 选项
     - 类型
     - 说明
   * - currencyCode（可选）
     - string
     - 货币代码，比如：JPY 表示人民币，USD 表示美元（默认）
   * - display（可选）
     - string | boolean
     - | 货币指示器的格式（可选，默认值是symbol），有效值包括下面这些：
       | code：显示货币代码（如 USD）
       | symbol(默认)：显示货币符号（如 $）
       | symbol-narrow：使用区域的窄化符号。比如，加拿大元的符号是 CA$，而其窄化符号是 $。
       | String：使用指定的字符串值代替货币代码或符号。空字符串将会去掉货币代码或符号。
   * - digitsInfo（可选）
     - string
     - | 数字展现的选项，通过格式 {a}.{b}-{c} 的字符串指定：
       | a：小数点前的最小位数，默认为 1
       | b：小数点后的最小位数，默认为 0
       | c：小数点后的最大位数，默认为 3
   * - locale（可选）
     - string
     - 要使用的本地化格式代码。如果未提供，默认为 en-US。

currency 管道的使用场景，请参阅下面的示例：

 .. code-block:: TypeScript

   import { Component } from '@angular/core';

   @Component({
   selector: 'app-root',
   template: `
      <!--输出 '$0.26'-->
      <p>1、{{a | currency}}</p>

      <!--输出 '¥0'-->
      <p>2、{{a | currency:'JPY'}}</p>

      <!--输出 '¥0.26'-->
      <p>3、{{a | currency:'JPY':'symbol':'1.2-2'}}</p>

      <!--输出 '¥0.26'-->
      <p>4、{{a | currency:'¥'}}</p>

      <!--输出 'RMB 0.26'-->
      <p>5、{{a | currency:'RMB '}}</p>

      <!--输出 '¥0.26'-->
      <p>6、{{a | currency:'JPY':'symbol':'1.2-2'}}</p>

      <!--输出 '¥0.258'-->
      <p>7、{{a | currency:'JPY':'symbol':'1.2-3'}}</p>

      <!--输出 'CA$0.26'-->
      <p>8、{{a | currency:'CAD'}}</p>

      <!--输出 'CAD0.26'-->
      <p>9、{{a | currency:'CAD':'code'}}</p>

      <!--输出 'CA$0,123.00'-->
      <p>10、{{b | currency:'CAD':'symbol':'4.2-2'}}</p>

      <!--输出 '$0,123.00'-->
      <p>11、{{b | currency:'CAD':'symbol-narrow':'4.2-2'}}</p>
      `,
   styles: []
   })
   export class AppComponent {
      a: number = 0.258;
      b: number = 123;
   }

上述代码中，演示了 currency 管道的常见使用场景，输出结果请参阅代码中的注释。

使用date管道
*************************
date管道负责格式化日期值，date管道的使用语法如下：

 .. code-block:: scss

   {{ 日期表达式 | date [ : format [ : timezone [ : locale ] ] ] }}

上述参数详情描述见下表：

 .. list-table:: date管道配置选项
   :widths: 5 3 20

   * - 选项
     - 类型
     - 说明
   * - format（可选）
     - string
     - 日期和时间的格式，使用预定义选项或自定义格式字符串，默认值是 'mediumDate'
   * - timezone（可选）
     - string
     - 用户浏览器上的本地系统时区，默认值是 'undefined'
   * - locale（可选）
     - string
     - 区域代码，默认值是 'undefined'

关于date管道的format选项的值，使用预定义选项或自定义格式字符串，下面对它们进行归纳说明：

 .. list-table:: 预定义的format选项
   :widths: 5 20

   * - 字符串值
     - 说明
   * - short
     - 等价于 'M/d/yy, h:mm a' (4/25/20, 11:07 PM)
   * - medium
     - 等价于 'MMM d, y, h:mm:ss a' (Apr 25, 2020, 11:07:01 PM)
   * - long
     - 等价于 'MMMM d, y, h:mm:ss a z' (April 25, 2020 at 11:07:01 PM GMT+8)
   * - full
     - 等价于 'EEEE, MMMM d, y, h:mm:ss a zzzz' (Saturday, April 25, 2020 at 11:07:01 PM GMT+08:00)
   * - shortDate
     - 等价于 'M/d/yy' (4/25/20)
   * - mediumDate
     - 等价于 'MMM d, y' (Apr 25, 2020)
   * - longDate
     - 等价于 'MMMM d, y' (April 25, 2020)
   * - fullDate
     - 等价于 'EEEE, MMMM d, y' (Saturday, April 25, 2020)
   * - shortTime
     - 等价于 'h:mm a' (11:07 PM)
   * - mediumTime
     - 等价于 'h:mm:ss a' (11:07:01 PM)
   * - longTime
     - 等价于 'h:mm:ss a z' (11:07:01 PM GMT+8)
   * - fullTime
     - 等价于 'h:mm:ss a zzzz' (11:07:01 PM GMT+08:00)


除了上述使用预定义的format选项外，format选项还可以使用自定义格式字符串，下面对它们进行归纳说明：

 .. list-table:: format选项自定义格式字符串
   :widths: 5 5 20

   * - 格式
     - 说明
     - 示例
   * - y
     - 年份
     - 2020
   * - yy
     - 2位数字的年份。
     - 1999（99），2019（19）
   * - yyy
     - 3位数字的年份。大于3位数时，取全，小于3位数时，数值前面补0
     - 2020
   * - yyyy
     - 4位数字的年份
     - 2020
   * - M | L
     - 月份
     - 4
   * - MM | LL
     - 2位数字的月份
     - 04
   * - MMM | LLL
     - 英文月份的缩写
     - Apr
   * - MMMM | LLLL
     - 英文月份的全称
     - April
   * - MMMMM
     - 英文月份的最简写
     - A
   * - LLLLL
     - 英文月份的最简写
     - A4
   * - w（小写字母）
     - 今年的第多少周
     - 17
   * - ww（小写字母）
     - 今年的第多少周（2位数字表示）
     - 17
   * - W（大写字母）
     - 本月的第多少周
     - 4
   * - d
     - 本月的第多少日
     - 25
   * - dd
     - 本月的第多少日（2位数字表示）
     - 25
   * - E/EE/EEE
     - 英文单词星期的缩写
     - Sat
   * - EEEE
     - 英文单词星期的全称
     - Saturday
   * - EEEEE
     - 英文单词星期的最简
     - S
   * - EEEEEE
     - 英文单词星期的最短
     - Sa
   * - a | aa | aaa
     - 上午或下午(am/pm 或 AM/PM)
     - PM
   * - h
     - 12小时制的小时
     - 11
   * - hh
     - 12小时制的小时（2位数）
     - 11
   * - H
     - 24小时制的小时
     - 23
   * - HH
     - 24小时制的小时（2位数）
     - 23
   * - m
     - 分钟
     - 7
   * - mm
     - 2位数的分钟
     - 07
   * - s
     - 秒
     - 1
   * - ss
     - 2位数的秒
     - 01


date 管道的使用场景，请参阅下面的示例：

 .. code-block:: TypeScript

   import { Component } from '@angular/core';

   @Component({
   selector: 'app-root',
   template: `
      <!--演示date管道示例-->
      <div>1、{{currentDate | date}}</div>
      <div *ngFor="let format of formats; let i = index;">
         {{i+2}}、{{format}} -- {{currentDate | date:format}}
      </div>
      `,
   styles: []
   })
   export class AppComponent {
   currentDate: number = Date.now();
   formats: Array<string> = ['short', 'medium', 'long', 'full', 'shortDate',
      'mediumDate', 'longDate', 'fullDate', 'shortTime', 'mediumTime', 'longTime',
      'fullTime', 'y', 'yy', 'yyy', 'yyyy', 'M', 'MM', 'MMM', 'MMMM', 'MMMMM',
      'L', 'LL', 'LLL', 'LLLL', 'LLLLLL', 'w', 'ww', 'W', 'd', 'dd', 'E', 'EE', 'EEE',
      'EEEE', 'EEEEE', 'EEEEEE', 'a', 'aa', 'aaa', 'h', 'hh', 'H', 'HH', 'm', 'mm', 's',
      'ss','z','zz','zzz','zzzz','Z','ZZ','ZZZ','ZZZZ','ZZZZZ','O','OO','OOO','OOOO']
   }

上述代码产生的页面结果如下：

 .. code-block:: text

   1、Apr 25, 2020
   2、short -- 4/25/20, 11:07 PM
   3、medium -- Apr 25, 2020, 11:07:01 PM
   4、long -- April 25, 2020 at 11:07:01 PM GMT+8
   5、full -- Saturday, April 25, 2020 at 11:07:01 PM GMT+08:00
   6、shortDate -- 4/25/20
   7、mediumDate -- Apr 25, 2020
   8、longDate -- April 25, 2020
   9、fullDate -- Saturday, April 25, 2020
   10、shortTime -- 11:07 PM
   11、mediumTime -- 11:07:01 PM
   12、longTime -- 11:07:01 PM GMT+8
   13、fullTime -- 11:07:01 PM GMT+08:00
   14、y -- 2020
   15、yy -- 20
   16、yyy -- 2020
   17、yyyy -- 2020
   18、M -- 4
   19、MM -- 04
   20、MMM -- Apr
   21、MMMM -- April
   22、MMMMM -- A
   23、L -- 4
   24、LL -- 04
   25、LLL -- Apr
   26、LLLL -- April
   27、LLLLLL -- A4
   28、w -- 17
   29、ww -- 17
   30、W -- 4
   31、d -- 25
   32、dd -- 25
   33、E -- Sat
   34、EE -- Sat
   35、EEE -- Sat
   36、EEEE -- Saturday
   37、EEEEE -- S
   38、EEEEEE -- Sa
   39、a -- PM
   40、aa -- PM
   41、aaa -- PM
   42、h -- 11
   43、hh -- 11
   44、H -- 23
   45、HH -- 23
   46、m -- 7
   47、mm -- 07
   48、s -- 1
   49、ss -- 01
   50、z -- GMT+8
   51、zz -- GMT+8
   52、zzz -- GMT+8
   53、zzzz -- GMT+08:00
   54、Z -- +0800
   55、ZZ -- +0800
   56、ZZZ -- +0800
   57、ZZZZ -- GMT+08:00
   58、ZZZZZ -- +08:00
   59、O -- GMT+8
   60、OO -- GMT+8
   61、OOO -- GMT+8
   62、OOOO -- GMT+08:00

预定义的format选项就是组合使用format自定义格式字符串的结果，因此用户可以任意组合使用format选项的自定义格式字符串。


使用i18nSelect管道
*************************
i18nSelect管道类似一个通用选择器，显示匹配当前值的字符串。
i18nSelect 管道的使用场景如在数据中存在性别字段，它的值一般为0和1，但是期望在页面上显示中文的性别，这时可以考虑使用i18nSelect 管道，请参阅下面的示例：

 .. code-block:: TypeScript

   import { Component } from '@angular/core';

   @Component({
   selector: 'app-root',
   template: `
      <!--演示i18nSelect管道示例-->
      <div>{{ female | i18nSelect: dicMap }} </div>
      `,
   styles: []
   })
   export class AppComponent {
      female: string = '0';
      dicMap: any = { '0': '女', '1': '男' };
   }

上述代码中，i18nSelect管道显示匹配当前值male的字符串'女'。


用户自定义管道
==================
除了Angular内置管道外，根据实际需求，用户也可以自定义管道。

用户自定义管道的步骤
*********************
实现自定义管道的步骤可以分为3步：

#. 自定义一个类，该类需要实现PipeTransform接口，并实现接口中的transform()方法；
#. 用@Pipe装饰器声明该类，并且通过装饰器中的元数据name属性定义管道的名字，管道名一般推荐小写字符串形式；
#. 注册自定义管道类。将管道类导入到NgModule类中declarations数组中。

.. admonition:: 提示

   @Pipe装饰器中除了name属性外，还有pure属性，它表示该管道是否是纯管道，pure值等于true时，表示为纯管道，意思是当transform()方法中的参数发生变化时，管道才执行方法中的逻辑；反之，则为非纯管道。pure属性为可选的，默认值等于true，Angular中的内置管道都是属于纯管道。

Angular中提供了Angular CLI命令 ``ng generate pipe 管道类`` 生成自定义管道。由于性能原因，Angular 2+ 之后就没有自带 Filter（过滤）和 OrderBy（排序）管道。下面，我们通过示例演示创建一个排序自定义管道。


[示例 pipe-ex100] 创建一个排序自定义管道
*********************************************

1. 用Angular CLI构建应用程序，具体命令如下：

 .. code-block:: sh

  ng new pipe-ex100 --minimal --interactive=false


2. 启动服务，具体命令如下：

 .. code-block:: sh

  ng serve

3. 查看应用程序结果。打开Web浏览器并浏览到 “http://localhost:4200”，应该看到文本 “Welcome to pipe-ex100!”。

4. 新建接口。使用命令 ``ng generate pipe orderby 简写 ng g p orderby`` 新建接口，并将文件src/app/orderby.pipe.ts更改为以下内容：

 .. code-block:: TypeScript

   import { Pipe, PipeTransform } from '@angular/core';

   @Pipe({
      name: 'orderby'
   })
   export class OrderbyPipe implements PipeTransform {

   transform(value: Array<unknown>, ...args: unknown[]): Array<unknown> {
      if (args.length == 0 || args[0] === 'asc') {
         return value.sort();
      } else if (args[0] === 'desc') {
         return value.sort().reverse();
      }
      return value;
   }

   }

5. 编辑组件。编辑文件src/app/app.component.ts，并将其更改为以下内容：

 .. code-block:: TypeScript

   import { Component } from '@angular/core';

   @Component({
   selector: 'app-root',
   template: `
      <!--演示自定义管道示例-->
      <div>{{ fruits | orderby }} </div>
      <div>{{ fruits | orderby:'desc' }} </div>
      `,
   styles: []
   })
   export class AppComponent {
      fruits: Array<string> = ['apple', 'tomato', 'banana'];
   }


6. 观察应用程序页面，页面显示如下内容：

 .. code-block:: scss

   apple,banana,tomato
   tomato,banana,apple


在上面的示例pipe-ex100中，完成了以下内容：

1. 使用命令 ``ng generate pipe orderby`` 时，Angular CLI命令已经帮助用户基本完成了创建自定义管道的3个步骤，用户仅是需要编辑transform()方法中的逻辑内容；
2. transform()方法中的第一个参数是模板中传递的表达式值，这里是变量fruits的值；第二个参数是附加在orderby管道上的参数；在transform()方法通过判断附加在orderby管道上的参数，决定是升序还是降序；
3. 在模板中，自定义管道的使用方法与使用Angular的内置管道的方法一样。


小结
==========
本章介绍了使用Angular管道的应用知识，带领读者从认识管道的基本概念开始，分别详细介绍了4种类型的内置管道，然后通过示例演示了如何创建自定义管道以及使用它的方法。
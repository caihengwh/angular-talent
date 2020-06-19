.. sectnum::
   :start: 29

##################################
如何部署Angular应用程序
##################################

在开发过程中，包括本书的示例中，指导用户通常会使用 ng serve 命令来借助 webpack-dev-server 在本地内存中构建、监控和提供服务。但是，如果正式部署Angular应用程序时，就必须使用 ng build 命令来构建应用并在其它地方部署这些构建后的文件。

使用ng build命令构建应用
===========================
Angular CLI的ng build 和 ng serve 命令在构建项目之前都会清除输出文件夹，但只有 ng build 命令会把生成的构建的文件写入outputPath参数指定的输出文件夹中。outputPath参数位于angular.json文件中，默认值为dist/project-name/，project-name为应用的名称。
ng build命令的格式如下：

 .. code-block:: sh

  ng build <project> [options]

其中，<project>参数为可选的，指应用程序的名称。options选项参数有很多，下面表格中列举常用的几个进行介绍：

 .. list-table:: ng build 命令参数介绍
   :widths: 5 10

   * - 选项
     - 参数说明
   * - --aot=true|false
     - 使用 Ahead of Time 编译器构建
   * - --base-href=baseHref
     - 修改应用资源的根路径，所有的资源的访问路径以baseHref开头
   * - --prod
     - 使用生产构建，相当于 ng build --configuration prod
   * - --configuration 别名：-c
     - 使用angular.json文件中的“configuration”部分中指定的命名进行构建
   * - --output-path
     - 指定构建文件输出的路径


部署到GitHub页面服务器上的方法
===================================
发布 Angular 应用的简单途径是使用GitHub页面服务器（GitHub Pages）。GitHub页面服务是一个静态站点的服务托管平台，它直接从 GitHub 上的仓库获取 HTML、CSS 和 JavaScript 文件，在构建过程中通过静态站点生成器发布静态应用。GitHub页面服务作为一个免费的服务平台，它只支持静态网站(HTML, CSS, JavaScript)，没有服务端，所以不支持服务端的语言(没有Java，PHP，Python)，我们可以将Angular的应用部署到GitHub页面服务上，

手工发布到GitHub页面服务器
*****************************
下面步骤是指导用户手工发布应用到GitHub页面服务上。

1. 需要创建一个 GitHub 账号，然后为项目创建一个仓库。记下 GitHub 中的用户名（user_name）和仓库名（repository_name）；
2. 克隆仓库到本地，创建Angular工程；
3. 使用 Angular CLI 命令 ng build 来构建这个 GitHub 项目，命令如下：

 .. code-block:: sh

   ng build --prod --output-path docs --base-href=https://pages.github.com/user_name/repository_name/  # 注意repository_name后有个"/"(斜杠)

4. 当构建完成时，复制一份 docs/index.html 文件，命令为 docs/404.html；
5. 提交代码到GitHub仓库；
6. 进入GitHub项目的Setting页中，把该项目配置为从 docs 目录下发布。

完成上述步骤后，可以通过URL https://pages.github.com/user_name/repository_name/ 中查看部署好的页面。

.. admonition:: 注意

  把上述步骤中出现的的user_name和repository_name替换为实际的用户名和仓库名。如果使用的是企业github，请将github.com替换成企业地址。

使用Angular CLI命令自动发布到GitHub页面服务
***********************************************
Angular CLI命令为市面上流行的平台提供了对应的脚手架命令，用来发布发布Angular工程到其平台上，这些平台包括Azure、GitHub Pages和NPM等。其中angular-cli-ghpages命令用于发布Angular工程到GitHub页面服务上，angular-cli-ghpages命令的使用步骤如下：

1. 同手工发布类似，也是需要创建一个 GitHub 账号，然后为项目创建一个仓库。记下 GitHub 中的用户名（user_name）和仓库名（repository_name）；
2. 克隆仓库到本地，创建Angular工程；
3. 安装GitHub页面服务提供的脚手架，命令如下：

 .. code-block:: sh

  ng add angular-cli-ghpages

4. 使用 Angular CLI 命令 ng deploy 来构建这个 GitHub 项目，命令如下：

 .. code-block:: sh

   ng deploy --base-href=/repository_name/ # 注意repository_name前后有个"/"(斜杠)
  
5. 提交代码到GitHub仓库。
   
完成上述步骤后，可以通过URL https://user_name.github.io/repository_name/ 中查看部署好的页面。

.. admonition:: 注意

  把上述步骤中出现的的user_name和repository_name替换为实际的用户名和仓库名。

为什么要在服务器端配置404页面
========================================
Angular路由器的作用就是拦截客户发来的任何请求，然后把它路由到正确的页面。
如果应用程序启动了Angular路由器，部署应用到服务器上时，就必须配置服务器，让它对请求不存在的路由返回应用程序的宿主页面(index.html)。

假如，应用程序中有这样的一个URL：http://www.site.com/user/12，这个URL看上去是请求id等于12的用户详细信息。如果，用户是从应用程序中的某个页面导航到这个URL，Angular路由器会拦截这个URL，并且把它路由到用户信息的页面。但是，当从邮件中点击这个URL链接或直接在浏览器地址栏中输入这个URL时，浏览器会直接向服务器请求这个URL，所有这些操作都是由浏览器本身处理的，在Angular应用的控制范围之外，这时，Angular路由器没机会插手。

在上述场景下，静态服务器会在收到对 http://www.site.com/ 的请求时返回 index.html，如果收到 http://www.site.com/user/12 的请求， 服务器会返回一个 404 - Not Found 的错误。

针对上述问题，不同的服务器有不同的处理方式，比如：Apache服务器中，可以添加一个重写规则，将不存在的URL全部重写、跳转到index.html。Nginx服务器可以使用 try_files 指向 index.html。

如果部署在GitHub页面服务上，由于我们没办法直接配置 Github 的页面服务器，但可以添加一个 404 页，简单的做法就是复制一份index.html到404.html就可以了。当服务器给出一个 404 响应，但是返回的文件内容是index.html，浏览器收到文件内容相当于重新加载了一遍index.html，因此将会正常加载该应用。如果在GitHub主分支的docs目录下启动服务，这是本章前面介绍的手工部署到GitHub页面服务，GitHub页面服务提供了针对此问题的解决方案，就是创建一个空的.nojekyll文件。

使用容器部署Angular应用
=================================


小结
========

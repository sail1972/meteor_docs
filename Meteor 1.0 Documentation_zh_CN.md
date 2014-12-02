***
**本文是由Sail Lee翻译的《Meteor 1.0 Documentation》的中译本。由于译者水平有限，文中不乏错误及无脑直译之处，请各位大虾及Meteor粉丝包涵，欢迎斧正。**

**项目地址：https://github.com/sail1972/meteor_docs**
***

[TOC]

# Meteor概述
***Meteor是一个构建时髦网站超简单的环境。即使是那些号称最好的工具，仍需花费数周才能掌握，而Meteor只需花费几小时即可。***

Web最初被设计成和70年代工作的主机一样。应用服务器绘制一个屏幕并通过网络将它发送给一个哑终端。每当用户做了什么，服务器都会绘制一个新的屏幕。该模式适合Web，运行良好超过十年。它催生了LAMP、Rails、Django、PHP等。

但如今最好的团队、最多的预算和最长的工期都被用来使用Javascript构建运行在客户端的应用程序。这些应用有密集的接口。它们不会重载页面。它们是响应式的：从任何客户端来的变化会立即出现在每个人的屏幕上。

其他人用笨方法来构建它们。而Meteor使之成为更简单、更有趣。你可以在一个周末就构建一个完整的应用程序，或者一场兴奋异常的编程马拉松。你无须准备服务器资源、在云中部署API端点、管理数据库、争论ORM层、在Javascript和Ruby之间来回交换数据、广播数据失效给客户端。

Meteor是一项正在进行中的工作,但我们希望它展现了我们思考的方向。我们希望听到你的反馈。

## 快速入门

下面的代码可工作于所有[支持的平台](https://github.com/meteor/meteor/wiki/Supported-Platforms)

安装Meteor：
```sh
$ curl https://install.meteor.com | /bin/sh
```
创建一个项目：
```sh
$ meteor create myapp
```
在本地运行：
```sh
$ cd myapp
$ meteor
# Meteor server running on: http://localhost:3000/
```
应用发布（到我们提供的免费服务器）：
```sh
$ meteor deploy myapp.meteor.com
```

## Meteor的原则

- 数据在线。Meteor不会通过网络发送HTML。服务器发送数据并让客户端来绘制它。
- 一种语言。无论是应用的客户端还是服务器端，Meteor都可以让你用Javascript来编写。
- 数据库无处不在。从客户端到服务器端，你可以用同样的方法来访问数据库。
- 延时补偿。在客户端，Meteor预读取数据并模拟模型来使之看起来像立即从服务器方法调用返回。
- 全栈响应性。在Meteor中，默认是实时的。有需要时，从数据库到模板的所有层会自动自我更新。
- 包含全生态系统。Meteor是开源的，并集成了现有的开源工具和框架。
- 简单就是生产力。欲简之则简之。Meteor的主要功能有简洁、优雅美丽的应用程序接口。

## 开发者资源

假如Meteor的任何方面引起了你的兴趣，我们希望你会参与到该项目中来。

### 教程

*[Meteor官方教程](https://www.meteor.com/install)*伴随你快速入门！

### Stack Overflow

技术提问（和解答）最好的地方就在*[Stack Overflow](http://stackoverflow.com/questions/tagged/meteor)*。确认在你的提问上加入**meteor**标签。

### 邮件列表

我们有两个Meteor的邮件列表。[meteor-talk@googlegroups.com](http://groups.google.com/group/meteor-talk)用于一般提问、帮助请求和新项目。[meteor-core@googlegroups.com](http://groups.google.com/group/meteor-core)用于协调Meteor核心功能包和构建工具的变更。

### GitHub

核心代码放在[GitHub](http://github.com/meteor/meteor)上。假如你能编写代码或提出问题，我们乐于接受你的帮助。请阅读[Contributing to Meteor](https://github.com/meteor/meteor/wiki/Contributing-to-Meteor)来了解如何启动。

### Meteor手册

关于Meteor核心部件的深入文章可以在[Meteor手册](http://manual.meteor.com/)上看到。

***

# 概念

我们已经手工编写过单页Javascript应用程序了。完全用一种语言（Javascript）和一种数据格式（JSON）编写整个应用程序是一件真正快乐的事。当编写那些应用时，Meteor就是我们想要的全部。

## 什么是Meteor？

Meteor就是两件东西：
- 一个程序库包：在你应用中所需的预先写好的、独立模组。有近一打大部分应用会用到的核心Meteor包。两个例子：[webapp](http://docs.meteor.com/#webapp)，用于出来进入的HTTP连接；templating，让你制作在数据变更时自动动态更新的HTML模板。也有可选的包，如email，它使你的应用能发送邮件，或者Meteor的账号系列（accounts-password、accounts-facebook、accounts-ui等），它们提供了一个能直接应用在你系统中的全功能用户账号系统。除了这些“核心”包以外，还有成千上万个在[Atmosphere](https://atmospherejs.com/)里的社区编写的包，总有一款可能是你所需要的。
- 一个叫**meteor**的命令行工具。meteor是一个类似于make、rake、或Visual Studio的非可视部分的构建工具。它收集了所有在你应用程序中的源文件和资产，实现了任何所需的构建步骤（例如编译CoffeeScript、压缩CSS文件、构建npm模组、或生成源码映射图等），获取在你的应用中使用的包，并输出一个独立的、随时能够运行的应用程序包。在开发模式时，它能交互地做所有这些事，因此无论任何时候你修改了一个文件，你会立即在浏览器中看到变化。它开箱即用超级容易，却也能扩展：你能够给你的应用通过加入构建插件包的方式，来为新语言和编译器添加支持。Meteor包系统的关键思想是：一切都应该同样地工作在浏览器和服务器端（只要是有意义的，当然：浏览器不能发送邮件而服务器也不能捕捉鼠标事件）。我们整个生态系统已经建成并从底层开始就支持这点。

## 构造你的应用程序

一个Meteor应用程序是个的混合体，它融入了运行在网页浏览器中的客户端Javascript程序或PhoneGap移动应用、运行在Meteor服务器端装在Node.js容器中的Javascript程序、所有支持的HTML模板、CSS规则及静态资产等。Meteor自动打包并传送这些不同的部件，对于你在你的文件树中如何选择去构造那些部件，是相当灵活的。

### 特殊的目录

任何在你的Meteor文件夹中的Javascript文件默认会捆绑并发送到客户端和服务器端。但是，在你项目里的文件和目录的名字可以影响它们的装载顺序、它们在哪被装载和其他一些特性。这里有一个会被Meteor特殊对待的文件和目录名列表。

- client 任何叫做`client`的目录都不会在服务器端被装载。这类似于你的代码被包裹在`if (Meteor.isClient) {...}`中。当处于产品模式中时，所有在客户端中载入的文件被自动串接并压缩。在开发模式中，为了易于除错，每个文件被单独地发送。
区别于服务器端的框架，在一个Meteor应用程序中的HTML文件会有相当不同的处理。Meteor扫描所有在你目录中的HTML文件来查找三个高层级元素：<head>、<body>和<template>。head和body部分被分别地串接到单个head和body，它们在初始化页面装载时被传送到客户端。
- server 任何叫做`server`的目录都不会在客户端中被装载。除非客户端连代码也收不到，这类似于你的代码被包裹在`if (Meteor.isServer) {...}`中。任何你不想暴露给客户端的敏感代码，例如包含密码或验证机制，应该被保存在server目录中。
Meteor收集你所有的Javascript文件，包括所有client、public和private子目录下的，都被装载入一个Node.js服务器实例中。在Meteor中，对应每个请求，你的服务器端代码都会运行在一个线程中，而非在一个异步回调中这样的典型Node的方式。在一个Meteor应用程序中，我们发现线性执行模式比较适合典型的服务器端代码。
- public 叫作`public`的顶层目录中的所有文件，都被认为是服务于客户端的。当引用这些资产时，在URL中不要包含`public/`，编写URL要象它们都在最高层级目录一样。举个例子，要以`<img src='/bg.png'>`来引用`public/bg.png`。这个目录是存放`favicon.ico`、`robots.txt`之类文件的最佳地方。
- private 叫作`private`的顶层目录中的所有文件，仅能从服务器端代码访问，并且通过Assets API来装载。该文件夹常用于保存私有数据文件和你不想被外界访问到的你项目目录中任何文件。
- client/compatibility 该文件夹是可兼容的Javascript程序库，它们使用var声明依赖变量在顶层被导出为全局变量。该目录中的文件无需包裹在一个新的变量范围内即可被执行。这些文件在其他客户端Javascript文件前被执行。
- tests 叫作`tests`的目录不会在任何地方被装载，这是用来存放本地测试代码的。

### 特殊目录以外的文件

所有在特殊目录外的Javascript文件都能在客户端和服务器端中被装载。那个地方是给模型定义和其他功能使用的。Meteor提供了Meteor.isClient和`Meteor.isServer`变量，以便依靠其是运行在客户端还是服务器端来改变你的代码的行为。

在特殊目录以外的CSS和HTML文件仅能在客户端上被装载，而且不能在服务器端代码中被使用。

### 文件结构实例

你Meteor应用的文件结构可以非常灵活。这有个应用了上述提及的特殊目录的布局示例：
```sh
lib/                      # common code like collections and utilities
lib/methods.js            # Meteor.methods definitions
lib/constants.js          # constants used in the rest of the code

client/compatibility      # legacy libraries that expect to be global
client/lib/               # code for the client to be loaded first
client/lib/helpers.js     # useful helpers for your client code
client/body.html          # content that goes in the <body> of your HTML
client/head.html          # content for <head> of your HTML: <meta> tags, etc
client/style.css          # some CSS code
client/<feature>.html     # HTML templates related to a certain feature
client/<feature>.js       # JavaScript code related to a certain feature

server/lib/permissions.js # sensitive permissions code used by your server
server/publications.js    # Meteor.publish definitions

public/favicon.ico        # app icon

settings.json             # configuration data to be passed to meteor --settings
mobile-config.js          # define icons and metadata for Android/iOS
```
你也可以按照示例应用来仿构你的目录。运行`meteor create --example todos`并浏览那些目录来看看在一个真实应用中所有的文件是如何放置的。

### 文件装载顺序

最好用这样的方法来编写你的应用程序：它对文件装载顺序是不敏感的，例如通过使用[Meteor.startup](http://docs.meteor.com/#meteor_startup)，或是把装载顺序敏感的代码放入[包](http://docs.meteor.com/#usingpackages)中，从而能明确地控制它们内容的装载顺序和相对于其他包而言它们装载的顺序。但是有时在应用程序中加载顺序依赖关系是不可避免的。

在不使用特殊的文件名和目录时：
- 子目录中的文件加载顺序优先于在父目录中的文件，这样可以使在最深层子目录中的文件被首先加载，最后才是根目录的文件。一个目录之内，会按文件名的字母顺序进行文件加载。

下面是一个控制文件加载顺序的特殊文件和目录的完整清单：
- lib 上述的排序后，在叫做`lib`目录下的所有文件都移到其他一切之前，保留它们的顺序。
- main.* 所有符合`main.*`的文件都被移到其他一切之后，保留它们的顺序。

### 组织你的项目

有三种主要方法来组织你的文件到功能组别或部件中。让我们假设在我们的项目中有两个类型的对象：`apples`和`oranges`。

#### 方法1：根层级文件夹

```sh
apples/lib/               # code for apple-related features
apples/client/
apples/server/

oranges/lib/              # code for orange-related features
oranges/client/
oranges/server/
```

#### 方法2: 在client/和server/中的文件夹

```sh
lib/apples/               # common code for apples
lib/oranges/              # and oranges

client/apples/            # client code for apples
client/oranges/           # and oranges

server/apples/            # server code for apples
server/oranges/           # and oranges
```

#### 方法3：包

这是代码分离、模块化和可重用性的终极形态。假如你把各项功能的代码放置在一个独立的包中，一项功能的代码将不能访问其他功能的代码，除非通过导出，这使它们从属关系更明确。这也使得独立的功能测试变得超简单。你也能够发布这些包并通过`meteor add`命令在多个应用中使用它们。
```sh
packages/apples/package.js     # files, dependencies, exports for apple feature
packages/apples/<anything>.js  # file loading is controlled by package.js

packages/oranges/package.js    # files, dependencies, exports for orange feature
packages/oranges/<anything>.js # file loading is controlled by package.js
```

## 数据与安全

Meteor使编写分布式客户端代码变得象和本地数据库交谈一样。它是一个简洁安全的方法，不再需要实施个体RPC端点，手动缓存数据在客户端上以避免缓慢的往返到服务器，当数据变更时精心编排失效消息到每一个客户端。

在Meteor里面，客户端和服务器端共享同样的数据库API。同样需要的应用程序代码，像验证器和计算属性，能够时常运行在两个地方。但是当代码运行在服务器端时能直接访问数据库，而运行在客户端则不行。这个差别是Meteor的数据安全模型的基础。

> 一个新的Meteor应用默认包括`autopublish`和`insecure`包，它们在一起模拟了每个客户端拥有完全读写访问服务器端数据库权限的效果。这些是有用的原型设计工具，却显然不适合产品化应用程序。当你开发完后，就删掉它们。

每个Meteor客户端包含了一个内存数据库缓存。要管理客户端缓存，服务器端`publishes`发布JSON文档集，而客户端则`subscribes`订阅那些集。当一个文档集中的内容变化了，服务器会修补每个客户端的缓存。

因为得到最佳的支持，所以现在大多数的Meteor应用使用MongoDB作为它们的数据库，不过其他数据库的支持不久也会推出。`Mongo.Collection`类常用于定义Mongo数据集并处理它们。感谢`minimongo`，Meteor的客户端Mongo模拟器，客户端和服务器两端的代码都能够使用到Mongo.Collection。
```javascript
// declare collections
// this code should be included in both the client and the server
Rooms = new Mongo.Collection("rooms");
Messages = new Mongo.Collection("messages");
Parties = new Mongo.Collection("parties");

// server: populate collections with some initial documents
Rooms.insert({name: "Conference Room A"});
var myRooms = Rooms.find({}).fetch();
Messages.insert({text: "Hello world", room: myRooms[0]._id});
Parties.insert({name: "Super Bowl Party"});
```
每个文档集由服务器端的一个发布函数来定义。一个新的客户端订阅一个文档集，发布函数就会运行。文档集中的数据可以来自任何地方，但通常都是发布一个数据库查询。
```javascript
// server: publish all room documents
Meteor.publish("all-rooms", function () {
  return Rooms.find(); // everything
});

// server: publish all messages for a given room
Meteor.publish("messages", function (roomId) {
  check(roomId, String);
  return Messages.find({room: roomId});
});

// server: publish the set of parties the logged-in user can see.
Meteor.publish("parties", function () {
  return Parties.find({$or: [{"public": true},
                             {invited: this.userId},
                             {owner: this.userId}]});
});
```
发布函数能够提供不同的结果给每个客户端。在上一个例子中，一个已登入的用户仅能看见被公开的Party文档，那是该用户拥有的或是该用户已被邀请到的。

一旦订阅了，客户端象一个快速的本地数据库一样地使用缓存，戏剧性地简化了客户端代码。读取无需代价高昂的往返到服务器端。而且它们仅限缓存的内容：对于集合中的每一个文档，客户端上的查询将仅返回服务器端发布的文档到那个客户端。
```javascript
// client: start a parties subscription
Meteor.subscribe("parties");

// client: return array of Parties this client can read
return Parties.find().fetch(); // synchronous!
```
复杂的客户端能够订阅的开关来控制有多少数据被保留在缓存中和管理网络流量。当一个订阅被关闭，所有它的文档从缓存中删除，除非相同的文档也由另一个活跃的订阅提供了。

当客户端改变了一个或更多的文档，它发送一个消息到服务器端请求变更。服务器端依靠一组你编写的类似Javascript函数的允许/拒绝规则来检查提出的变更。只有所有的规则都通过了，服务器端才接受这个变更。
```javascript
// server: don't allow client to insert a party
Parties.allow({
  insert: function (userId, party) {
    return false;
  }
});

// client: this will fail
var party = { ... };
Parties.insert(party);
```
如果服务器端接受变更，它将更改应用到数据库并自动将更改传播到其他订阅了受影响文档的客户。如果不接受，更新失败，服务器端的数据库仍然保持原样，没有其他客户端能看到更新。

不过，Meteor有个乖巧的把戏。当一个客户端提交一个写入到服务器，它也立即更新本地缓存，而不必等待服务器端响应。这意味着屏幕马上重绘。如果服务器接受更新，这绝大多数应该发生在一个正常行为的客户端，那么客户端在变更上获得了一个跳跃周期，不必等待往返来更新自己的屏幕。如果服务器拒绝变更，Meteor用服务器端的结果来修补该客户端的缓存。

所有的都整合在一起，这些技术实现了延迟补偿。客户端持有着一份所需的最新数据拷贝，而不需要等待到服务器端的往返。当客户端修改数据时，那些修改能在本地运行无需等待来自服务器端的确认，同时仍然把请求的变更的最终决定权给了给服务器端。

>当前版本的Meteor支持MongoDB，流行的文档数据库，本章节中的实例使用[MongoDB API](http://www.mongodb.org/display/DOCS/Manual)。以后的版本会包括对其他数据库的支持。

### 鉴权与用户账户

Meteor包括了[Meteor Accounts](http://docs.meteor.com/#accounts_api)，一个先进的认证系统。它的特点是安全密码登录采用[bcrypt](http://en.wikipedia.org/wiki/Bcrypt)算法，并且集成了包括Facebook、GitHub、Google、Meetup、Twitter和Weibo等外部服务。Meteor账号定义了一个[Meteor.users](http://docs.meteor.com/#meteor_users)集合，开发者能够在里面存放应用程序特有用户数据。

Meteor还包括了给通用任务用的预建表单，如：登录、注册、密码变更和邮件重置密码。你可以仅用一行代码添加[Accounts UI](http://docs.meteor.com/#accountsui)到你的应用中。这个accounts-ui包甚至还提供了一个配置向导来引导你完成设置你应用中正在使用的外部登录服务。

### 输入校验

Meteor允许你的方法和发布函数使用任何JSON类型的参数（事实上，Meteor的连线协议支持EJSON，一个JSON的扩展，它也支持其他通用类型，象日期和二进制缓冲区）。Javascript的动态类型意味着你无须在你的应用中声明每个变量的精确类型，但它通常有助于确保客户端传递到你方法和发布函数的参数是如你所需的类型。

Meteor提供了一个[轻量库](http://docs.meteor.com/#check_package)来检查参数和其他值是如你所期待的。用这些语句简单开始你的函数，如`check(username,String)`或者`check(offic,{building: String,room：Number}）`。假如它的参数是非期望的类型，这个`check`调用会抛出一个错误。

## 响应性

Meteor遵从[响应式编程](http://en.wikipedia.org/wiki/Reactive_programming)的概念。这意味着你能够在一个简单的命令式风格里编写你的代码，无论你代码所依赖数据何时改变，结果都会自动重算。
```javascript
Tracker.autorun(function () {
  Meteor.subscribe("messages", Session.get("currentRoomId"));
});
```
这个例子（从一个聊天室中拿出来的代码）基于会话变量`currentRoomId`来设置一个数据订阅。假如`Session.get("currentRoomId")`的值因某些原因而改变，该函数将被自动重新运行，设置一个新的订阅来替代旧的。

这个自动重新计算机制是通过`Session`和`Tracker.autorun`之间的协作来实现。`Tracker.autorun`执行一个任意的“响应式运算”追踪里面的数据依赖性，并在必要时重新运行其函数参数。在另一方面，数据提供者，如会话，记录下它们被调用的来源运算和什么数据被请求，并且在数据变更时，发送一个无效信号给该运算。

这个简单的模式（响应式计算+响应式数据源）有广泛的适用性。编写退订/重订阅调用并确认在正确的时间被调用，把程序员拯救出来。一般情况下，Meteor能够消除数据传播代码的全部类，否则它们容易出错的逻辑会堵塞你的应用程序。

以下这些Meteor函数象响应式运算一样运行你的代码：

- [Template](http://docs.meteor.com/#livehtmltemplates)
- [Tracker.autorun](http://docs.meteor.com/#tracker_autorun)
- [Blaze.render](http://docs.meteor.com/#blaze_render)和[Blaze.renderWithData](http://docs.meteor.com/#blaze_renderwithdata)

以及能够触发变更的响应式数据源是：

- [Session](http://docs.meteor.com/#session)变量
- 在[Collection](http://docs.meteor.com/#find)上的数据库查询
- [Meteor.status](http://docs.meteor.com/#meteor_status)
- 在一个[订阅处理](http://docs.meteor.com/#meteor_subscribe)中的ready()方法
- [Meteor.user](http://docs.meteor.com/#meteor_user)
- [Meteor.userId](http://docs.meteor.com/#meteor_userid)
- [Meeor.loggingIn](http://docs.meteor.com/#meteor_loggingin)

此外，下列返回一个附带stop方法对象的函数，如果从一个响应式运算中被调用，当该运算重运行或停止是会被停止。

- [Tracker.autorun](http://docs.meteor.com/#tracker_autorun)（嵌套的）
- [Meteor.subscribe](http://docs.meteor.com/#meteor_subscribe)
- 在游标上的[observe()](http://docs.meteor.com/#observe)和[observeChange()](http://docs.meteor.com/#observe_changes)

Meteor的实现是一个叫Tracker的包，它相当的短小直接。你自己能够用它来实现新的响应式数据源。

## 活化的HTML模板

HTML模板是Web应用程序的中枢。通过Blaze，Meteor的活化页面更新技术，你能够响应式地绘制你的HTML，这意味着它将自动跟踪在数据中的变化并用来生成它。

Meteor令连同Meteor的活化页面更新技术一起使用你最喜欢的HTML模板语言变得容易。仅仅和过去编写你的模板一样，而Meteor将照顾它，使之实时更新。

Meteor附带了一个叫[Spacebars](https://github.com/meteor/meteor/blob/devel/packages/spacebars/README.md)的模板语言，它产生自[Handlebars](http://handlebarsjs.com/)。它分享了一些Handlebars的精神和语法，但它已被修改用于在编译时产生响应式Meteor模板。

>如今，虽然我们的社区已经创建了适合于其他语言的包，例如[Jade](https://atmospherejs.com/mquandalle/jade)，但跟随Meteor提供的唯一模板系统是Spacebars。

要定义模板，在你的项目中创建一个带`.html`后续名的文件。在该文件中，建一个`<template>`标签并给它一个`name`属性。把模板内容放在这个标签里面。Meteor将预编译模板，传送到客户端，并使其作为全局Template对象一样可用。

当你的应用被装载时，它自动绘制叫`<body>`特定模板，那个使用`<body>`元素而不是`<template>`。通过使用`{{> 包含内容}}`在另一个模板中插入一个模板。

要把数据塞进模板里面最简单的办法是通过用Javascript定义`helpers`函数。用`Template.templateName.helpers({...})`函数来定义`helpers`。把它放在一起：
```html
<!-- in myapp.html -->
<body>
  <h1>Today's weather!</h1>
  {{> forecast}}
</body>

<template name="forecast">
  <div>It'll be {{prediction}} tonight</div>
</template>
```
```javascript
// in client/myapp.js: reactive helper function
Template.forecast.helpers({
  prediction: function () {
    return Session.get("weather");
  }
});
```
```javascript
// in the JavaScript console
> Session.set("weather", "cloudy");
> document.body.innerHTML
 => "<h1>Today's weather!</h1> <div>It'll be cloudy tonight</div>"

> Session.set("weather", "cool and dry");
> document.body.innerHTML
 => "<h1>Today's weather!</h1> <div>It'll be cool and dry tonight</div>"
```
遍历一个数组或数据库游标,使用`{{#each}}`:
```html
<!-- in myapp.html -->
<template name="players">
  {{#each topScorers}}
    <div>{{name}}</div>
  {{/each}}
</template>
```
```javascript
// in myapp.js
Template.players.helpers({
  topScorers: function () {
    return Users.find({score: {$gt: 100}}, {sort: {score: -1}});
  }
});
```
在这个案例中，数据来自数据库查询。当数据库游标被传送到`{{#each}}`，它将连接所有的装置来高效地添加和移动DOM节点作为新结果进入该查询。

`helpers`能获取参数，而且它们接收在此的当前模板上下文数据。注意，某些`block helpers`会改变当前的上下文（尤其是`{{#each}}`和`{{#with}}`）：
```javascript
// in a JavaScript file
Template.players.helpers({
  leagueIs: function (league) {
    return this.league === league;
  }
});
```
```html
<!-- in a HTML file -->
<template name="players">
  {{#each topScorers}}
    {{#if leagueIs "junior"}}
      <div>Junior: {{name}}</div>
    {{/if}}
    {{#if leagueIs "senior"}}
      <div>Senior: {{name}}</div>
    {{/if}}
  {{/each}}
</template>
```
`helpers`也能传递常量数据。
```javascript
// Works fine with {{#each sections}}
Template.report.helpers({
  sections: ["Situation", "Complication", "Resolution"]
});
```
最后，你可以使用一个模板的`events`函数附加事件处理器。传递到`events`中的对象被记录在[事件映射](http://docs.meteor.com/#eventmaps)。事件处理器的`this`参数将是触发该事件的元素的数据上下文。
```html
<!-- myapp.html -->
<template name="scores">
  {{#each player}}
    {{> playerScore}}
  {{/each}}
</template>

<template name="playerScore">
  <div>{{name}}: {{score}}
    <span class="give-points">Give points</span>
  </div>
</template>
```
```javascript
// myapp.js
Template.playerScore.events({
  'click .give-points': function () {
    Users.update(this._id, {$inc: {score: 2}});
  }
});
```
关于`Spacebars`更详细内容，参见[Spacebars README](https://github.com/meteor/meteor/blob/devel/packages/spacebars/README.md)

## 使用包

迄今为止你已阅知的所有功能，在标准的Meteor包中都被实现了。这应感谢Meteor异乎寻常强大的同构包和构建系统。同构指的是工作在Web浏览器中的相同包，无论其在移动应用中还是在服务器上。包也能包含扩展构建过程的插件，例如`coffeescript`（编译CoffeeScript）或`templating`（编译HTML模板）。

任何人都能发布一个Meteor包，而且迄今为止成千上万社区编写的包已被发布。浏览这些包的最佳地方是由Percolate Studio建立的[Atmosphere](http://www.atmospherejs.com/)。你也可以用[meteor search](http://docs.meteor.com/#meteorsearch)和[meteor show](http://docs.meteor.com/#meteorshow)命令。

你能用[meteor add](http://docs.meteor.com/#meteoradd)命令来添加包到你的项目，还能用[meteor remove](http://docs.meteor.com/#meteorremove)命令来移除它们。另外，[meteor list](http://docs.meteor.com/#meteorlist)将告诉你项目中有什么正在使用，可能的话，[meteor update](http://docs.meteor.com/#meteorupdate)将更新它们到最新版本。

默认所有的应用都包含`meteor-platform`包。这将自动安装包组成核心Meteor栈。假如你想构建你自己的定制栈，只要从你应用中移除`meteor-platform`并添加回你想保留的标准包。

Meteor采用了一个单一装载包的系统，这意味着Meteor仅仅装载每个包的一个版本。在一个包添加或升级到一个特定版本之前，Meteor采用约束求解器来检查这样做会否导致破坏其他包。默认情况下，Meteor将做保守选择。当添加具有可传递依赖项时（包装其他包，而非应用程序本身），Meteor将尝试选择较早期的版本。

除了Meteor释出的官方包被你的应用使用，`meteor list`和`meteor add`也会搜索在你应用顶部的`packages`目录。为了方便，你也能使用`packages`目录来分解你的应用到子包或你可能要发布的测试包里面去。详见[编写包](http://docs.meteor.com/#writingpackages)。

## 命名空间

Meteor的命名空间支持，令其可以很容易地使用Javascript来编写大型应用程序。你用在应用中的每个包，都存在自己独立的命名空间。这意味着它只能看到自己的全局变量和所有它特定使用的包提供的变量。在此说下它是如何工作的。

当定义一个顶层变量时，你有一个选择。你可以建立变量文件范围（File Scope）或包范围（Package Scope）。
```javascript
// File Scope. This variable will be visible only inside this
// one file. Other files in this app or package won't see it.
var alicePerson = {name: "alice"};

// Package Scope. This variable is visible to every file inside
// of this package or app. The difference is that 'var' is
// omitted.
bobPerson = {name: "bob"};
```
请注意，这只是为了声明一个变量是本地或全局正常的JavaScript语法。Meteor扫描你的源代码 全局变量赋值并产生一个包裹来确保你的全局变量不会跑到它们适当的命名空间外。

除了文件范围（File Scope）或包范围（Package Scope），还有出口（Exports）。一个出口就是一个在你想使用它时包提供给你的变量。例如，邮件包输出的`Email`变量。假如你的应用使用该`email`包（而且它仅使用该`email`包！），那么你的应用能看见`Email`，并且你能调用`Email.send`。多数的包仅有一个出口，但有些包可能有两个或三个（例如，一个提供了几个协同工作的类）。

你仅看见你直接使用的包的出口。假如你使用了A包，而A包又使用了B包，那么你仅能看见A包的出口。由于你只使用了A包，B包的出口不会“泄露”到你的命名空间中去。这令每个命名空间都保持漂亮和整洁。每个应用或包仅能看见它们自己的全局变量加上它们特定请求的包的API

当调试你的应用时，浏览器的Javascript控制台的行为就像它是附加在你应用的命名空间中。你看到应用的全局变量和你应用直接使用的包的出口。你看不见那些来自内部包的变量，而且你看不见你的可传递依赖项的出口（非你应用直接使用的包，但却是由你应用使用的包用到的）。

假如你想从浏览器中的调试工具中查看内部包，你有两个选择：
- 在包代码中设置断点。当停在断点时，控制台将位于该包的命名空间中。你将看见这个包的包范围内的变量、入口和位于你停留位置所在文件的任何文件范围变量。
- 假如一个叫`foo`的包包含在你的应用中，无论你的应用是否直接使用它，它的出口在`Package.foo`中可用。例如，如果`email`包被装载，那么你可以从命名空间中访问`Package.email.Email.send`事件，即使其没有直接使用`email`包。

当声明函数时，记住，`function x() {}`就是在Javascript中`var x = function () {}`的简写。参考这些例子：
```javascript
// This is the same as 'var x = function () ...'. So x() is
// file-scope and can be called only from within this one file.
function x () { ... }

// No 'var', so x() is package-scope and can be called from
// any file inside this app or package.
x = function () { ... }
```
>从技术角度说，一个应用中的全局变量（而不是在一个包里）实际上是真正的全局变量。它们不能在对于应用代码是私有的范围内被捕捉到，因此这将意味着它们在调试期间不会在控制台中可见！这意味着应用全局变量实际上最终在包中可见。那从不应是正确编写包代码的一个问题（因为应用全局变量仍将被包中的声明完全地遮蔽）。你当然不应依赖这种怪癖，而且假设你这样做了，将来Meteor会检查并抛出一个错误。

## 部署

Meteor是一个完全的应用服务器。我们提供了所有部署到互联网的所需：你只要提供Javascript、HTML和CSS文件。

### 运行在Meteor提供的基础设施上

部署你应用程序最简单的方法就是使用`meteor deploy`命令。就个人而言，我们提供它是因为那是我们一直想要的：一种简易方法去产生一个应用创意，用一个周末来实现它，并把它发布到那里给全世界使用，什么都妨碍不了创造力。
```sh
$ meteor deploy myapp.meteor.com
```
现在你的应用程序已经在myapp.meteor.com上了。如果这是首次部署到该主机名，Meteor会为你的应用程序创建一个新的空数据库。如果你要部署一个升级版，Meteor将保留现有的数据，只是刷新代码。

你也可以部署到自己的域名。只要设置想要使用的主机名到`origin.meteor.com`作为一个CNAME，然后部署那个名字。
```sh
$ meteor deploy www.myapp.com
```
我们提供这种免费服务，所以你可以尝试下Meteor。它也有助于迅速建造内部beta版、演示等。更多信息，详见[meteor deploy](http://docs.meteor.com/#meteordeploy)。

### 运行在你自己的基础设施上

你也可以把应用程序运行在自己的基础设施或任何能运行Node.js应用的主机提供商上。

首先，运行：
```sh
$ meteor build my_directory
```
该命令将生成一个完全容纳的Node.js应用程序tarball包。要运行该应用程序，你需要提供Node.js 0.10和一个MongoDB服务器。（Meteor的当前版本已在Node 0.10.29下测试通过；较旧的版本包含了一个会导致生产服务器停滞的严重bug。）之后，你可以通过援引节点运行你的应用程序，为应用程序指定监听的HTTP端口和MongoDB端点。
```sh
$ cd my_directory
$ (cd programs/server && npm install)
$ PORT=3000 MONGO_URL=mongodb://localhost:27017/myapp node main.js
```
某些包可能需要其他环境变量。例如，`email`包需要一个`Mail_URL`的环境变量。

## 编写包

编写Meteor包很简单。要初始化一个Meteor包，运行
```sh
$ meteor create --package username:packagename
```
其中`username`是你的`Meteor Developer`名称。这将从零开始创建一个包并且用一个`package.js`控制文件和一些javascript脚本预填到目录中。默认情况下，Meteor将从包含该package.js文件的目录名中提取该包名称。

Meteor承诺包和应用程序都可重复构建。这意味着，一旦你在一台机器上构建了你的包，然后签入那些代码到一个代码库中并在其他地方签出，你应该得到相同的结果。在你的包目录中，你会找到一个自动生成的`version.json`文件。该文件指定了用于构建你的包所有包的版本和源码的一部分。签入版本控制，以确保跨越机器可重复构建。

>有时候，包不仅仅依靠它们自己，还在一个应用的上下文中发挥作用（特别是在应用的`packages`目录下的包）。在那些案例中，应用的上下文将优先考虑。而不是用`version.json`文件作为指引，我们将用那些与应用使用的相同的依赖项去构建该包（在实践中，发现你的本地包是用不同版本的相同包来构建，我们认为这很困惑）。但是，我们仍旧编写新的`version.json`。

Meteor为它的包使用了扩展的semver版本控制器（一种语义化版本控制器）：这意味着，一个可选的预发布版本，其版本号用点分开成三部分：主版本、次版本和补丁版本（例如，1.2.3）。关于这个，你可以到[semver.org](http://www.semver.org/)去了解更多。此外，由于一些meteor包包裹了外部库，Meteor支持使用_来表示一个包裹数量这样的约定。

你可以在API章节中读到更多关于[package.js](http://docs.meteor.com/#packagejs)文件的内容。

因为测试是开发过程的一个重要部分，所以有两个通用办法来测试一个包：
- 要测试一个包，集成测试是最通用的方法（直接放置一个包到应用程序里去，并针对应用程序编写测试）。在创建你的包之后，添加它到你应用的`/packages`目录并运行`meteor add`。这将添加你的包作为一个本地包到你应用。然后，你可以测试和像平常一样运行你的应用。Meteor将检测并反馈变化到你的本地包，就像对待你的应用文件一样。
- 单元测试是用[meteor test-packages package-name](http://docs.meteor.com/#meteortestpackages)。和[package.js](http://docs.meteor.com/#packagejs)章节中描述的一样，你可以使用`package.js`文件来指定你的单元测试位于何处。如果你有一个仅包含该包源代码的代码库，你可以通过指定到该包目录的路径（必须包含一个`/`）来测试你的包，例如：`meteor test-packages ./`。

要发布一个包，从该包目录运行[meteor publish](http://docs.meteor.com/#meteorpublish)。已发布的包有一些额外的限制：它们必须包含一个版本（Meteor包使用严格的semver版本控制器来控制版本）而且它们的名字必须以作者的用户名和一个冒号作为前缀，像这样：`iron:router`。该命名空间允许更具描述性和切题的包名。

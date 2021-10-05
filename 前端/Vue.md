# 笔记

## 1.简介

### 1.1 概述

Vue是一套用于构建用户界面的**渐进式框架**,发布于2014年2月。与其他大型框架不同的是Vue被设计为可以自底向上逐层应用，**Vue的核心库只关注视图层**，不仅易于上手还便于与第三方库（如：`vue-router:跳转`,`axios::通信`,`vuex:状态管理`）或既有项目整合

> 视图层:HTML+CSS+JS,给用户看,刷新后台的数据

官网:https://cn.vuejs.org/v2/guide/

### 1.2 前端三要素

1. HTML（结构）：超文本标记语言（Hyper Text Markup Language），决定网页的结构和内容
2. CSS（表现）：层叠样式表（Cascading Style Sheets），设定网页的表现样式
3. JavaScript（行为）：是一种弱类型脚本语言，其源代码不需经过编译，而是由浏览器解释运行，用于控制网页的行为

1. ### 结构层(HTML)

   ...

2. ### 表现层(CSS)

   **概述**:CSS 层叠样式表是一门标记语言，并不是编程语言，因此不可以自定义变量，不可以引用等，换句话说就是不具备任何语法支持;

   缺点:

   1. 语法不够强大，比如无法嵌套书写，导致模块化开发中需要书写很多重复的选择器;
   2. 没有变量和合理的样式复用机制，使得逻辑上相关的属性值必须以字面量的形式重复输出，导致难以维护;

   这就导致了我们在工作中无端增加了许多工作量。为了解决这个问题，前端开发人员会使用一种称之为【CSS 预处理器】 的工具，提供 CSS 缺失的样式层复用机制、减少冗余代码，提高样式代码的可维护
   性。大大提高了前端在样式上的开发效率;

   常用的CSS预处理器:

   1. SASS：基于 Ruby，通过服务端处理，功能强大。解析效率高。需要学习 Ruby 语言，上手难度高于 LESS;
   2. LESS：基于 NodeJS，通过客户端处理，使用简单。功能比 SASS 简单，解析效率也低于 SASS，但在实际开发中足够了，所以我们后台人员如果需要的话，建议使用 LESS

   > CSS预处理器:定义新的一门语言,为CSS增加一些编程的特性,也就是这个程序运行结果会产生一个CSS,而它本身并不是一个CSS!
   >
   > 换句话解释:用一种专门的编程语言,进行Web页面样式设计,再通过编译器转换为正常的CSS文件,以供项目使用!

3. ### 行为层(JavaScript)

   **概述**:JavaScript 一门弱类型脚本语言，其源代码在发往客户端运行之前不需经过编译，而是将文本格式的字符代码发送给浏览器由浏览器解释运行

   原生 JS 开发，也就是让我们按照 【ECMAScript】 标准的开发方式，简称是 ES，特点是所有浏览器都支持!

   TypeScript 是一种由微软开发的自由和开源的编程语言。它是 JavaScript 的一个超集，而且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程

   > TypeScript:编程完后打包成JavaScript去运行

### 1.3 JS框架

1. jQuery库

   ```
   大家最熟知的 JavaScript 库，优点是简化了 DOM 操作，缺点是 DOM 操作太频繁，影响前端性能；在前端眼里使用它仅仅是为了兼容 IE6、7、8
   ```

2. Angular

   ```
   Google 收购的前端框架，由一群 Java 程序员开发，其特点是将后台的 MVC 模式搬到了前端并增加了模块化开发的理念，与微软合作，采用 TypeScript 语法开发；对后台程序员友好，对前端程序员不太友好；最大的缺点是版本迭代不合理（如：1代 -> 2代，除了名字，基本就是两个东西；已推出了Angular6）
   ```

3. React

   ```
   Facebook 出品，一款高性能的 JS 前端框架；特点是提出了新概念 【虚拟 DOM】 用于减少真实 DOM操作，在内存中模拟 DOM 操作，有效的提升了前端渲染效率；缺点是使用复杂，因为需要额外学习一门 【JSX】 语言
   ```

4. Vue

   ```
   一款渐进式 JavaScript 框架，所谓渐进式就是逐步实现新特性的意思，如实现模块化开发、路由、状态管理等新特性。其特点是综合了 Angular（模块化） 和 React（虚拟 DOM） 的优点
   ```

5. Axios

   ```
   前端通信框架；因为 Vue 的边界很明确，就是为了处理 DOM，所以并不具备通信能力，此时就需要额外使用一个通信框架与服务器交互；当然也可以直接选择使用 jQuery 提供的 AJAX 通信功能
   ```

### 1.4 UI框架

常用的:

- Ant-Design：阿里巴巴出品，基于 React 的 UI 框架
- ElementUI、iview、ice：饿了么出品，基于 Vue 的 UI 框架
- Bootstrap：Twitter 推出的一个用于前端开发的开源工具包
- AmazeUI：又叫“妹子 UI”，一款 HTML5 跨屏前端框架
- Layui：轻量级框架

其他的:

1. View

   ```
   iview 是一个强大的基于 Vue 的 UI 库，有很多实用的基础组件比 elementui 的组件更丰富，主要服务于PC 界面的中后台产品。使用单文件的 Vue 组件化开发模式 基于 npm + webpack + babel 开发，支持ES2015 高质量、功能丰富 友好的 API ，自由灵活地使用空间。
   ```

   [官网地址] https://www.iviewui.com/

   注意:属于前端主流框架，选型时可考虑使用，主要特点是移动端支持较多

2. ElementUI

   ```
   Element 是饿了么前端开源维护的 Vue UI 组件库，组件齐全，基本涵盖后台所需的所有组件，文档讲
   解详细，例子也很丰富。主要用于开发 PC 端的页面，是一个质量比较高的 Vue UI 组件库
   ```

   [官网地址] http://element-cn.eleme.io/#/zh-CN

   注意:属于前端主流框架，选型时可考虑使用，主要特点是桌面端支持较多

3. ICE

   ```
   飞冰是阿里巴巴团队基于 React/Angular/Vue 的中后台应用解决方案，在阿里巴巴内部，已经有 270 多个来自几乎所有 BU 的项目在使用。飞冰包含了一条从设计端到开发端的完整链路，帮助用户快速搭建属于自己的中后台应用。
   ```

   [官网地址] https://alibaba.github.io/ice

4. VantUI

   ```
   Vant UI 是有赞前端团队基于有赞统一的规范实现的 Vue 组件库，提供了一整套 UI 基础组件和业务组件。通过 Vant，可以快速搭建出风格统一的页面，提升开发效率
   ```

   [官网地址] https://youzan.github.io/vant/#/zh-CN/intro

5. AtUI

   ```
   at-ui 是一款基于 Vue 2.x 的前端 UI 组件库，主要用于快速开发 PC 网站产品。 它提供了一套 npm +webpack + babel 前端开发工作流程，CSS 样式独立，即使采用不同的框架实现都能保持统一的 UI 风格
   ```

   [官网地址] https://at-ui.github.io/at-ui/#/zh

6. CubeUI

   ```
   cube-ui 是滴滴团队开发的基于 Vue.js 实现的精致移动端组件库。支持按需引入和后编译，轻量灵活；扩展性强，可以方便地基于现有组件实现二次开发
   ```

   [官网地址] https://didi.github.io/cube-ui/#/zh-CN

7. Flutter

   ```
   Flutter 是谷歌的移动端 UI 框架，可在极短的时间内构建 Android 和 iOS 上高质量的原生级应用。
   Flutter 可与现有代码一起工作, 它被世界各地的开发者和组织使用, 并且 Flutter 是免费和开源的
   ```

   [官网地址] https://flutter.dev/docs

   注意:Google 出品，主要特点是快速构建原生 APP 应用程序，如做混合应用该框架为必选框架

8. Ionic

   ```
   Ionic 既是一个 CSS 框架也是一个 Javascript UI 库，Ionic 是目前最有潜力的一款 HTML5 手机应用开发框架。通过 SASS 构建应用程序，它提供了很多 UI 组件来帮助开发者开发强大的应用。它使用JavaScript MVVM 框架和 AngularJS/Vue 来增强应用。提供数据的双向绑定，使用它成为 Web 和移动开发者的共同选择。
   ```

   [官网地址] https://ionicframework.com/

9. mpvue

   ```
   mpvue 是美团开发的一个使用 Vue.js 开发小程序的前端框架，目前支持 微信小程序、百度智能小程序，头条小程序 和 支付宝小程序。 框架基于 Vue.js，修改了的运行时框架 runtime 和代码编译器compiler 实现，使其可运行在小程序环境中，从而为小程序开发引入了 Vue.js 开发体验
   ```

   [官网地址] http://mpvue.com/

   注意:完备的 Vue 开发体验，并且支持多平台的小程序开发，推荐使用

10. WeUI

    ```
    WeUI 是一套同微信原生视觉体验一致的基础样式库，由微信官方设计团队为微信内网页和微信小程序量身设计，令用户的使用感知更加统一。包含 button、cell、dialog、toast、article、icon 等各式元素
    ```

## 2. MVVM模式

### 2.1 什么是MVVM模式:

MVVM（Model-View-ViewModel）是一种软件架构设计模式，由微软 WPF（用于替代 WinForm，以前就是用这个技术开发桌面应用程序的）和 Silverlight（类似于 Java Applet，简单点说就是在浏览器上运行的 WPF） 的架构师 Ken Cooper 和 Ted Peters 开发，是一种简化用户界面的事件驱动编程方式。由 John Gossman（同样也是 WPF 和 Silverlight 的架构师）于 2005 年在他的博客上发表。MVVM 源自于经典的 MVC（Model-View-Controller）模式。MVVM 的核心是 ViewModel 层，负责转换 Model 中的数据对象来让数据变得更容易管理和使用，其作用如下：

- 该层向上与视图层进行双向数据绑定
- 向下与 Model 层通过接口请求进行数据交互

![image-20211005102757784](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211005102757784.png)

MVVM 已经相当成熟了，当下流行的 MVVM 框架有 Vue.js ， AngularJS 等。

### 2.2 为什么使用

MVVM 模式和 MVC 模式一样，主要目的是分离视图（View）和模型（Model），有几大好处：

- 低耦合:
  - 视图（View）可以独立于 Model 变化和修改，一个 ViewModel 可以绑定到不同的 View上，当 View 变化的时候 Model 可以不变，当 Model 变化的时候 View 也可以不变
- 可复用:
  - 你可以把一些视图逻辑放在一个 ViewModel 里面，让很多 View 重用这段视图逻辑
- 独立开发:
  - 开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计
- 可测试:
  - 界面素来是比较难于测试的，而现在测试可以针对 ViewModel 来写。

### 2.3 组成部分

![image-20211005103040512](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211005103040512.png)

1. View:

   ```
   View 是视图层，也就是用户界面。前端主要由 HTML 和 CSS 来构建，为了更方便地展现ViewModel 或者 Model层的数据，已经产生了各种各样的前后端模板语言，比如 FreeMarker、
   Thymeleaf 等等，各大 MVVM 框架如 Vue.js，AngularJS，EJS 等也都有自己用来构建用户界面的内置模板语言。
   ```

2. Model:

   ```
   Model 是指数据模型，泛指后端进行的各种业务逻辑处理和数据操控，主要围绕数据库系统展开。这里的难点主要在于需要和前端约定统一的 接口规则
   ```

3. ViewModel:

   ```
   ViewModel 是由前端开发人员组织生成和维护的视图数据层。在这一层，前端开发者对从后端获取的Model 数据进行转换处理，做二次封装，以生成符合 View 层使用预期的视图数据模型
   ```

   注意: ViewModel 所封装出来的数据模型包括视图的状态和行为两部分，而 Model 层的数据模型是只包含状态的

   - 比如页面的这一块展示什么，那一块展示什么这些都属于视图状态（展示）
   - 页面加载进来时发生什么，点击这一块发生什么，这一块滚动时发生什么这些都属于视图行为（交互）

   > 视图状态和行为都封装在了 ViewModel 里。这样的封装使得 ViewModel 可以完整地去描述 View 层。由于实现了双向绑定，ViewModel 的内容会实时展现在 View 层，这是激动人心的，因为前端开发者再也不必低效又麻烦地通过操纵 DOM 去更新视图

MVVM 框架已经把最脏最累的一块做好了，我们开发者只需要处理和维护 ViewModel，更新数据视图就会自动得到相应更新，真正实现 事件驱动编程 。

> View 层展现的不是 Model 层的数据，而是 ViewModel 的数据，由 ViewModel 负责与Model 层交互，这就完全解耦了 View 层和 Model 层，这个解耦是至关重要的，它是前后端分离方案实施的重要一环

## 3.快速使用

### 3.1 MVVM模式的实现者

- Model：模型层，在这里表示 JavaScript 对象
- View：视图层，在这里表示 DOM（HTML 操作的元素）
- ViewModel：连接视图和数据的中间件，Vue.js 就是 MVVM 中的 ViewModel 层的实现者

> 在 MVVM 架构中，是不允许 数据 和 视图 直接通信的，只能通过 ViewModel 来通信，而 ViewModel就是定义了一个 Observer 观察者

- ViewModel 能够观察到数据的变化，并对视图对应的内容进行更新
- ViewModel 能够监听到视图的变化，并能够通知数据发生改变

至此，我们就明白了，Vue.js 就是一个 MVVM 的实现者，他的核心就是实现了 DOM 监听 与 数据绑定

### 3.2 好处

1. 轻量级，体积小是一个重要指标。Vue.js 压缩后有只有 20多kb （Angular 压缩后 56kb+，React压缩后 44kb+）
2. 移动优先。更适合移动端，比如移动端的 Touch 事件
3. 易上手，学习曲线平稳，文档齐全
4. 吸取了 Angular（模块化）和 React（虚拟 DOM）的长处，并拥有自己独特的功能，如：计算属性
5. 开源，社区活跃度高
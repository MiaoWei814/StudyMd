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

MVVM（Model-View-ViewModel）是一种**软件架构设计模式**，由微软 WPF（用于替代 WinForm，以前就是用这个技术开发桌面应用程序的）和 Silverlight（类似于 Java Applet，简单点说就是在浏览器上运行的 WPF） 的架构师 Ken Cooper 和 Ted Peters 开发，是一种简化用户界面的**事件驱动编程方式**。由 John Gossman（同样也是 WPF 和 Silverlight 的架构师）于 2005 年在他的博客上发表。MVVM 源自于经典的 MVC（Model-View-Controller）模式。

> MVVM 的核心是 ViewModel 层，负责转换 Model 中的数据对象来让数据变得更容易管理和使用，

其作用如下：

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

> 跟SOC:关注度分离原则

### 2.3 组成部分

![image-20211005103040512](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211005103040512.png)



> 理解:在View层有个templates表示是模板的意思,比如JSP,而我们取数据就是用$符号取的;这个MVVM模式分了三层,Model放数据,他这个数据会通过ViewModel层跟前端进行绑定!所以说ViewModel它会去监听,比如说model发生变化了那么view也会发生发生变化,在VUE中是虚拟DOM去监听这个事情!

1. View:

   ```
   View 是视图层，也就是用户界面。前端主要由 HTML 和 CSS 和 模板 来构建，为了更方便地展现ViewModel 或者 Model层的数据，已经产生了各种各样的前后端模板语言，比如 FreeMarker、
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

### 3.3 使用

Vue本身就是一个javaScript框架,所以我们需要导入它的JS

这里直接用CDN进行演示:

开发版本:

- 包含完整的警告和调试模式：https://vuejs.org/js/vue.js
- 删除了警告，30.96KB min + gzip：https://vuejs.org/js/vue.min.js

CDN:

- <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>

- <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>

示例:

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>vue</title>
</head>
<body>
    <!--view层,这里就变成了一个模板-->
    <div id="app">
        <!-- 只需要在绑定的元素中使用 双花括号 将 Vue 创建的名为 message 属性包裹起来，即可实现数据绑定功能，也就实现了 ViewModel 层所需的效果-->
        {{message}}
    </div>
</body>
<!--导入Vue.js-->
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    //Vue是一个对象,所以我们需要new一个
    let vue = new Vue({
        //绑定,el:全称:element,代表元素的意思
        el : "#app",
        //model层:数据
        data : {
            //消息
            message:"hello,vue!"  //数据对象中有一个名为 message 的属性，并设置了初始值
        }
    });
</script>
</html>
```

**注意**:这里message可自定义,也可以写成"messageeee"都可以!

View层:

```html
<div id="app">
	{{message}}
</div>
```

Model层:

```javascript
<script>
    let vue = new Vue({
        el : "#app",
        data : {
            message:"hello,vue!"
        }
    });
</script">
```

ViewModel层:
![image-20211005161213901](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211005161213901.png)

**解释**:这个`vue.message`是model,页面显示`哇塞`是视图,而ViewModel将其绑定起来了!我们之前的JS中,我们改变值理论上前后端不绑定,前端正常不刷新不重新加载是不会改变的。HTML写死了不会变数据的；此时我们在控制台直接输入vue.message来修改值，中间是可以省略data的，在这个操作中，我并没有主动操作DOM，就让页面的内容发生了变化，这就是借助了Vue的数据绑定功能实现的,那么这就是`ViewModel`,MVVM模式中要求ViewModel层就是使用观察者模式来实现数据的监听与绑定，以做到数据与视图的快速响应!

> 理解:ViewModel就会读取Model里的对象,因为它会去管理model,然后它跟前端视图View是双向绑定,比如说绑定了这个message的值,这个值在我们vue对象里管理着,我们之前Html发生变化是通过JS中获取DOM元素然后删除它然后进行改变它,一旦前端操作DOM元素一旦过多就会变得非常卡,而VUE不改变DOM元素,它只是把这个值换了,从model中获取而已;

Vue跟原生JS最大的区别就是:前者在不改变DOM对象的情况下去改变值!

> VUE是MVVM具体实现者,它解耦了视图层和数据层!

## 4.语法

### 4.1 v-bind

**引入**:我们已经成功创建了第一个 Vue 应用！看起来这跟渲染一个字符串模板非常类似，但是 Vue 在背后做了大量工作。现在数据和 DOM 已经被建立了关联，所有东西都是响应式的。我们在控制台操作对象属
性，界面可以实时更新！

我们还可以使用`v-bind`来绑定元素特性!

```vue
<body>
<div id="app">
    <!--
        如果要将模型数据绑定在html属性中
        则可以使用 v-bind 指令,此时title中显示的是模型数据
    -->
    <!--这里使用vue将其title属性进行绑定,然后值就会去model中获取-->
   <span v-bind:title="message">
       鼠标悬停几秒查看此处动态绑定的提示信息!
   </span>
    <!-- v-bind 指令的简写形式: 冒号(:) -->
   <h1 :title="message">我是标题</h1>
</div>

</body>
<!--导入Vue.js-->
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    let vue = new Vue({
        el : "#app",
        data : {
            message:"页面加载于"+new Date().toLocaleString()
        }
    });
</script>
```

效果:

![image-20211005172248356](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211005172248356.png)

> 除了使用插值表达式{{}}进行数据渲染，也可以使用 v-bind指令，它的简写的形式就是一个冒号（:）

### 4.2 v-if,v-else

这个就很简单了就是if判断了!

上代码:

```vue
<body>
<div id="app">
    <!--获取type属性判断是否全等于A,如果是则为true-->
    <!-- === 三个等号在 JS 中表示绝对等于（就是数据与类型都要相等）-->
    <h1 v-if="type==='A'">A</h1>
    <h1 v-else-if="type==='B'">B</h1>
    <h1 v-else-if="type==='c'">C</h1>
    <h1 v-else>who</h1>
</div>

</body>
<!--导入Vue.js-->
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    new Vue({
        el:'#app',
        data : {
            type:'aa'
        }
    })
</script>
```

### 4.3 v-for

语法格式:

```vue
<div id="vue">
  <li v-for="item in items">
   	{{ item.message }}
  </li>
</div>
```

注:items是数组,item是数组元素迭代的别名!

下面是实例:

```vue
<body>
<div id="app">
   <li v-for="item in items">
       {{item.message}}
   </li>
</div>

</body>
<!--导入Vue.js-->
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    var vm=new Vue({
        el: '#app',
        data: {
            //items数组
            items: [
                //数组内封装对象
                {message: 'java'},
                {message: 'VUE'}
            ]
        }
    })
```

页面:

![image-20211005194808360](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211005194808360.png)

此时我们若是在控制台输入`vm.items.push({message: '哇塞哇塞'})` ，尝试追加一条数据，你会发现浏览器中显示的内容会增加一条:

![image-20211005195118533](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211005195118533.png)

### 4.4 v-on

v-on:监听事件

事件有Vue的事件、和前端页面本身的一些事件！我们这click是vue的事件，可以绑定到vue中的**methods**中的方法事件

上代码:

```vue
<body>
<div id="app">
    <!--这里通过v-前缀绑定了,那么这里的函数只能去访问Vue的,放在外面其他地方那么是不受控制的-->
    <!--这里使用v-on绑定了click事件,并指定了名为sayHi的方法-->
   <button v-on:click="sayHi">点我</button>

    <!--v-on 指令的简写形式 @-->
    <button @click="sayHi">点我哦</button>
</div>
<!--放在绑定的元素外是没有Vue对象的,也就找不到-->
<button @click="sayHi">点我哦</button>
</body>
<!--导入Vue.js-->
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    var vm=new Vue({
        //el元素绑定必须存在,
        el: '#app',
        data: {
            message:"hello world"
        },
        //方法必须定义在Vue的Methods对象中
        methods:{
            //v-on监听事件,所以这里可以定义一个事件来接收
            sayHi:function(event) {
                //this 代表当前实例,因为我们这里Vue对象绑定了元素,而我们在所在元素内使用Vue对象,所以当前对象是这个实例.
                //而通过实例去调用属性message是没问题的
                alert(this.message);
            }
        }
    })
</script>
```

 事件如果记不住,这里JQuery就提供了:事件https://www.jquery123.com/category/events/browser-events/

> 要注意的点就是我们使用Vue对象要绑定元素,而我们需要在绑定的元素使用对应的实例使用!

### 4.5 v-model

**什么是双向数据绑定?**

Vue.js 是一个 MVVM 框架，即数据双向绑定，即当数据发生变化的时候，视图也就发生变化，当视图发生变化的时候，数据也会跟着同步变化。这也算是 Vue.js 的精髓之处了。

> 值得注意的是，我们所说的数据双向绑定，一定是对于 UI 控件来说的，非 UI 控件不会涉及到数据双向绑定。对于我们处理表单，Vue.js 的双向数据绑定用起来就特别舒服了。

**如何使用?**
你可以用`v-model` 指令在表单 < input>、< textarea> 及 < select> 元元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 v-model 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理

测试代码-input:

```vue
<body>
<div id="app">
    <!--双向绑定:我这里在input框输入的,那么旁边也会跟着变化,那么这就是当视图发生变化,那么数据也会发生变化-->
    <!--使用vi-model进行双向绑定,这里将input框与vue对象中的message属性进行绑定,然后这里message又跟vue中的对象属性进行绑定-->
    <!--
        怎么理解呢?
            这里input框是view视图,下面JS是model数据,而vue.js是viewModel,然后view视图与model数据进行绑定,监听视图发生变化那么model也会发生变化,
            而如果model发生了变化,那么视图层的message和input框也会跟着发生变化,那么这就是双向绑定!
        -->
    输入的文本: <input type="text" v-model="message"> {{message}}
</div>

</body>
<!--导入Vue.js-->
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    var vm=new Vue({
        //el元素绑定必须存在,
        el: '#app',
        data: {
            message:""
        },
    })
</script>
```

演示:

1. 我们在input框输入,同时旁边message也会跟着发生变化:

   ![image-20211006085541453](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211006085541453.png)

2. 我们在控制台对model层的message进行改变,那么view层的数据也都会发生改变:

   ![image-20211006085631439](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211006085631439.png)

> input框跟textarea也是一样的,就不需要进行测试了,接下来测试一下单选框和复选框

单选框测试代码:

```vue
<body>
<div id="app">
  性别:
    <!-- v-model 绑定了视图,通俗的话来说就是绑定了input框就会获取它的值,也就是获取value   -->
    <input type="radio" name="sex" value="男" v-model="message">男
    <input type="radio" name="sex" value="女" v-model="message">女

    <p>
        选中了谁:   {{message}}
    </p>
</div>

</body>
<!--导入Vue.js-->
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    var vm=new Vue({
        //el元素绑定必须存在,
        el: '#app',
        data: {
            message:""
        },

    })
</script>
```

演示:

![image-20211006091143717](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211006091143717.png)

下拉框测试代码(JS都一样就不展示了):

```vue
<div id="app">
    <select v-model="message">
        <option disabled value="">请选择</option>
        <option>A</option>
        <option>B</option>
        <option>C</option>
    </select>
    <span>选中的值: {{message}}</span>
</div>
```

演示:

![image-20211006093049201](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211006093049201.png)

**注意**:如果 v-model 表达式的初始值未能匹配任何选项，< select> 元素将被渲染为“未选中”状态。在 iOS 中，这会使用户无法选择第一个选项。因为这样的情况下，iOS 不会触发 change 事件。因此，更推荐像上面这样提供一个值为空的禁用选项

多复选框测试代码:

```vue
<body>
  <div id="app">
   多复选框：
    <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
    <label for="jack">Jack</label>
      
    <input type="checkbox" id="john" value="John" v-model="checkedNames">
    <label for="john">John</label>
      
    <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
    <label for="mike">Mike</label>
      
    <span>选中的值: {{ checkedNames }}</span>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js">
</script>
  <script>
    new Vue({
      el: '#app',
      data: {
        checkedNames: []
     }
   })
  </script>
</body>
```

演示:

![image-20211006093525512](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211006093525512.png)

## 5.组件

**什么是组件?**

​	组件是可**复用**的vue**实例**,说白了就是一组可以**重复使用**的**模板**,跟JSTL的自定义标签、Thymeleaf的th:fragment等框架有着异曲同工之妙;

通常一个应用会以一颗嵌套的组件树的形式来组织:

![image-20211006094201140](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211006094201140.png)

比如:你可能会有页头、侧边栏、内容区等组件,每个组件又包含了其他的像导航链接、博文之类的组件，为了不出现大量重复的代码，那么我们应该将其抽取出来放在公共区！

### 5.1 vue.component

接下来将使用这种方式开发组件,但实际开发中不会这样子做,只是临时写一下理解一下:

```vue
<body>

<div id="app">
    <ul>
        <!--使用组件    -->
        <!-- 有点类似自定义标签 -->
        <miao-da-wei></miao-da-wei>
    </ul>
</div>
</body>
<!--导入Vue.js-->
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>

    //定义一个Vue组件component
    //定义组件名称
    Vue.component('MiaoDaWei', {
        //创建模板
        template: '<li>Hello</li>'
    })

    var vm = new Vue({
        //el元素绑定必须存在,
        el: '#app'
    })
</script>
```

就是我们定义一个组件,然后这个组件我们就可以用使用标签的形式进行使用,但是这个组件我们必须在Vue对象绑定的元素内,因为这毕竟是Vue对象的而不是其他的,放在外面就没用了!

**注意**:

1. 定义组件必须放在Vue绑定的元素内使用!
2. 我们在定义组件名字的时候,首字母可以大小写,后面的字母也可以大小写,但是在DOM中使用的时候大写字母前面需要**用"-"隔开**,并且使用的时候不区分大小写,只需要注意大写字母前加**-**即可,

> 我们使用组件的标签在DOM中,就可以自动替换了组件中的template中的内容!

### 5.2 props

我们光写一个组件是远远不够的,我们得让它联动起来,所以我们使用`props`进行参数的接收;

代码示例:

```vue
<body>

<div id="app">
    <ul>
        <!--使用组件    -->
        <!--
            这里进行循环遍历,然后我们需要将循环的元素跟我们的模板进行绑定,
            注意:我们不能直接用item去放在我们模板里面,我们得想办法将其绑定起来!
        -->
        <!--这里for循环,将元素item进行绑定到com这个参数,然后将值传递给组件中的值,然后在组件通过props进行接收参数-->
        <miao-da-wei v-for=" item in items" v-bind:com="item"></miao-da-wei>
    </ul>
</div>
</body>
<!--导入Vue.js-->
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>

    //定义一个Vue组件component
    Vue.component('MiaoDaWei', {
        //props:接收参数
        props:['com'],
        //接收到参数后就可以使用该参数
        template: '<li>{{com}}</li>'
    })
//思考?我们现在需要将下面的数组内的数据放在组件中的template内的模板内
    var vm = new Vue({
        //el元素绑定必须存在,
        el: '#app',
        data:{
            //定义一个数组
            items : ["java","Linux","C++"]
        }
    })
</script>
```

说明:首先我们遍历出来的数据是items里的item,这个时候数据遍历就结束了,这个时候我们需要将遍历出来的数据传给组件中去,这个时候我们将数据item绑定到组件参数中的com,通过这个com又跟组件中的props接收参数的com进行绑定,我们就可以使用循环中的item参数了!

> 理解:其实这里的props说传递参数或者接收参数都可以,其作用都是将参数跟循环中的元素进行绑定!只要已绑定我们就能获取到循环中的元素!而我们知道绑定的属性是产生双向绑定的效果!

## 6.Axios异步通信

**什么是Axios:**

​	Axios是一个开源的可以用在浏览器端和NodeJS的异步通信框架!

**作用:**

​	主要作用就是实现AJAX异步通信,其功能特点如下:

- 从浏览器中创建XMLHttpRequests异步请求
- 从node.js创建http请求
- 支持Promise API【Js中的链式编程】
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换JSON数据
- 客户端支持防御XSRF(跨站请求伪造)

**为什么要使用Axios:**

​	由于Vue.js是一个视图层框架,并且作者(尤玉溪)严格遵守SOC(关注度分离原则),所以Vue.js并不包含AJAX的通信功能,为了解决通信问题,作者单独开发了一个名为vue-resource的插件,不过在进入2.0版本以后停止了对该插件的维护并推荐了Axios框架,不建议使用或者少用JQuery,因为它操作DOM太频繁!

> 使用Axios那么我们浏览器必须支持ECMAScript6,否则不支持,而我们如果使用IDEA那么我们也要将JavaScript默认的版本改为6

### 6.1 Vue的生命周期

Vue实例有一个完整的生命周期,也就是从开始创建、初始化数据、编译模板、挂载DOM、渲染->更新->渲染、卸载等一系列过程,我们称这是Vue的生命周期,通俗说就是Vue实例从创建到销毁的过程,就是生命周期!

在Vue的整个生命周期中,它提供了一系列的事件,它可以让我们在事件触发时注册JS方法,可以让我们用自己注册的JS方法控制整个大局,在这些事件响应方法中的this直接指向的是Vue的实例;

![image-20211006141335242](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211006141335242.png)

解读:我们在`new Vue()`在实例化的时候并没有真正的去渲染,然后在创建实例化的时候就会立马去创建`钩子函数`,然后在初始化注入后跟在`el`判断是否存在之前还有一个事情就是创建`钩子`,而这些函数人家已经提供好了,我们只需要放在那里去使用,比如在初始化注入之前有个钩子函数是`beforeCreate`,我们就能直接使用;然后当el创建好了之后就要往里面渲染template数据,这个时候就要判断是否有template选项,如果有,那么就把数据插进去,然后编译成模板,然后将编译好的HTML替换掉el属性所指向的DOM,挂载之后,就是实时监听!

### 6.2 快速使用

了解了生命周期,那么我们就直接使用`mounted`函数进行发起ajax请求:

实例代码:

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>vue</title>
<!--  c-clock解决页面刷新闪烁出视图层的模板  -->
    <style>
        /*属性选择器*/
        [v-cloak]{
            /*让其白屏*/
            display: none;
        }
    </style>
</head>
<body>

<div id="app" v-cloak>
    <!--获取属性-->
    <div>{{info.name}}</div>
    <!--获取对象中属性-->
    <div>{{info.address.city}}</div>
    <!--数组循环遍历-->
    <div v-for="link in info.links">{{link.name}}</div>
    <!--绑定在标签中-->
    <a v-bind:href="info.url">点我</a>
</div>
</body>
<!--导入Vue.js-->
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<!--导入axios-->
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>

    var vm = new Vue({
        //el元素绑定必须存在,
        el: '#app',
        //这个函数在将编译好的HTML挂载到页面完成执行的事件钩子
        mounted() {  //钩子函数,就是程序执行可以插入进去执行的函数
            //参数是url,链式编程,这是ES6的新特性 "=>"
            //得到它然后进行响应,将获取到json数据赋值给下面的data()方法中info参数
            axios.get('data.json').then(response => (this.info=response.data));
        },
        //之前的data是vm的属性,而这里data()是函数式方法,而其中return就是用于接收上面异步请求响应的数据,然后将其return到data数据里面
        data(){
            return{
                //这里请求的返回参数必须跟json字符串中的参数保持一致!
                info:{
                    //这里临时写几个不写完
                    name:null,
                    url:null,
                    address : {
                        street:null,
                        city:null,
                        country:null
                    },
                    links:[
                        {
                            name : null,
                            url: null
                        }
                    ]
                }
            }
        }
    })
</script>
</html>
```

data.json:

```json
{
  "name": "测试Axios",
  "url": "https://www.baidu.com",
  "page": 1,
  "isNo": true,
  "address": {
    "street": "洪崖洞",
    "city": "重庆市",
    "country": "中国"
  },
  "links": [
    {
      "name": "张三",
      "url": "www.baidu.com"
    },
    {
      "name": "李四",
      "url": "www.baidu.com"
    },
    {
      "name": "王二",
      "url": "www.baidu.com"
    }
  ]
}
```

我将所有的注释都写在代码中,其中要注意的地方就是

1. 版本问题,使用链式编程那么必须版本在ESC6
2. 在info中的参数只是用于接收json中的参数,如果写错那么在页面上不显示,注意:这里的null并不是默认值,你写其他数值都是一样的!
3. 使用 axios 框架的 get 方法请求 AJAX 并自动将数据封装进了 Vue 实例的数据对象中
4.  我们在data中的数据结构必须要和 Ajax 响应回来的数据格式匹配!

页面效果:

![image-20211006152454487](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211006152454487.png)

## 7.计算属性

计算属性是VUE的一大特色!

​	计算属性的重点突出在**属性**这两个字上(属性是名词),首先它是个属性其次这个属性有**计算**的能力(计算是动词),这里的**计算**就是个函数;简单来说:它就是一个能够计算结果缓存起来的属性(将行为转化成了静态的属性),仅此而已,可以想象为缓存!

> 计算属性:计算出来的结果,保存在属性中~ 好处:内存中运行,虚拟DOM

示例:

```vue
<div id="app">
    <!--调用方法-->
    <p>当前时间1: {{currentTime1()}}</p>
    <!--调用属性-->
    <p>当前时间2: {{currentTime2}}</p>
</div>
</body>
<!--导入Vue.js-->
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>

    var vm = new Vue({
        //el元素绑定必须存在,
        el: '#app',
        data : {
            message:"hello,Miao"
        },
        methods : {
            currentTime1:function (){
                return new Date().toLocaleString();
            }
        },
        //计算属性,这里跟methods是一样的
        computed : {
            //computed跟currentTime1不能出现重名,重名之后,只会调用methods中的方法
            currentTime2:function (){
                this.message; //这个发生改变那么当前属性的缓存也会被重新刷新
                //这个跟mybatis的缓存很像,mybatis一旦遇到增删改查那么就会立即刷新缓存
                return Date.now(); //被缓存起来了,控制台获取这个属性每次获取都是不变的

                //好处:这样的缓存就是应对大量的方法区调用浏览器就会卡死,而通过内存去调用速度就会变得非常快 
            }
        }
    })
</script>
```

演示:

![image-20211006161343172](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211006161343172.png)

然后我们控制台做出以下测试:

![image-20211006161358552](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211006161358552.png)

**解释**:我们第二次获取属性中时间戳,然后跟第一次没变化,说明被缓存起来了,然后我们修改了message属性,那么这个时候整个缓存就被刷新起来了,那么又会重新缓存!这个跟mybatis的一级缓存很相似,

**注意**:methods和computed里的东西不能重名

**说明:**

1. methods:定义方法,调用方法使用currentTime1(),注意是需要带括号的;
2. computed:定义计算属性,调用属性使用currentTime2,注意这是个属性而不是方法,所以不需要带括号;this.message是为了能够让currentTime2观察到数据变化而变化
3. 如果在方法中的值发生了变化,则缓存就会刷新!可以在控制台使用`vm.message="xxx"`改变数据的值,那么就会看见`currentTime2`的变化

**结论:**

​	调用方法时,每次都需要进行计算,既然有计算过程则必定产生系统开销,那如果这个结果是不经常变化的呢?此时就可以考虑将这个**结果缓存**起来,采用计算属性可以很方便的做到这一点

> 计算属性的主要特性就是为了将不经常变化的计算结果进行缓存,以节约我们的系统开销!

## 8.内容分发-插槽

### 8.1 简单使用

在 Vue 中我们使用`<slot>` 元素，作为承载分发内容的出口，作者称其为 插槽，可以应用在组合组件的场景中;

> 理解:就是将原来动态的东西进行替换,这个插槽好比就是占位符,我们需要将其组合替换;

先看示例代码:

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>插槽</title>
</head>
<body>

<div id="app">
    <!--
        一般这样的都会从服务器中获取数据,但是像url标签就无法进行遍历
    -->
    <p>列表书籍</p>
    <ul>
        <li>Java</li>
        <li>Java</li>
    </ul>
    <hr>
<!-- 这是组件   -->
    <todo>
<!--  这里将插槽进行绑定,这里:title只是v-bind的简写形式      -->
        <todo-title slot="todo-title" :title="title"></todo-title>
<!--  这里遍历获取的是Vue实例中的属性,然后跟组件中的props进行绑定,      -->
<!--  这里循环可以写成(item, index) in todoItems, 其中index是下标的意思      -->
        <todo-items slot="todo-items" v-for="item in todoItems" :item="item"></todo-items>
    </todo>
</div>
</body>
<!--导入Vue.js-->
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    Vue.component("todo",{
        template :
        //这里也可以使用\反斜杠来代替换行符,比如<div>\<slot></slot>中间就省略了换行导致的拼接+
        //这里定义了slot插槽,用于插数据
            '<div>'+
            /*这里留了一个插槽,也就是上面p标签变空了*/
                    /*这里为这个插槽定义一个名称*/
                '<slot name="todo-title"></slot>'+
                '<ul>'+
                    /* 这里插槽就替换了上面的li标签*/
                    '<slot name="todo-items"></slot>'+
                '</ul>'+
            '</div>'
    })

    Vue.component('todo-title',{
        props:['title'],
        template :'<div>{{title}}</div>'
    })
    Vue.component('todo-items',{
        props : ['item'],
        template :'<li>{{item}}</li>'
    })


    var vm = new Vue({
        //el元素绑定必须存在,
        el: '#app',
        data : {
            title: '书籍列表',
            //只是将写死的数据从Vue对象中获取
            todoItems : ['测试1','测试2']
        }
    })
</script>
</html>
```

页面演示:

![image-20211006171907389](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211006171907389.png)

解释:

这里我将每一步步骤进行解析:

1. 定义一个书籍列表的组件:

   ```html
   <script> //这里写script这是用于让代码有颜色好看
   Vue.component("todo",{
           template :
               '<div>'+
                   '<slot name="todo-title"></slot>'+
                   '<ul>'+
                       '<slot name="todo-items"></slot>'+
                   '</ul>'+
               '</div>'
   })
   </script>
   ```

   这里就是一个总组件,然后针对其中动态的数据进行用插槽来代替,我们要让标题和值进行动态的绑定

2. 我们定义标题和内容的组件:

   ```vue
   <script> //这里写script这是用于让代码有颜色好看
   // 标题
   Vue.component('todo-title',{
           props:['title'],
           template :'<div>{{title}}</div>'
       })
   //内容
   Vue.component('todo-items',{
       props : ['item'],
       template :'<li>{{item}}</li>'
   })
   </script>
   ```

   这里我们定义两个组件,用于替换插槽,然后我们既然是替换那么数据就不能写死,我们可以进行动态的绑定!所以这里用props进行参数的传递

3. 实例化Vue并初始化数据

   ```vue
   <script> //这里写script这是用于让代码有颜色好看
   var vm = new Vue({
           //el元素绑定必须存在,
           el: '#app',
           data : {
               title: '书籍列表',
               //只是将写死的数据从Vue对象中获取
               todoItems : ['测试1','测试2']
           }
       })
   </script> 
   ```

   这是我们定义初始化数据,定义标题和内容

4. 将数据同插槽进行动态插入:

   ```vue
   <todo>
       <!--注意:这里:title是跟组件中props参数进行绑定,所以获取到Vue中实例然后传递给组件-->
       <todo-title slot="todo-title" :title="title"></todo-title>
       <todo-items slot="todo-items" v-for="item in todoItems" :item="item"></todo-items>
   </todo>
   ```

   首先是总的组件,因为总组件里的插槽是需要我们替换的,所以我们另外的两个组件也应该放在其中,然后我们通过*slot*跟插槽中的name进行绑定,然后此时我们获取Vue实例中的属性值然后跟组件中的*props*参数进行绑定!

> 给我的感觉插槽就是使用组件进行动态的替换组件中某些动态内容!就好比是模板!

### 8.2 自定义事件

我们在使用上述插槽的时候,我们仅仅用于展示出来, 我们通过不同的插槽组件进行替换,而组件中用props进行参数传递,现在我们需要的如何进行动态删除?

我先展示完整的代码:

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>插槽-自定义事件</title>
</head>
<body>

<div id="app">
    <todo>
        <todo-title slot="todo-title" :title="title"></todo-title>
        <!--增加了 v-on:remove="removeTodoItem(index)" 自定义事件，该事件会调用 Vue 实例中定义的名为 removeTodoItem 的方法-->
        <!--这里remove为自定义事件名,然后调用Vue实例中的方法,这里key进行绑定自定义事件分发的参数-->
        <!--
			slot="todo-items":绑定插槽
			v-for="(item,index) in todoItems":获取实例中的数据项进行循环,这里不仅获取循环的元素和下标
			:item="item" :index="index": 这里前面的item和index指的是组件中的props的参数,后面的item和index指的循环的元素和下标
			v-on:remove="removeItem(index)":绑定事件,remove:自定义事件名跟组件中自定义事件名保持一致,removeItem(index)指Vue实例中的函数方法
			:key="index":绑定参数key为index,这里index为组件中自定义事件传递的参数名
		-->
        <todo-items slot="todo-items" v-for="(item,index) in todoItems" :item="item" :index="index" v-on:remove="removeItem(index)" :key="index"></todo-items>
    </todo>
</div>
</body>
<!--导入Vue.js-->
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    Vue.component("todo",{
        template :
            '<div>'+
                '<slot name="todo-title"></slot>'+
                '<ul>'+
                    '<slot name="todo-items"></slot>'+
                '</ul>'+
            '</div>'
    })

    Vue.component('todo-title',{
        props:['title'],
        template :'<div>{{title}}</div>'
    })
    Vue.component('todo-items',{
        props : ['item','index'],
        //绑定事件函数,我们这里当前组件只能绑定当前事件,不能用这里的组件去调用Vue实例里的函数,会找不到!
        template :'<li>{{index}} ~ {{item}}<button @click="remove">删除</button></li>',
        methods : {
            remove : function(index) {
                //我们要通过这个函数去操作Vue实例中节点
                //这里我们定义自定义事件 , emit自定义事件分发
                // 这里的 remove 是自定义事件的名称，需要在 HTML 中使用 v-on:remove 的方式指派
                this.$emit('remove',index)
            }
        }
    })


    var vm = new Vue({
        //el元素绑定必须存在,
        el: '#app',
        data : {
            title: '书籍列表',
            todoItems : ['测试1','测试2']
        },
        methods : {
            // 该方法可以被模板中自定义事件触发
            removeItem : function(index) {
                console.log("删除 " + this.todoItems[index] + " 成功");
                //这里使用JS中的splice()方法区删除,有三个参数:第一个是指定下标,第二个参数是删除多少个元素, 第三个参数是一个可变参数,表示添加的元素
                this.todoItems.splice(index, 1);
            }
        }
    })
</script>
```

接下来解释每一步步骤和思路:

1. 在vue的实例中,增加了 methods 对象并定义了一个名为 removeTodoItem 的方法用于删除数组元素

   ```vue
   <script>
   var vm = new Vue({
           //el元素绑定必须存在,
           el: '#app',
           data : {
               title: '书籍列表',
               todoItems : ['测试1','测试2']
           },
           methods : {
               // 该方法可以被模板中自定义事件触发
               removeItem : function(index) {
                   console.log("删除 " + this.todoItems[index] + " 成功");
                   //这里使用JS中的splice()方法区删除,有三个参数:第一个是指定下标,第二个参数是删除多少个元素, 第三个参数是一个可变参数,表示添加的元素
                   this.todoItems.splice(index, 1);
               }
           }
       })
   </script>
   ```

2. 修改todo-items组件中的代码,增加一个删除按钮,并且我们绑定一个事件,在事件里我们增加一个自定义事件进行参数传递与事件分发

   ```vue
   <script>
   Vue.component('todo-items',{
           props : ['item','index'],
           //绑定事件函数,我们这里当前组件只能绑定当前事件,不能用这里的组件去调用Vue实例里的函数,会找不到!
           template :'<li>{{index}} ~ {{item}}<button @click="remove">删除</button></li>',
           methods : {
               remove : function(index) {
                   //我们要通过这个函数去操作Vue实例中节点
                   //这里我们定义自定义事件 , emit自定义事件分发
                   // 这里的 remove 是自定义事件的名称，需要在 HTML 中使用 v-on:remove 的方式指派
                   this.$emit('remove',index)
               }
           }
       })
   </script>
   ```

   > 这里就比较关键了,这里的目的与作用就是给删除按钮绑定一个事件,通过当前组件调用当前函数方法,然后在方法内我们想要去操作Vue实例中的数据项,那么就采用自定义事件来解决!

3. 修改 todo-items内容组件的 HTML 代码,增加一个自定义事件,比如叫 remove,可以和组件的方法绑定,然后绑定到vue的方法中!

   ```vue
    <todo-items slot="todo-items"  v-for="(item,index) in todoItems" 
                :item="item" :index="index" v-on:remove="removeItem(index)" :key="index"></todo-items>
   ```

   因为在组件中我们添加了一个自定义事件,然后既然是事件那么我们这里进行绑定该事件,然后通过自定义事件调用Vue实例中的函数方法,传递参数是index;

> 通过以上代码不难发现，数据项在 Vue 的实例中，但删除操作要在组件中完成，那么组件如何才能删除Vue 实例中的数据呢？此时就涉及到参数传递与事件分发了，Vue 为我们提供了自定义事件的功能很好
> 的帮助我们解决了这个问题；使用 this.$emit('自定义事件名', 参数)

**逻辑理解**:

![image-20211006213605016](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211006213605016.png)

这里前端是可以直接调用Vue对象中的方法,因为这里前端跟Vue对象是绑定了的,通过`el:#APP`进行绑定;而组件跟前端也是绑定了的,是通过插槽的方式`<slot>`进行绑定;而我们想从组件3中进行删除Vue对象中的数据项,但是Vue是不能通过一个组件去调用Vue对象中去,那怎么办呢?我们只能通过前端这个东西进行搭桥,通过`this.#emit`跟前端自定义事件进行绑定,然后前端这个自定义事件的值就去调用Vue对象中的方法!

> 自定义事件用于将组件和Vue对象进行绑定在一起,进行调用!

## 9.小结

**核心**:以数据驱动并且组件化

> 数据驱动:React的虚拟DOM, 组件化:Angular的模块化

**优点**:借鉴了AngularJS的模块化开发和React的虚拟DOM,虚拟DOM就是把DOM操作放在内存中执行;

**常用的属性**:

- v-if
- v-else-if
- v-else
- v-for
- v-on 绑定事件,简写`@`
- v-model 数据双向绑定
- v-bind 给组件绑定参数,简写`:`

**组件化**:

- 组合组件slot插槽
- 组件内部绑定事件需要使用到`this.$emit('事件名',参数)`
- 计算属性的特色,缓存计算数据

> 遵循SoC关注度分离原则,Vue是纯粹的视图框架,并不包含,比如Ajax之类的通信功能,为了解决通信问题,我们需要使用Axios框架做异步通信;

**说明**:

​	Vue的开发都是要基于NodeJS,实际开发采用vue-cli脚手架开发,vue-router路由,vuex做状态管理;Vue UI界面我们一般使用ElementUI(饿了么出品),或者ICE(阿里巴巴出品)来快速搭建前端项目

官网:

- https://element.eleme.cn/#/zh-cn
- https://ice.work



## 10.Vue-Cli

vue-cli 官方提供的一个脚手架,用于快速生成一个vue的项目模板!

> 脚手架:预先定义好的目录结构及基础代码,就好比咱们在创建Maven项目时可以选择创建一个骨架项目,这个骨架项目就是脚手架,它使我们的开发更加的快速!

![image-20211006221809378](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211006221809378.png)

主要的功能:

- 统一的目录结构
- 本地调试
- 热部署
- 单元测试
- 集成打包上线

> node.js 是一个基于 Chrome V8 引擎的 JavaScirpt 运行环境。简单的说 Node.js 就是运行在服务端的 JavaScript。一种javascript的运行环境，能够使得javascript脱离浏览器运行。

npm:

```
引入:
当一个网站依赖的代码越来越多，程序员发现这是一件很麻烦的事情：
去 jQuery 官网下载 jQuery
去 BootStrap 官网下载 BootStrap
去 Underscore 官网下载 Underscore
……
npm横空出世:用一个工具把这些代码集中到一起来管理吧,这个工具就是他用 JavaScript （运行在 Node.js 上）写的 npm，全称是 Node Package Manager
```

npm具体的步骤:

NPM 的思路大概是这样的：

1. 买个服务器作为代码仓库（registry），在里面放所有需要被共享的代码
2. 发邮件通知 jQuery、Bootstrap、Underscore 作者使用 npm publish 把代码提交到 registry 上，分别取名 jquery、bootstrap 和 underscore（注意大小写）
3. 社区里的其他人如果想使用这些代码，就把 jquery、bootstrap 和 underscore 写到 package.json 里，然后运行 npm install ，npm 就会帮他们下载代码
4. 下载完的代码出现在 node_modules 目录里，可以随意使用了

> 这些可以被使用的代码被叫做「包」（package），这就是 NPM 名字的由来：Node Package(包) Manager(管理器)。

### 10.1 需要的环境

- Node.js:http://nodejs.cn/download/
- Git:https://git-scm.com/downloads

下载node.js自带了npm,所以我们这里可以直接打印npm的版本

确认node.js安装成功,我们在cmd命令里输入:*node -v*和*npm -v*

![image-20211006223136303](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211006223136303.png)

> 这个npm,就是一个软件包管理工具,就和maven一样,这个npm里面有好多的软件,我们要的话直接放依赖就可以了

- 安装Node.js淘宝镜像加速器(cnpm),这样下载速度会得到极大的提升!

```properties
# -g就是全局安装
npm install cnpm -g
# 或每次使用的时候添加如下语句就可以解决 npm速度慢的问题
npm install --registry=https://registry.npm.taobao.org
```

> 我们一般使用npm去安装东西,如果安装失败,我们就可以考虑cnpm,优先级依然是npm

安装:

![image-20211007085623114](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211007085623114.png)

那么我们可以去对应的安装目录查看:

![image-20211007085739061](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211007085739061.png)

好以后我们安装就可以直接使用`cnpm`来进行安装,因为这个使用的是国内淘宝镜像,速度比直接使用`npm`访问国外要快许多

- 安装vue-cli

```properties
cnpm install vue-cli -g
# 测试是否安装成功
# 查看可以基于哪些模板创建 vue 应用程序，通常我们选择 webpack
vue list
```

安装完毕后在文件夹中可以查看:

![image-20211007090724111](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211007090724111.png)

接下来我们查看有哪些模板?

![image-20211007090926868](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211007090926868.png)

### 10.2 快速使用

1. 创建一个空的文件夹`vue-cli`;

2. 创建一个基于 webpack 模板的 vue 应用程序

   ```properties
   # 这里的 myvue 是项目名称，可以根据自己的需求起名
   # vue 初始化一个 webpack项目,这个项目名字叫 myvue
   vue init webpack myvue
   # 一路都选择no即可;
   ```

   这是输入的:

   ![image-20211007092554804](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211007092554804.png)

   说明:

   - Project name：项目名称，默认 回车 即可
   - Project description：项目描述，默认 回车 即可
   - Author：项目作者，默认 回车 即可
   - Install vue-router：是否安装 vue-router，选择 n 不安装（后期需要再手动添加)
   - Use ESLint to lint your code：是否使用 ESLint 做代码检查，选择 n 不安装（后期需要再手动添加）
   - Set up unit tests：单元测试相关，选择 n 不安装（后期需要再手动添加）
   - Setup e2e tests with Nightwatch：单元测试相关，选择 n 不安装（后期需要再手动添加）
   - Should we run npm install for you after the project has been created：创建完成后直接初始化，选择 n，我们手动执行;运行结果!

3. 执行完毕看文件夹:

   ![image-20211007092745179](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211007092745179.png)

4. 初始化运行

   ```properties
   # 切换到myvue文件夹下
   cd myvue 
   # 安装依赖
   npm install
   # 运行启动
   npm run dev
   ```

   注:安装依赖它是根据项目里有个文件`package.json`,我们点进去看全都是版本管理就跟maven的版本管理一样,它就会根据这个去安装依赖;

   ![image-20211007093241216](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211007093241216.png)

   我们等待依赖安装完毕那么这个项目就初始化完成了!

   启动完毕我们就执行`npm run dev`,

   如果发现这一行:

   *webpack-dev-server --inline --progress --config build/webpack.dev.conf.js*

   表示要打包之后才能启动,它现在就在做打包这个事情!

   ![image-20211007094117073](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211007094117073.png)

> 我们浏览器访问这地址我们就能看到项目了,而我们看8080这就是node的服务器,跟tomcat是一样的,node.js它是一个服务,他可以运行一些东西通过npm去运行安装依赖

关闭服务在DOC命令使用快捷键`Ctrl+C`关闭服务!

1. 目录结构

   ![image-20211007095915830](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211007095915830.png)

   - build 和 config：WebPack 配置文件,如果要改端口就去config文件夹下的index.js修改
   - node_modules：用于存放 npm install 安装的依赖文件
   - src： 项目源码目录
   - static：静态资源文件
   - .babelrc：Babel 配置文件，主要作用是将 ES6 转换为 ES5
   - .editorconfig：编辑器配置
   - eslintignore：需要忽略的语法检查配置文件
   - .gitignore：git 忽略的配置文件
   - .postcssrc.js：css 相关配置文件，其中内部的 module.exports 是 NodeJS 模块化语法
   - index.html：首页，仅作为模板页，实际开发时不使用
   - package.json：项目的配置文件
     - name：项目名称
     - version：项目版本
     - description：项目描述
     - author：项目作者
     - scripts：封装常用命令
     - dependencies：生产环境依赖
     - devDependencies：开发环境依赖

2. src目录(目录是项目的源码目录，所有代码都会写在这里！):

   ![image-20211007100211250](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211007100211250.png)

   - main.js

     ```js
         // The Vue build version to load with the `import` command
         // (runtime-only or standalone) has been set in webpack.base.conf with an alias.
         import Vue from 'vue'
         import App from './App'
         Vue.config.productionTip = false;
         /* eslint-disable no-new */
         new Vue({
          el: '#app',
          components: { App },
          template: '<App/>'
         });
     ```

     - *import Vue from 'vue'*:ES6 写法，会被转换成 require("vue"); （require 是 NodeJS 提供的模块加载器）
     - *import App from './App'* ：意思同上，但是指定了查找路径，./ 为当前目录
     - *Vue.config.productionTip = false* ：关闭浏览器控制台关于环境的相关提示
     - *new Vue({...})* ：实例化 Vue
       - *el: '#app'* ：查找 index.html 中 id 为 app 的元素
       - *template: ''* ：模板，会将 index.html 中替换为""
       - *components: { App }* ：引入组件，使用的是 import App from './App' 定义的 App组件

   - App.vue

     ```vue
     <template>
       <div id="app">
         <img src="./assets/logo.png">
         <HelloWorld/>
       </div>
     </template>
     
     <script>
     import HelloWorld from './components/HelloWorld'
     
     export default {
       name: 'App',
       components: {
         HelloWorld
       }
     }
     </script>
     
     <style>
     #app {
       font-family: 'Avenir', Helvetica, Arial, sans-serif;
       -webkit-font-smoothing: antialiased;
       -moz-osx-font-smoothing: grayscale;
       text-align: center;
       color: #2c3e50;
       margin-top: 60px;
     }
     </style>
     ```

     - *template*：HTML 代码模板，会替换 <App/> 中的内容
     - *import HelloWorld from './components/HelloWorld'*：引入 HelloWorld 组件用于替换 template中的 < HelloWorld/>
     - *export default{...}*:导出 NodeJS 对象，作用是可以通过 import 关键字导入
       - *name: 'App'*：定义组件的名称
       - *components: { HelloWorld }*:定义子组件
     - 在hello,Vue中,关于 < style scoped> 的说明：CSS 样式仅在当前组件有效，声明了样式的作用域,是当前的界面私有的!

### 10.3 安装Webpack

​	WebPack是一款模块加载器兼打包工具,它能把各种资源,如JS、JSX、ES6、SASS、LESS、图片等都作为模块来处理和使用

安装:

```properties
# 打包工具
npm install webpack -g
# 客户端
npm install webpack-cli -g
# 以上两个都需要安装
```

测试安装成功:

- webpack -v
- webpack-cli -v

![image-20211007105009001](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211007105009001.png)

配置:

创建`webpack.config.js`配置文件

- entry:入口文件,指定webpack用哪个文件作为项目的入口
- output:输出,指定Webpack把处理完成的文件放置到指定路径
- module:模块,用于处理各种类型的文件
- plugins:插件,如:热更新、代码重用等
- resolve:设置路径指向
- watch:监听,用于设置文件改动后直接打包

```js
module.exports = {
    entry:"",
    output: {
        path:"",
        filename:""
    },
    module:{
        loaders:[
            {test:/\.js$/,loader:""}
        ]
    },
    plugins:{},
    resolve:{},
    watch:true
}
```

直接运行`webpack`命令打包

#### 使用

1. 创建项目

2. 创建一个名为modules的目录,用于放置JS模块等资源文件

3. 在modules下创建模块文件,如hello.js,用于编写JS模块相关代码

   ```js
   //暴露一个方法
   exports.sayHi=function() {
       document.write("<h1>试试ES6规范</h1>")
   }
   ```

4. 在 modules下创建一个名为 main.s的入口文件，用于打包时设置 entry属性

   ```js
   //因为这里引入模块,所以不能写hello.js,直接写名字就可以了
   //hello.js中可以暴露很多个方法,但是这里只用require接收hello这个对象,通过这个对象去调用方法
   var hello = require("./hello");
   //这个hello我们就引进来了,所以我们可以通过这个hello调用hello.js中方法
   hello.sayHi()
   ```

5. 在项目目录下创建 webpack.config.js配置文件，使用 webpack命令打包

   ```js
   //模块导出
   module.exports ={
       //程序入口
       entry:"./modules/main.js",
       //程序打包输出到哪
       output:{
           //规范命名
           filename:"./js/bundle.js"
       }
   }
   ```

6. 我们在终端执行`webpack`这个命令,等待一下我们就能在modules同级目录看到有个js目录下有个buundle.js,说明我们打包成功了!

   接下里我们在测试一下

7. 我们新建一个index.html,然后将打包压缩的js进行导入进来

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>首页</title>
   </head>
   <body>
   <script type="text/javascript" src="dist/js/bundle.js"></script>
   </body>
   </html>
   ```

8. 页面:

   ![image-20211007143642068](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211007143642068.png)

> 以后我们的项目里html就直接导入打包好的JS,无论项目多复杂,只需要webpack打包压缩,然后我们只需导入就OK,这样说明了Vue为什么只有一个主入口,前端工程化了之后就不需要那么多的页面了,就全都是一个一个的Vue组件或者JS组件,这就是前端的模块化开发 

webpack就是一个打包工具,选择从哪个地方的入口自动帮你把这个入口里面所需要的所有东西通过它的方式打包起来运行!



我们可以用**webpack --watch**用于监听变化,实现热部署,只要发生了变化就立马打包好!就不断监听你的操作, 



## 11.vue-router路由

**描述**:Vue Router是Vue.js官方的路由管理器。它和Vue.js的核心深度集成,让构建单页面应用变得易如反掌!

**包含功能**:

- 嵌套的路由/视图表
- 模块化的、基于组件的路由配置
- 路由参数、查询、通配符
- 基于Vue.js过渡系统的视图过渡效果
- 细粒度的导航控制
- 带有自动激活的CSS class的链接
- HTML5历史模式或hash模式,在IE9中自动降级
- 自定义的滚动条行为

> 理解:因为Vue是纯粹的视图层框架,是SoC原则(关注度分离原则),只关注视图层,其他都做不了,所以把跳转页面路由交给了vue-router组件

### 1.安装

vue-router 是一个插件包，所以我们还是需要用 npm/cnpm 来进行安装的。打开命令行工具，进入你的项目目录，输入下面命令:

```properties
#  --save-dev:它会保存到我们开发的配置里面
npm install vue-router --save-dev
```

> 说白了就是安装vue-router到node_modules 里面去,就跟Java的导包一样

如果在一个模块化工程中使用它，必须要通过 Vue.use() 明确地安装路由功能:

```java
//在导入的时候,后面的'vue'是导入的名字,前面的Vue是导入进来的名字,也就是说后面的名字我们不能乱改,但是前面的名字我们可以修改

//这里是从模块里获取的,而这个模块是去node_modules下去找的
import Vue from 'vue'
//这是从我们自己定义的获取的-当前目录下
import App from './App'
//导入vue-router
import VueRouter from 'vue-router'
// 显示的使用
Vue.use(VueRouter);
```

### 2.使用

1. 定义组件 Content.vue 组件

   ```vue
   <template>
     <h1>内容页</h1>
   </template>
   
   <script>
   /*
      这里定义导出,那其他地方如何导入呢?
      import Content from './compoents/Content'
    */
   export default {
     name: "Content"
   }
   </script>
   <!--
     scoped:作用域,相当于说这个页面的style只在当前模板中有效,
   -->
   <style scoped>
   
   </style>
   ```

2. 我们现在新建一个Main.vue组件

   ```vue
   <template>
   <h1>首页</h1>
   </template>
   
   <script>
   export default {
     name: "main"
   }
   </script>
   
   <style scoped>
   
   </style>
   ```

3. 新建文件夹用于专门存放路由,并且写入index.js

   ```js
   import Vue from "vue";
   //导入路由插件
   import VueRouter from "vue-router"
   import Content from "../components/Content";
   import Main from "../components/main";
   //安装路由 显示声明使用VueRouter
   Vue.use(VueRouter);
   
   //这个文件就是专门的路由文件
   
   //配置导出路由,导出一个默认的对象,
   export default new VueRouter({
     routes : [
       {
         //路由路径  相当于后端的@RequestMapping
         path : "/content",
         //路由的名字
         name : "content",
         //跳转的组件
         component : Content
       },{
         //路由路径
         path : "/main",
         //路由的名字
         name : "main",
         //跳转的组件
         component : Main
       }
     ]
   })
   ```

4. 在 **main.js** 中配置路由

   ```js
   //从vue组件里导入过来
   import Vue from 'vue'
   import App from './App'
   //导入我们刚刚写的路由配置
   import router from './router/index'  //我们可以不用写index,因为它会自动扫描里面的路由配置,去默认加载index
   
   Vue.config.productionTip = false
   
   new Vue({
     el: '#app',
     //配置路由,我们只需要将router给它就好了,他会自动的去做,类似于我们springBoot启动类上的注解@SpringBootApplication
     router,
     components: { App },
     template: '<App/>'
   })
   ```

5. 在 **App.vue** 中使用路由

   ```vue
   <template>
     <div id="app">
   <!--    <h1>APP</h1>-->
       <!--
         to可以理解为href属性
       -->
       <h1>Vue-Router</h1>
       <!--router-link： 默认会被渲染成一个 <a> 标签，to 属性为指定链接-->
       <router-link to="/main">首页</router-link>
       <router-link to="/content">内容页</router-link>
       <!--展示template模板-->
       <router-view></router-view>
     </div>
   </template>
   
   <script>
   
   export default {
     name: 'App'
   }
   </script>
   
   <style>
   #app {
     font-family: 'Avenir', Helvetica, Arial, sans-serif;
     -webkit-font-smoothing: antialiased;
     -moz-osx-font-smoothing: grayscale;
     text-align: center;
     color: #2c3e50;
     margin-top: 60px;
   }
   </style>
   ```

6. ![image-20211007160747853](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211007160747853.png)

7. 我们启动测试一下:**npm run dev**

   ![image-20211007160654361](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211007160654361.png)

总结:总的来说就是我们有很多组件,然后我们通过一个js进行管理跳转这个组件,然后这个js就被需要的地方引入即可,他会自动去找这个路由!

1. ## 解决路由中带#号问题:

路由模式有两种:

1. hash：路径带 # 符号，如 http://localhost/#/login
2. history：路径不带 # 符号，如 http://localhost/login

我们修改路由配置:

```js
export default new VueRouter({
  mode:"history",
  routes: []
}
```

这样我们看页面路由:

![image-20211007161909219](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211007161909219.png)

2. ## 404问题

   首先我们创建一个组件然后添加到路由中去

   ```vue
   <template>
     <div>
       页面不存在，请重试！
       </div>
   </template>
   
   <script>
   export default {
     name: "NotFount"
   }
   </script>
   
   <style scoped>
   
   </style>
   ```

   添加路由:

   ```js
   import NotFount from "../components/NotFount";
   {
       path : "*",
       component:NotFount
   }
   ```

   看效果:

   ![image-20211007162124254](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211007162124254.png)

## 12.Vue-ElementUI

这是将ElementUI与Vue进行结合,进行实际操作:

### 1.创建工程

1. 创建一个名为hello-vue的工程`vue init webpack hello-vue`
2. 安装依赖，我们需要安装vue-router、element-ui、sass-loader和node-sas5四个插件

```properties
#进入工程目录
cd he11o-vue
#安装vue-router 
npm install vue-router --save-dev
#安装element-ui 
npm i element-ui -s
#安装依赖
npm install
#安装SASS加载器(前端有些东西不能用纯CSS来写,所以需要通过SASS去编译),这里用cnpm是因为npm是国外的有些东西下载不下来
cnpm install sass-loader node-sass --save-dev
#启动测试
npm run dev
```

3. npm命令解释:

   - `npm install moduleName`:安装模块到项目目录下
   - `npm install -g moduleName`:-g的意思是将模块安装到全局,具体的安装到磁盘哪个位置,要看npm config prefix的位置
   - `npm install -save moduleName`:--save的意思是将模块安装到项目目录下,并在package文件的dependencies节点写入依赖,-S为该命令的缩写
   - `npm install --save-dev moduleName`:--save-dev的意思是将模块安装到项目目录下,并在package文件的devDependencies节点写入依赖,-D为该命令的缩写

4. 此时看目录结构:

   ![image-20211007202825376](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211007202825376.png)

5. 页面:

   ![image-20211007202838045](https://gitee.com/miawei/pic-go-img/raw/master/imgs/image-20211007202838045.png)

...接下来就去饿了么UI官网搬砖使用:

https://element.eleme.cn/#/zh-CN/
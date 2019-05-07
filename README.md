# online-store

## 
## 目标：
1. 商城页面主页
2. 商城里的每件商品的详细页面
3. 详细页面购买商品添加到购物车功能
4. 购物车页面
5. 分类
## 
## 一.使用Vue CLI
### 1.介绍：
它是一种快速开发Vue应用程序的方法，不用自己手动搭建webpack. 开箱即用。
[安装使用请看文档。](https://cli.vuejs.org/zh/guide/)

### 2.配置Vue CLI
```
vue create test //创建项目
```

![图片](https://uploader.shimo.im/f/p8fvBZ37FPkcNZCp.png!thumbnail)

2.选择你想要的，按 `回车键` 

3.我们选择自定义

![图片](https://uploader.shimo.im/f/haCY8J64yt04tW66.png!thumbnail)

4.点击空格选中你想要的插件。

1. Babel：一个广泛使用的转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。
2. TypeScript: 着力于解决 JavaScript 语言天生的弱势：静态类型。这样我们调用函数的时候，可以知道这个函数需要的参数是什么，有什么类型，返回值是什么类型，哪些字段是可能空，哪些字段又需要什么样的格式。
3. PWA：PWA 可以将 Web 和 App 各自的优势融合在一起：渐进式、可响应、可离线、实现类似 App 的交互、即时更新、安全、可以被搜索引擎检索、可推送、可安装、可链接。
4. Router：路由管理器,实现单页面应用。
5. Vuex：它采用集中式存储管理应用的所有组件的状态
6. CSS预处理程序：将让我们选择一个预处理程序，以便我们可以更有效地编写CSS
7. Linter / Formatter：将让我们选择一个Linter来保持代码的整洁。

选了router之后会询问是否开启history模式，以及选择css预处理器，根据个人情况选用。
接下来就是eslint选用规则，推荐第一或者第四种.

选择“Lint on save”，这样我们就可以更频繁地捕捉错误.

你更喜欢将Babel、PostCSS、ESLint等配置放在哪里?  保存到本地文件，而不是package.json

然后提醒你要不要保存这个配置设置，yes或no，根据个人情况吧。（我选NO



---

### 3.分析整个目录：

![图片](https://uploader.shimo.im/f/DZTWVHdd8xoyuuBq.png!thumbnail)


**public目录：**

可以放置css文件和图像等静态资产，并通过绝对路径访问它们，但是我们不会这样做。

这个文件夹主要在写代码时候过程中使用（生产环境）

[具体看文档](https://cli.vuejs.org/zh/guide/html-and-static-assets.html#public-%E6%96%87%E4%BB%B6%E5%A4%B9)

在目录下有个index 文件：
```
    <div id="app"></div>
```
这就是程序最终将通过Webpack添加到页面的地方，Webpack是一个标准的模块捆绑器，它接受我们项目的所有文件，并将它们转换成常规的HTML、CSS和JS文件。



**src目录：**

是我们进行所有开发工作的地方。

    1. “assets”是存储全局CSS文件和图像的地方，可以通过相对路径访问。

    2. “components”目录将存储包含应用程序的所有单个文件组件(按钮、卡片、表单等)。“单个文件组件”是一个术语，在本例中，它指以.vue结尾的文件。

    3. “views”目录存储了由组件（相当一个ui）组成的页面，它们是分开的(ui元素和页面分别)。

    4. App.vue作为最基本（根本）的.vue文件，其中包含导航和路由方面和内容输出。

    5. Router.js存储数组的所有线路(即链接)我们将使用的应用程序。每个页面需要一个相对路径(URL),一个名字(这样你可以很容易地把它整个应用程序,而无需记住一个潜在的长相对路径),和一个组件。

    6. store.js存储所有全局状态数据，并处理我们想要对其进行的任何更新。这就是Vuex发挥作用的地方。

    7. main.js创建了一个Vue实例，它将我们的路由(链接)、数据(demo的数据)和app . Vue文件组合在一起，并告诉Webpack在public/index.html中插入什么元素(前面指出的id为“app”的特殊<div>)。



---

## 二.Vuex使用

store.js文件: 将保存我们希望在应用程序中显示的所有数据


Vuex由三部分组成：
    1. state
    2. mutations
    3. actions


---

### 1.State
用一个对象就包含了全部的应用层级状态。至此它便作为一个“唯一数据源“。

每个应用将仅仅包含一个 store 实例。

 Vuex 的状态存储是响应式的，从 store 实例中读取状态最简单的方法就是在**计算属性**中返回某个状态。


1.访问：
  1. this.store.state
  2. 组件访问：this.$store.state.count

2.改变状态：

actions和mutations都有触发事件的功能。

    1. 通过actions通过 commit 都会触发 mutations，不能直接在actions修改
    2. mutations 修改 state状态（内容）


更新状态一般通过commit提交给mutations，而提交commit是在action中使用。
将actions和mutations分开也会使代码更具可重用性。
mutations中只需要关心以某种方式影响状态，而actions可以专注于处理业务逻辑，决定使用哪个commit

### 2.action和commit 
[文档](https://vuex.vuejs.org/zh/guide/mutations.html)

![图片](https://uploader.shimo.im/f/9UyCF2N1qLg3WX7a.png!thumbnail)

当用户在我们的页面上执行操作时），他们会触发或“调度”一个action，该action会commit 给mutations并更新状态，然后刷新存储状态的形式将状态传递回视图。

组件中: 使用dispatch调用action操作。

### 3.Getter：
跟计算属性类似。

但是如果它返回的是函数，它不会进行缓存。


---



## 3.vue文件的组成：
1. template
2. script
3. style

### 1.template
注意事项：
    1. 它必须是最外层。
    2. 在<template>中，不能有多个div元素，一个div就行了，其他元素都在这个div下

### 2.script
它定义该组件的任何数据和行为。

### 3.style
编写CSS的地方，可以是全局或局部范围的，具体取决于您是否在样式标记上包含“scoped”属性. 

标注了就是局部。



---

## 4.填充数据：

在Store.js中填充数据。

从这个[网站](https://www.flickr.com/photos/154265243@N08/sets/72157705568078074/)获取图片素材。

并且把素材保存到 src 目下的 assets 中。




---


## 5.组件访问state

通过计算属性的形式访问：

```
export default {
	  name: 'home',
	  computed: {
	    products() {
	      return this.$store.state.products
	    }
	  }
	};
```



---

## 6.动态加载本地图片:

必须使用 require

```
return require(`../assets/img/products/${product.images[0]}`);
```




---

## 7.v-for key 有什么用？

“key”是某种惟一的标识符(对我们来说是product id)，
它帮助Vue跟踪它正在呈现/更改的数组中的哪个项，以便尽可能高效地运行。
如果某个特定项发生更改，Vue不会重新对整个页面进行重新排序，
而是只更新该特定部分(由“key”标识)。


**不要用下标作为key!!!!**




---

## 8.方法复用

把多个页面用到的方法，抽取到一个公共文件中。

在src目录下：
![图片](https://uploader.shimo.im/f/zgwahi8gI9w8IlOr.png!thumbnail)

文件內：
![图片](https://uploader.shimo.im/f/V8sOgK9mVIEbAwoC.png!thumbnail)


组件引用：

![图片](https://uploader.shimo.im/f/J9qCzVu19JoBirar.png!thumbnail)





---



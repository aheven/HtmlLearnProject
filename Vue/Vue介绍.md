### Vue 介绍

#### 简介

Vue 是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层。

#### 安装

```html
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

##### 声明式渲染

Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统：

```html
<body>
<div id="app">
    <p>{{ message }}</p>
</div>
<script src="javascript/index.js"></script>
</body>
```

```javascript
var app = new Vue({
    el: '#app',
    data: {
        message: 'Hello Vue!'
    }
})
```

除了文本插值，还可以这样来绑定元素 attribute：

```html
<div id="app-2">
    <span v-bind:title="message">
        鼠标悬停几秒钟查看此处动态绑定的提示信息！
    </span>
</div>
```

```javascript
var app = new Vue({
    el: '#app-2',
    data: {
        message: '页面加载于 ' + new Date().toLocaleString()
    }
})
```

`v-bind` attribute 被称为指令。指令带有前缀`v-`，以表示它们是 Vue 提供的特殊的 attribute。

#### 条件与循环

控制切换一个元素是否显示也相当简单：

```html
<div id="app-3">
    <p v-if="seen">现在你看到我了</p>
</div>
<script src="javascript/index.js"></script>
```

```javascript
var app = new Vue({
    el: '#app-3',
    data: {
        seen: true
    }
})
```

`v-for`指令可以绑定数组的数据来渲染一个项目的列表：

```html
<div id="app-4">
    <ol>
        <li v-for="todo in todos">
            {{todo.text}}
        </li>
    </ol>
</div>
```

```javascript
var app = new Vue({
    el: '#app-4',
    data: {
        todos: [
            {text: '学习 JavaScript'},
            {text: '学习 Vue'},
            {text: '整个牛项目'}
        ]
    }
})
```

#### 处理用户输入

为了让用户和你的应用进行交互，可以用`v-on`指令添加一个事件监听器：

```html
<div id="app-5">
    <p>{{message}}</p>
    <button v-on:click="reverseMessage">反转消息</button>
</div>
```

```javascript
var app = new Vue({
    el: '#app-5',
    data: {
        message: 'Hello Vue.js'
    },
    methods: {
        reverseMessage: function () {
            this.message = this.message.split(``).reverse().join(``)
        }
    }
})
```

Vue 还提供了`v-model`指令，它能轻松实现表单输入和应用状态之间的双向绑定。

```html
<div id="app-6">
    <p>{{message}}</p>
    <label>
        <input v-model="message">
    </label>
</div>
```

```javascript
var app = new Vue({
    el: '#app-6',
    data: {
        message: 'Hello Vue!'
    }
})
```

#### 组件化应用构建

组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。

在 Vue 里，一个组件本质上是一个拥有预定义选项的一个 Vue 实例。在 Vue 中注册组件很简单：

```javascript
//定义一个名为todo-item的新组件
Vue.component('todo-item', {
    template: '<li>这是一个待办事项</li>'
})
```

现在可以用它构建另一个组件模板：

```html
<ol>
    <!-- 创建一个 todo-item 组件的实例 -->
    <todo-item></todo-item>
</ol>
```

但是这样会为每个待办项渲染同样的文本，我们应该能从父作用域将数据传到子组件才对。修改一下组件的定义，使之能够接受一个`prop`：

```javascript
Vue.component('todo-item', {
    //自定义一个 attribute
    props: ['todo'],
    template: '<li>{{todo.text}}</li>'
})

var app = new Vue({
    el: '#app-7',
    data: {
        groceryList: [
            {id: 0, text: '蔬菜'},
            {id: 1, text: '奶酪'},
            {id: 2, text: '随便其它什么人吃的东西'}
        ]
    }
})
```

```html
<div id="app-7">
    <ol>
        <todo-item
                v-for="item in groceryList"
                v-bind:todo="item"
                v-bind:key="item.id"></todo-item>
    </ol>
</div>
```

在一个大型应用中，有必要将整个应用程序划分为组件，以使开发更易管理。这里有一个 (假想的) 例子，以展示使用了组件的应用模板是什么样的：

```html
<div id="app">
    <app-nav></app-nav>
    <app-view>
        <app-sidebar></app-sidebar>
        <app-content></app-content>
    </app-view>
</div>
```




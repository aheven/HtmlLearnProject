### Vue 实例

每个 Vue 应用都是通过`Vue`函数创建一个新的 Vue 实例开始的：

```js
var vm = new Vue({
    ...
})
```

#### 数据与方法

当一个 Vue 实例被创建时，它将`data`对象中的所有的 property 加入到 Vue 的响应式系统中。当这些 property 的值发生改变时，视图将会产生“响应”。

```js
//数据对象
var data = { a: 1 }
//将对象加入到Vue实例中
var vm = new Vue({
    data: data
})
//获取这个实例上的property
vm.a == data.a //->true
//设置property也会影响原始数据
vm.a = 2
data.a //->2
//反之亦然
data.a = 3
vm.a //->3
```

值得注意的是只有当实例被创建时就已经存在于`data`中的 property 才是响应式的。如果你添加一个新的 property，比如`vm.b = 'hi'`，则 b  的改动不会触发任何试图的更新。因此如果需要响应式，则需要在开始的时候初始化。

当使用`Object.freeze()`时，将修改现有的 property，意味着响应系统无法在追踪变化。

除了数据 property，Vue 实例还暴露了一些有用的实例 property 与方法。它们都有前缀`$`，以便与用户定义的 property区分开来。

```js
var data = { a: 1}
var vm = new Vue({
    el: `#example`,
    data: data
})
vm.$data === data //->true
vm.$el === document.getElementById('example') //-> true
vm.$watch('a', function (newValue, oldValue){
    // 这个回调将在`vm.a`改变后调用
})
```

##### 实现生命周期钩子

每个 Vue 实例在被创建时都要经过一系列的初始化过程，将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做**生命周期钩子**的函数，这给了用户在不同阶段添加自己的代码的机会：

```js
var vm = new Vue({
    el: '#example',
    data: {
        a: 1
    },
    created: function () {
        console.log('a is: ' + this.a)
    }
})
//output--> a is: 1
```

生命周期有`beforeCreate`、`created`、`beforeMount`、`mounted`、`beforeDestroy`和`destroyed`。

值得注意的是，不要在选项`property`或回调上使用箭头函数，比如`created:() => console.log(this.a)`或`vm.$watch('a',newValue => this.myMethod())`。因为箭头函数并没有`this`，`this`会作为变量一直向上级作用域查找，直到找到位置。

因此箭头函数经常导致出现 `Uncaught TypeError: Cannot read property of undefined` 或 `Uncaught TypeError: this.myMethod is not a function` 之类的错误。

Vue 生命周期图示：

![Vue生命周期图示](https://github.com/aheven/HtmlLearnProject/blob/main/Vue/pic/lifecycle.png)

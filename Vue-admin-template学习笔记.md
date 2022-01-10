### store index.js



Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块 —— 从上至下进行同样方式的分割：

```
const store = createStore({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```

### store getter.js

Vuex 通过 Vue 的插件系统将 store 实例从根组件中 “注入” 到所有的子组件里。且子组件能通过 `this.$store` 访问到。导出为getters

### store app.js

更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的**事件类型 (type) \**和一个\**回调函数 (handler)**。这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数：

```
const store = createStore({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      // 变更状态
      state.count++
    }
  }
})
```

Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作。

```
const store = createStore({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
})
```

Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 `context.commit` 提交一个 mutation，或者通过 `context.state` 和 `context.getters` 来获取 state 和 getters。

```
actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```

如果希望你的模块具有更高的封装度和复用性，你可以通过添加 `namespaced: true` 的方式使其成为带命名空间的模块。当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名。



### setting.js

，`exports` 只辅助 `module.exports` 操作内存中的数据，辛辛苦苦各种操作数据完，累得要死，结果到最后真正被 `require` 出去的内容还是 `module.exports` 的，真是好苦逼啊。



:key的用法：

- **预期**：`number | string | boolean (2.4.2 新增) | symbol (2.5.12 新增)`

  `key` 的特殊 attribute 主要用在 Vue 的虚拟 DOM 算法，在新旧 nodes 对比时辨识 VNodes。如果不使用 key，Vue 会使用一种最大限度减少动态元素并且尽可能的尝试就地修改 / 复用相同类型元素的算法。而使用 key 时，它会基于 key 的变化重新排列元素顺序，并且会移除 key 不存在的元素。

  有相同父元素的子元素必须有**独特的 key**。重复的 key 会造成渲染错误。

  最常见的用例是结合 `v-for`：

  ```
  <ul>
    <li v-for="item in items" :key="item.id">...</li>
  </ul>
  ```

  它也可以用于强制替换元素 / 组件而不是重复使用它。当你遇到如下场景时它可能会很有用：

  - 完整地触发组件的生命周期钩子
  - 触发过渡

  例如：

  ```
  <transition>
    <span :key="text">{{ text }}</span>
  </transition>
  ```

  当 `text` 发生改变时，`<span>` 总是会被替换而不是被修改，因此会触发过渡。



```
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
```

---

## 响应路由参数的变化

提醒一下，当使用路由参数时，例如从 `/user/foo` 导航到 `/user/bar`，**原来的组件实例会被复用**。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。**不过，这也意味着组件的生命周期钩子不会再被调用**。

复用组件时，想对路由参数的变化作出响应的话，你可以简单地 watch (监测变化) `$route` 对象：

```js
const User = {
  template: '...',
  watch: {
    $route(to, from) {
      // 对路由变化作出响应...
    }
  }
}
```

---

**$route.matched**

- 类型: `Array<RouteRecord>`

一个数组，包含当前路由的所有嵌套路径片段的**路由记录** 。路由记录就是 `routes` 配置数组中的对象副本 (还有在 `children` 数组)。



**`toLocaleLowerCase()`**方法根据任何指定区域语言环境设置的大小写映射，返回调用字符串被转换为小写的格式。

**`trim()`** 方法会从一个字符串的两端删除空白字符。在这个上下文中的空白字符是所有的空白字符 (space, tab, no-break space 等) 以及所有行终止符字符（如 LF，CR 等）。

### [vm.$emit( eventName, […args\] )](https://cn.vuejs.org/v2/api/?#vm-emit)

- **参数**：

  - `{string} eventName`
  - `[...args]`

  触发当前实例上的事件。附加参数都会传给监听器回调。



render 函数
Vue 推荐使用在绝大多数情况下使用 template 来创建你的 HTML。然而在一些场景中，你真的需要 JavaScript 的完全编程的能力，这就是 render 函数，它比 template 更接近编译器。

```js
render: h => h(App) 是下面内容的缩写：
render: function (createElement) {
  return createElement(App);
}
进一步缩写为(ES6 语法)：
render (createElement) {
  return createElement(App);
}
再进一步缩写为：
render (h){
   return h(App);
}
按照 ES6 箭头函数的写法，就得到了：
render: h => h(App);
```



### [el](https://cn.vuejs.org/v2/api/#el)

- **类型**：`string | Element`

- **限制**：只在用 `new` 创建实例时生效。

- **详细**：

  提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标。可以是 CSS 选择器，也可以是一个 HTMLElement 实例。

  在实例挂载之后，元素可以用 `vm.$el` 访问。

  如果在实例化时存在这个选项，实例将立即进入编译过程，否则，需要显式调用 `vm.$mount()` 手动开启编译。

### main.js

main.js 是 vue 的入口函数，逻辑很简单，就是导入需要使用库，并挂载到 id 为 app 的 DOM 节点

## HTML

```
<script src="https://unpkg.com/vue@3"></script>
<script src="https://unpkg.com/vue-router@4"></script>

<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!--使用 router-link 组件进行导航 -->
    <!--通过传递 `to` 来指定链接 -->
    <!--`<router-link>` 将呈现一个带有正确 `href` 属性的 `<a>` 标签-->
    <router-link to="/">Go to Home</router-link>
    <router-link to="/about">Go to About</router-link>
  </p>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</div>
```

### `router-link`[#](https://next.router.vuejs.org/zh/guide/#router-link)

请注意，我们没有使用常规的 `a` 标签，而是使用一个自定义组件 `router-link` 来创建链接。这使得 Vue Router 可以在不重新加载页面的情况下更改 URL，处理 URL 的生成以及编码。我们将在后面看到如何从这些功能中获益。

### `router-view`[#](https://next.router.vuejs.org/zh/guide/#router-view)

`router-view` 将显示与 url 对应的组件。你可以把它放在任何地方，以适应你的布局。

export default
es6 语法，起导出模块作用
name 属性，组件的全局唯一 ID

类型：string
限制：只有作为组件选项时起作用。
详细：
允许组件模板递归地调用自身。注意，组件在全局用 Vue.component () 注册时，全局 ID 自动作为组件的 name。
指定 name 选项的另一个好处是便于调试。有名字的组件有更友好的警告信息。另外，当在有 vue-devtools，未命名组件将显示成 ，这很没有语义。通过提供 name 选项，可以获得更有语义信息的组件树。



你可能有很多次想要在一个组件的根元素上直接监听一个原生事件。这时，你可以使用 `v-on` 的 `.native` 修饰符：

```
<base-input v-on:focus.native="onFocus"></base-input>
```



### $nextTick

在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

### $refs

尽管存在 prop 和事件，有的时候你仍可能需要在 JavaScript 里直接访问一个子组件。为了达到这个目的，你可以通过 `ref` 这个 attribute 为子组件赋予一个 ID 引用。例如：

```
<base-input ref="usernameInput"></base-input>
```

现在在你已经定义了这个 `ref` 的组件里，你可以使用：

```
this.$refs.usernameInput
```

来访问这个 `<base-input>` 实例，以便不时之需。比如程序化地从一个父级组件聚焦这个输入框。在刚才那个例子中，该 `<base-input>` 组件也可以使用一个类似的 `ref` 提供对内部这个指定元素的访问，例如：

```
<input ref="input">
```

甚至可以通过其父级组件定义方法：

```
methods: {
  // 用来从父级组件聚焦输入框
  focus: function () {
    this.$refs.input.focus()
  }
}
```

这样就允许父级组件通过下面的代码聚焦 `<base-input>` 里的输入框：

```js
this.$refs.usernameInput.focus()
```



Action 通过 `store.dispatch` 方法触发：

```js
store.dispatch('increment')
```
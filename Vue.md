# Vue

## 数据

- Data：类型：Object|Function；但是组件只接受函数

  ```vue
  var data = { a: 1 }
  
  // 直接创建一个实例
  var vm = new Vue({
    data: data
  })
  vm.a // => 1
  vm.$data === data // => true
  
  // Vue.extend() 中 data 必须是函数
  var Component = Vue.extend({
    data: function () {
      return { a: 1 }
    }
  })
  ```

- props：数组或对象，用于接收来自父组件的数据。

  ```
  // 简单语法
  Vue.component('props-demo-simple', {
    props: ['size', 'myMessage']
  })
  
  // 对象语法，提供验证
  Vue.component('props-demo-advanced', {
    props: {
      // 检测类型
      height: Number,
      // 检测类型 + 其他验证
      age: {
        type: Number,
        default: 0,
        required: true,
        validator: function (value) {
          return value >= 0
        }
      }
    }
  })
  ```

- computed：计算属性，结果会缓存，会加入到Vue中

- methods：方法

## DOM

- el：Vue实例挂载目标，挂载之后可以通过$el访问
- template：字符串模板，被使用后模板会替代挂载元素
- render：字符串模板的替换方案

## 实例对象

- vm.$data：数据对象
- vm.$props：组件接受到的props对象
- vm.$el：实例使用的根DOM元素
- vm.$slots：访问被插槽分发的内容
- vm.refs：引向子组件
- is特性：添加一个字符串模板




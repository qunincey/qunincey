---
title: render 渲染函数
date: 2018-12-15
tags:
 - vue,promise
categories:
 -  vue
---
# render 渲染函数

render函数在vue中被称为渲染函数，在我的理解里是用来动态创建节点的函数，vue最重要的理念在于MVVM，而这个概念的提出就意味着我们尽量少的去使用原生js操作dom元素。但有时候不得不操作dom元素时，我们就可以使用render函数

```html
<script type="text/x-template" id="anchored-heading-template">
  <h1 v-if="level === 1">
    <slot></slot>
  </h1>
  <h2 v-else-if="level === 2">
    <slot></slot>
  </h2>
  <h3 v-else-if="level === 3">
    <slot></slot>
  </h3>
  <h4 v-else-if="level === 4">
    <slot></slot>
  </h4>
  <h5 v-else-if="level === 5">
    <slot></slot>
  </h5>
  <h6 v-else-if="level === 6">
    <slot></slot>
  </h6>
</script>
```
```js
Vue.component('anchored-heading', {
  render: function (createElement) {
    return createElement(
      'h' + this.level,   // 标签名称
      this.$slots.default // 子节点数组
    )
  },
  props: {
    level: {
      type: Number,
      required: true
    }
  }
})
```

上面一段代码就来自于vue官方，目的就是简化特定的代码实现动态创建节点的功能

而 `render: h => h(App)` 这句就是利用render读取App.vue 渲染出dom模型，再挂载到vue实例上
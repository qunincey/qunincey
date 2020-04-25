---
title: promise中this指向问题
date: 2018-12-15
tags:
 - vue,promise
categories:
 -  vue
---
# promise中this指向问题

在使用axios框架时遇到了promise的this指向undefined问题，记录一下解决过程

JavaScript所有的设计都是基于单线程的，也就是所有的操作都是异步请求的，如果一个页面包含多个前后有关联的请求时，我们就需要不断的配置回调函数，也就是常说的回调地狱   

为了避免这个问题，JavaScript引入了一个promise对象，这个对象代表了一个异步操作，而这个操作不做任何的失败或者成功后的处理，例如这样
```js
var ajax = ajaxGet('http://...');
ajax.ifSuccess(success)
    .ifFail(fail);
```
这里的ajax就类似于promise对象，他的后续操作会在将来的某个时间进行，将请求和后续操作分开。这种链式的操作可以将几个请求并行或者串行的方式调用

axios框架就是基于promise技术的，但我们在发请求时，例如这样
```js
this.$axios.get('/api/seller').then(function (response) {
      response = response.data
      if (response.errno === ERROK) {
        console.log(response.data)
        this.seller = response.data
        console.log(this.seller)
      }
      console.log('success')
    }).catch(function (error) {
      console.log(error)
    })
```
then代表请求后的处理，但我们使用匿名函数时，函数中的this会指向windows，而当我们使用箭头函数时,this指向的是上一层对象
```js
this.$axios.get('/api/seller').then((response) => {
      response = response.data
      if (response.errno === ERROK) {
        console.log(response.data)
        this.seller = response.data
        console.log(this.seller)
      }
      console.log('success')
    }).catch(function (error) {
      console.log(error)
    })
```
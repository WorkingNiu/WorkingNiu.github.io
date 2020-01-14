# 前端面试题-Vue篇


个人收集的一些vue面试题。

很多讲的不够深入，如有错误欢迎指正:raised_hands:

<!--more-->
## vue两个核心

数据驱动 组件系统

## router和route

router为VueRouter的实例，相当于一个全局的路由器对象，里面含有很多属性和子对象，例如history对象，经常用的跳转链接就可以用this.$router.push，和router-link跳转一样。。。
route相当于当前正在跳转的路由对象，可以从里面获取name,path,params,query等

## keep-alive

当它包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。在组件切换过程中将状态保留在内存中，防止重复渲染DOM，减少加载时间及性能消耗，提高用户体验性和 transition 相似，keep-alive 是一个抽象组件：它自身不会渲染成一个 DOM 元素，也不会出现在父组件链中。主要在app,vue中配置，通过属性判断是否使用缓存.

使用示例

### app.vue
```vue
<pre>
<!--这里是需要keepalive的-->
<keep-alive>
 <router-view v-if="$route.meta.keepAlive"></router-view>
<keep-alive>
<!-- 这里不会被keepAlive -->
<router-view v-if="!$route.meta.keepAlive"></router-view>
</pre>
```

### index.js

```javascript
{
    path: '',
    name: '',
    component: '',
    meta: {keepAlive: true} // 这个是需要keepalive的
}，
{
    path: '',
    name: '',
    component: ,
    meta: {keepAlive: false} // 这是不会被keepalive的
}
```

## vue-router 导航钩子

vue-router 导航钩子包括全局级钩子、路由级钩子、组件级钩子

### 全局级钩子

```javascript
import router from '../router'
import NProgress from 'nprogress' // Progress 进度条组件
import 'nprogress/nprogress.css'// Progress 进度条样式

/**
 * 路由守卫
 */
router.beforeEach((to, from, next) => {
  /**
   to: (Route路由对象) 即将要进入的目标 路由对象 to对象下面的属性： path params query hash fullPath matched name meta
   from: (Route路由对象) 当前导航正要离开的路由
   next: (Function函数) 一定要调用该方法来 resolve 这个钩子。 调用方法：next(参数或者空) ***必须调用

   next(无参数的时候): 进行管道中的下一个钩子，如果走到最后一个钩子函数，那么 导航的状态就是 confirmed （确认的）
   next('/404') 或者 next({ path: '/404' }): 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。
   */
    NProgress.start()//开始进度条
    /**
     * 巴拉巴拉全局路由前的业务逻辑
     */
    next() //记得next(),不然不走

})

router.afterEach(() => {
    /**
     * 巴拉巴拉全局路由后的业务逻辑
     */
    NProgress.done() // 结束进度条
})
```

### 路由级钩子

```javascript
import Vue from 'vue'
import Router from 'vue-router'
Vue.use(Router)
export default new Router({
  routes: [
    {
      path: '/login',
      component: () => import('@/views/login/login'),
      beforeEnter:(to, from, next)=>{
        next();
      },
      hidden: true
    },
  ]
})
```

### 组件级钩子

```vue
<template>
  <div>
    login页
  </div>
</template>
<script>
    export default {
        data() {return {};},
        mounted() {},
        methods: {},
        beforeRouteEnter(to, from, next) {
            // 在渲染该组件的对应路由被 confirm 前调用
            // 不！能！获取组件实例 `this`
            // 因为当钩子执行前，组件实例还没被创建
        },
        beforeRouteUpdate(to, from, next) {
            // 在当前路由改变，但是该组件被复用时调用
            // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
            // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
            // 可以访问组件实例 `this`
        },
        beforeRouteLeave(to, from, next) {
            // 导航离开该组件的对应路由时调用
            // 可以访问组件实例 `this`
        }
    };
</script>
```

## 单页应用优缺点

### 优点

1速度快，用户体验好

2对服务器压力小

3适用于前后端分离，可以对应多端

### 缺点

1首次记载时间长

2seo支持不好

## 生命周期

### created(创建)

数据data已经初始化完成，方法也已经可以调用，但是DOM为渲染。在这个周期里面如果进行请求是可以改变数据并渲染，由于DOM未挂载，请求过多或者占用时间过长会导致页面线上空白。

### beforeMount(挂载前)

DOM未完成挂载，数据也初始化完成，但是数据的双向绑定还是显示{{}}，这是因为Vue采用了Virtual DOM（虚拟Dom）技术。先占住了一个坑。

### mounted(挂载)

数据和DOM都完成挂载，在上一个周期占位的数据把值给渲染进去。一般请求会放在这个地方，因为这边请求改变数据之后刚好
能渲染。

### beforeUpdate(更新前)

只要是页面数据改变了都会触发，数据更新之前，页面数据还是原来的数据，当你请求赋值一个数据的时候会执行这个周期，如果没有数据改变不执行。

### updated(更新)

只要是页面数据改变了都会触发，数据更新完毕，页面的数据是更新完成的。beforeUpdate和updated要谨慎使用，因为页面更新数据的时候都会触发，在这里操作数据很影响性能和容易死循环。

### beforeDestroy(销毁前)

这个周期是在组件销毁之前执行，在我项目开发中，觉得这个其实有点类似路由钩子beforeRouterLeave,都是在路由离开的时候执行，只不过beforeDestroy无法阻止路由跳转，但是可以做一些路由离开的时候操作，因为这个周期里面还可以使用data和method。比如一个倒计时组件，如果在路由跳转的时候没有清除，这个定时器还是在的，这时候就可以在这个里面清除计时器。

### Destroyed(销毁)

说实在的，我还真的不知道这个周期跟beforeDestroy有什么区别，我在这个周期里面调用data的数据和methods的方法都能调用，所以我会觉得跟beforeDestroy是一样的。
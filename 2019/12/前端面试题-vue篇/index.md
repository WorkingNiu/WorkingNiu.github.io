# 前端面试题-Vue篇


个人收集的一些vue面试题。

很多讲的不够深入，如有错误欢迎指正:raised_hands:

<!--more-->
## 1 vue两个核心

数据驱动 组件系统

## 2 router和route

router为VueRouter的实例，相当于一个全局的路由器对象，里面含有很多属性和子对象，例如history对象，经常用的跳转链接就可以用this.$router.push，和router-link跳转一样。。。
route相当于当前正在跳转的路由对象，可以从里面获取name,path,params,query等

## 3 keep-alive

当它包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。在组件切换过程中将状态保留在内存中，防止重复渲染DOM，减少加载时间及性能消耗，提高用户体验性和 transition 相似，keep-alive 是一个抽象组件：它自身不会渲染成一个 DOM 元素，也不会出现在父组件链中。主要在app,vue中配置，通过属性判断是否使用缓存.

使用示例

app.vue
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

index.js

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

## 4 vue-router 导航钩子

vue-router 导航钩子包括全局级钩子、路由级钩子、组件级钩子

### 4.1 全局级钩子



### 4.2 路由级钩子

### 4.3 组件级钩子


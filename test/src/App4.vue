<template>
  <div id="app">
    <p>当前路径: {{$route}}1111</p>
    <p>当前路由参数: {{$route.params | json}}</p>
    <a v-link="{name:'foo', params: { userId: 123 }}">11111111</a>
    <a v-link="{ path: '/foo' }">Go to Foo</a>
    <router-view></router-view>
  </div>
</template>
<script>
  import Vue from 'vue'
  import VueRouter from 'vue-router'

  window.addEventListener('load', function () {
    Vue.use(VueRouter)
    // 定义组件
    var Foo = Vue.extend({
      template:
        '<div class="foo">' +
          '<h2>This is Foo!</h2>' +
          '<router-view></router-view>' + // <- 嵌套的外链
        '</div>'
    })

    var Bar = Vue.extend({
      template: '<p>This is bar!</p>'
    })

    var Baz = Vue.extend({
      template: '<p>This is Baz!</p>'
    })

    // 路由器需要一个根组件。
    // 出于演示的目的，这里使用一个空的组件，直接使用 HTML 作为应用的模板
    var App = Vue.extend({})

    // 创建一个路由器实例
    // 创建实例时可以传入配置参数进行定制，为保持简单，这里使用默认配置
    var router = new VueRouter()

    // 定义路由规则
    // 每条路由规则应该映射到一个组件。这里的“组件”可以是一个使用 Vue.extend
    // 创建的组件构造函数，也可以是一个组件选项对象。
    // 稍后我们会讲解嵌套路由
    router.map({
      '/foo': {
        name: 'foo',
        component: Foo,
        // 在/foo下设置一个子路由
        subRoutes: {
          '/bar': {
            // 当匹配到/foo/bar时，会在Foo's <router-view>内渲染
            // 一个Bar组件
            component: Bar
          },
          '/baz': {
            // Baz也是一样，不同之处是匹配的路由会是/foo/baz
            component: Baz
          }
        }
      }
    })
    // 现在我们可以启动应用了！
    // 路由器会创建一个 App 实例，并且挂载到选择符 #app 匹配的元素上。
    router.start(App, '#app')
  })
</script>

---

title: 新手使用vue-router传参时注意事项
date: 2019/6/12 22:03:27
tags: vue vue-router 框架

---

# 新手使用vue-router传参时注意事项

vue-router里路由传参推荐用path和query的组合，或者是name和params的组合。其中name+params传参在刷新页面后路由参数会丢失，可以在路由配置时把参数配在url上，path+query的传参刷新页面后不会丢失，但是传参会拼接到url后，有长度的限制。具体用哪一种传参方式取决于业务的需要和实际的应用场景。

<!-- more -->

# 使用name和params组合传参

> ```javascript
> this.$router.push({name: 'details', params: {'id': 233}})	
> ```

## 路由配置

```javascript
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)

export default new Router({
    mode: 'history',
    routes: [
        {
            path: '/details',
            name: 'details',
            component: resolve => require(['../components/details'], resolve)
        }
    ]
})
```

## 获取参数

```javascript
this.$route.params.id // 233
```

![1](C:\Users\Administrator\Desktop\1.png)

## 刷新参数丢失 显示 undefined

```javascript
this.$route.params.id // undefined
```

![图片](https://segmentfault.com/img/bVbtyj2?w=1414&h=1022)

注意：此方法第一次跳转是没有问题的，参数也可以传过去，但是刷新页面后，参数就没了 (ps: 这个地方其实还有一个问题，当你传递的参数是number类型，第一次是没有问题的，获取的时候也是number类型，但是当你刷新页面后，number变成string类型，如果涉及计算的建议先类型转换一下)



第一次是预期结果 // 234

```javascript
console.log(this.$route.params.id + 1)
```

![](https://segmentfault.com/img/bVbtyon?w=1290&h=1016)

刷新页面后直接字符串拼接了 // 2331

![](https://segmentfault.com/img/bVbtyoo?w=1006&h=1050)

## 参数丢失解决方案

```javascript
routes: [
    {
        path: '/details/:id', // 这里配置的要和你传递的参数名保持一致
        name: 'details',
        component: resolve => require(['../components/details'], resolve)
    }
]
```

# path和query组合传参

```javascript
this.$router.push({path: '/details', query: {id: 666}})
this.$route.query.id // 666
```

![](https://segmentfault.com/img/bVbtyqO?w=1150&h=1046)

此方法参数会跟在问号后面 例如：/details?id=666，该方法刷新页面不会丢失参数

最后：根据自己的项目选择合适的传参方式
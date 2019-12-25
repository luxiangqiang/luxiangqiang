---
title: 前端框架之【Vue全家桶】
categories:
- 前端框架
tags:
- 前端框架
---

Vue 全家桶（Vue-router、Vuex ）浅入浅出！

<!--more-->

[TOC]

## Vue

### 一、生命周期

#### 1、created

> 在创建vue对象时，当html渲染之前就触发；但是注意，全局vue.js不强制刷新或者重启时只创建一次，也就是说，created()只会触发一次。 	一般会在下边这个钩子函数中初始化页面的数据。

```
// 做服务器请求什么的
created：{
	
}
```

#### 2、computed

> 是挂载vue实例后的钩子函数，钩子在主页挂载时执行一次，如果没有缓存的话，再次回到主页时，mounted还会执行，从而导致ajax反复获取数据。
>
> DOM 加载完成进行渲染，计算属性，实时响应，根据data中的值实时做出处理，就用 computed。

```
computed:{
	方法（）{
		return ； // 通过方法返回值来使页面的数据进行变换
	}
}
```

#### 3、mounted 

> 当新创建的元素挂载到DOM时，会在页面挂载完毕后会调用。

#### 4、Watch 

> 监听数据变化，做出响应的处理事件

```vue
// 监听器
watch: {
	letter() {
		console.log(this.letter)
	}
}
```

### 5、activated

> 是组件被激活后的钩子函数，钩子则不受缓存的影响，每次重新回到主页都会执行。



### 二、指令

#### 1、v-show

> 用于控制控件的显示。

#### 2、v-model

> 用于数据双向绑定，一般用于 input。

#### 3、v-for

> 用于循环渲染数据。

#### 4、@click

> 用于事件绑定。



### 三、属性

#### 1、ref

> `ref` 被用来给元素或子组件注册引用信息。引用信息将会注册在父组件的 `$refs` 对象上。如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素；如果用在子组件上，引用就指向组件实例：

```
 <div class="list" ref="wrapper">
 </div>
```

```
// 可以获取到 DOM 信息
this.$refs.wrapper 
```



### 三、组件

> 所有的组件存放到文件夹中，然后再 App.vue 进行导入。



#### 1、公共组件

> 公共组件写到一个公共的文件夹中。



## Vue-router

> 路由主要实现一级页面的跳转，二级页面跳转，三级页面跳转。

```
// 安装路由
npm install vue-router --save
```



#### 1、导入/使用路由

> 新建一个路由文件夹，新建 router.js 文件，导入路由，使用路由。

```
import Vue from 'vue'
import Router from 'vue-router'
```

```
Vue.use(Router)
```



#### 2、实例化路由对象

> 新建一个路由对象，然后暴露接口，在 Vue 的对象中去使用。

```
export default new Router({
  mode:'history',
  routes: []
})
```

```
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
```



#### 3、配置路由的跳转路径

> 配置路由跳转对象，有三个参数：

- path：跳转路径。
- name：路由的名字。
- component ：跳转的组件。

```
import Menu from '@/components/Menu'
{path: '/menu',name: 'MenuLink',component: Menu}
```



#### 4、在跳转的组件上设置跳转

> 使用 router-link 标签进行设置跳转。

```vue
// tag：替换的标签   :to : 使用的路由跳转对象（可以是由 path 也可以指定路由对象）
// 方式一：静态绑定路由
<router-link to="/">新闻</router-link>

// 方式二：动态绑定路由
<router-link :to="homeLink">新闻</router-link>
<script>
export default {
    data(){
        return {
            homeLink:'/'
        }
    }
}
</script>

// 方式三：使用路由名字绑定路由
<router-link tag="div" :to="{name:'historyLink'}">新闻</router-link>
```



###### ▉  默认跳转页面

> 如果输入错误的 URL 需要设置默认的跳转页面。

```vue
// 错误路径跳转根目录
{path: '*',redirect:'/'},
```



###### ▉  路由跳转方法

```
// 跳转上次浏览页面
// this.$router.go(-1)

// 指定跳转地址
// this.$router.replace('/menu')

// 指定跳转路由的名字下
// this.$router.replace({name:'MenuLink'})

// 通过push进行跳转(常用)
this.$router.push('/menu')
this.$router.push({name:'MenuLink'})
```



#### 5、在页面使用路由

> 全局的 Vue.app 下使用路由的。

```
<router-view></router-view>
```



#### 6、二级路由和三级路由

> 在路由文件中配置二级路由和三级路由。然后在对应的组件进行绑定路由名，和在二级或三级组件下进行显示跳转页面。

```vue
// 二级路由
{path: '/about',name: 'AboutLink',component: About,children:[
      {},
	  // 三级路由
      {},children:[
        {},
        {},
      ]},
      {},
      {},
    ]},
```



#### 7、进入默认显示页面

> 已进入二三级页面，要默认显示一个页面，而不是空白。

```
redirect:' '// 默认显示的组件路径
```



#### 8、全局守卫

> 全局守卫就是当进入整个路由页面时，要在进入之前做一些处理，比如弹框请先登录。

```
router.beforeEach((to, from, next) => {
    // 判断如果是登录页面或注册页面直接进入,否则提示登录
    if(to.path == "/login" || to.path == "/register"){
      next();
    }else{
      alert("您还没有登录,请先登录！")
      next("/login") // 将用户引导到登录页面中
    }
})
```



#### 9、后置钩子



#### 10、路由独享守卫

> 与全局守卫不同的是只作用于规定的路由页面。

```
 {path: '/admin',name: 'AdminLink',component: Admin,beforeEnter:(to, from, next)=>{
     alert('非登陆状态进制访问！')
     next();
 }},
```



#### 11、组件守卫

> 在组件内使用，进入组件之前和离开组件之后进行做处理。

```javascript
 	// 组件内守卫(进入/离开组件时应该做的事情)
    beforeRouteEnter:(to, from, next) => {
        // 页面的渲染顺序是先渲染组件内守卫，然后渲染data,所以在守卫里边直接拿不到data里的数据，所以要使用next回调函数
        next((vm) =>{
            alert("Hellow "+vm.name)
        })
    },

    // 离开组件时的守卫
    beforeRouteLeave:(to, from, next) => {
        if(confirm("确定要离开本页面吗？") == true){
            next();
        }else{
            next(false)
        }
    }
```



#### 12、组件复用

> 想要在当前页面下复用其他组件，必须在相对应的路由中进行配置，components。

```
// 想在根目录的主页中复用组件，所以在对应的路由中进行配置。
{path: '/',name: 'HomeLink',components:{
      // 默认显示组件
      default:Home,
      // 组件复用
      "history":History,
      "delivery":Delivery,
      "orderingGuilde":OrderingGuilde
    }},
```



#### 13、滚动行为

> 滚动路由直接定位页面要展现的位置。





## Vuex

> Vuex 可以很好的实现组件间的数据共享。

#### 1、本地安装 vuex

```vue
npm install vuex --save
```

#### 2、在 src 下新建 store 文件夹以及文件 store.js

#### 3、在文件中引入 Vuex

```vue
import Vue from 'vue'
import Vuex from 'vuex'
```

#### 4、使用 Vuex

```vue
Vue.use(Vuex)
```

#### 5、暴露出 Vuex 实例

```vue
export const store = new Vuex.Store({
    state:{
        // 设置属性

    },
    getters:{
        // 获取属性的状态

    },
    mutations:{
        // 改变属性的状态

    },
    actions:{
        // 应用 mutations
    }
})
```

#### 6、在 main.js 全局中的 Vue 实例中导入使用

```vue
import {store} from './store/store.js'
new Vue({
  el: '#app',
  router,
  store,
  components: { App },
  template: '<App/>'
})
```

#### 7、必须通过 mutations 改变 state 中的属性

```vue
 mutations:{
        // 改变属性的状态
        setMenuItem(state,data){
            state.menuItems = data
        }
    },
```

#### 8、将数据存入 store（存入状态）

> 通过在组件中调用 mutations  的方法就可以将值传入.

```
this.$store.commit("setMenuItem",data) //全局调用 store 中的 commit 方法
```

#### 9、在 store 取出数据（取出状态）

> 在组件中获取状态通过全局调用 store.state 就可以。如果页面响应数据实时变化，可以将方法写在 computed 中。

###### ▉ 第一种方式：

```
// 这样获取有个缺点，直接将属性暴露在外部了
this.$store.state.menuItems; // 全局获取 store 中的数据
```

###### ▉ 第二种方式：

```
// 通过 getter 获取
this.$store.getters.getMenuItems; // 调用 getters 里边的方法
```



## 模块化分离

> 将 Vuex 中的各功能部分进行分离。分别建立独立文件，然后实现模块化。



#### 1、四种状态进行分离

```
export const 功能
import * as getters from './getters'
```

```
export const store = new Vuex.Store({
    state:{
        // 设置属性
        menuItems:{},  // Menu 菜单 
        menuItems2:{}, // Admin 菜单 
        username:null, // 当前用户名
        isLogin:false  // 当前的登录状态
    },
    getters,
    mutations,
    actions
})
```



#### 2、使用 Moudle 优化分离

```
const state = {
  
}

const getters = {
  
}

const mutations = {
  
}

const actions = {
   
}

export default{
    state,
    getters,
    mutations,
    actions
}
```

```
import users from './Module/users'
export const store = new Vuex.Store({
    modules:{
        menu,
        status,
        users
    }
})
```



## 打包上线



## 其他

#### 1、服务器联调

> 通过 vue-cli 中的 config  配置文件中的index.js 文件设置代理，所有的请求都会通过代理请求相对应的服务器。

```vue
dev: {

    // Paths
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {
      // 代理转发
      '/api':{
        target: 'http://localhost:8080'
      }
    },

```



#### 2、真机测试

> 本地访问实现需要修改配置参数，因为 webpack 的配置不能通过本地 ip 去访问。

```
webpack-dev-server --host 0.0.0.0 --inline --progress --config build/webpack.dev.conf.js
```



**1）出现白屏**

> 手机不支持 promise ，需要安装一个第三方包，当判断手机不支持的时候

```
npm install babel-polyfill --save
```














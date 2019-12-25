---
title: 前端框架之【去哪网APP】
categories:
- 前端框架
tags:
- 前端框架
---

去哪网 APP  项目记录！

<!--more-->

## 去哪网App（Vue+VueRouter+Vuex）

[TOC]

### 一、页面配置

#### 1、禁止用户手动放大缩小页面

```html
<meta name="viewport" content="width=device-width,initial-scale=1.0,minium-scale=1.0,maxinum-scale=1.0,user-scalable=no">
```



#### 2、ViewPort

> viewPort被称为视口，是可以看到 Web 内容的窗口区域，一般会有滚动条便于查看。

###### ▉ meta 标签

> 为了能够为了使响应式的网页能够不触发 viewport ，所以就出现了 meta 标签，能够让开发者更好的控制视口的大小和比例

- `width：`控制视口的宽度（如果为 device-width 代表缩放 100% 时，以 CSS 像素计量的屏幕宽度）；
- `initial-scale：`控制页面最初加载时的缩放等级
- `maximum-scale、minimum-scale及user-scalable：`控制允许用户以怎样的方式放大或缩小页面



#### 3、移动端1px像素问题

> 在 PC 端的网页上 1px 的边框在移动端比较粗，这就是经典的移动端 1px 像素问题。



#### 4、移动端 onClick 事件会延迟 300 毫秒

> 本地依赖安装 fastClick 包。

```
npm install fastclick --save
```

在 main.js 导入：

```
import fastClick from 'fastclick'
// 使用 fastClick(移动端300毫秒延迟问题)
fastClick.attach(document.body)
```



#### 5、单位使用 rem

> 移动端的布局使用 rem 是相对于 html 根元素来算。

```
1 rem = html font-size = 50px
```



#### 6、Flex 布局

- flex 属性：属性用于设置或检索弹性盒模型对象的子元素如何分配空间。



#### 7、引入 iconfont 图标库

> 在阿里巴巴矢量图中下载到本地。
>
> 1）将 css 文件放入 asset 文件中。
>
> 2）将字体文件放到 styles文件的 iconfont 文件夹中。
>
> 3）改变 css 文件中引入字体文件的路径。
>
> 4）main.js 引入css 文件。



#### 8、布局抖动问题

> 当网速慢时，Slow 3G 就会先加载文字，后加载图片，导致文字会被图片挤到下方抖动一下。

解决：设置图片的比例，加载之前就占据位置。

```css
.wrapper{
    height: 0;
    overflow: hidden;
    padding-bottom: 26.6%;// 纵横比(相当于图片的宽度) 
    background-color: #ccc;//灰色背景占据
}
```



#### 9、组件样式问题

> 由于组件内 scoped 的样式只修饰当前组件，要想修饰当前组件引入的其他组件，必须进行样式穿透。

```css
// wrapper 样式下的所有 .swiper-pagination-bullet-active 样式都应用这个颜色
.wrapper >>> .swiper-pagination-bullet-active{
        background: #fff;
    }
```



#### 10、vue 的 for 循环遍历

> vue 的for循环遍历必须有个 key 值来用作唯一标识，。

```javascript
 <swiper-slide v-for="item of swiperList" :key="item.id">
     <img class="swiper-image" v-bind:src="item.imgUrl" alt="">
 </swiper-slide>
```



#### 11、文字溢出省略号

> 注意：如果不出现省略号，加 `min-width:0`。

```
overflow: hidden;
white-space:nowrap;
text-overflow: ellipsis;
```



#### 12、盒模型

> 如果设置了 padding 或 margin/border 布局溢出，设置盒模型为 border-box;



#### 13、BFC

> 触发 BFC 的条件，float 浮动，使得外边框不能撑开，所以要触发一下 BFC。



#### 14、使用 better-scroll 实现字母滑动

```
npm install better-scroll --save
```

```
<div class="list" ref="wrapper"></div>
```

```html
import Bscroll from 'better-scroll'
mounted () {
	this.scroll = new Bscroll(this.$refs.wrapper)
}
```



#### 15、v-for 循环遍历对象

```html
 <div class="area" v-for="(item, key) of allCities" :key="key">
     <div class="title">{{key}}</div>
     <ul class="item-list" v-for="innerItem of item" :key="innerItem.id">
         <li>{{innerItem.city}}</li>
     </ul>
</div>
```



#### 16、计算值之更新一次

> 写到 Updata 生命周期函数中，组件页面刷新后调用一次。



#### 17、函滚动节流



#### 18、背景渐变色

```
background-image: linear-gradient(top, rgba(0, 0, 0, 0),rgba(0,0,0,0.8))
```



#### 19、使用轮播插件（vue-awesome-swiper）

安装：

```
npm install vue-awesome-swiper --save
```

在 main.js 引入：

```
import VueAwesomeSwiper from 'vue-awesome-swiper' 

// 使用轮播图
Vue.use(VueAwesomeSwiper)
```



#### 20、样式绑定

> 样式可以设置变动的值。

```vue
:style="opacityStyle"

data () {
    return {
        opacityStyle: {
            opacity:0
        }
   }
},
```



#### 21、解除全局绑定

> 如果某个事件绑定了某个组件，是没有问题的，如果绑定了整个 window 就需要在页面不显示的时候解除绑定，因为会在其它页面会触发，影响性能。

```vue
mounted () {
	window.addEventListener('scroll', this.handleScroll)
},
// 页面销毁要解除绑定
destroyed () {
	window.removeEventListener('scroll', this.handleScroll)
}
```



#### 22、递归组件

> 在组件内自己调用自己，并且注意爆栈，需要进行判断。

```vue
<template>
    <div>
        <div 
        class="item" 
        v-for="(item, index) of list"
        :key="index">
            <div class="item-title">
                <span class="item-title-icon"></span>
                {{item.title}}
            </div>  
            <div v-if="item.children" class="item-children">
                <DetailList :list="item.children"></DetailList>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'DetailList',
    props: {
        list: Array
    }
}
</script>
```



#### 23、异步组件 

> 为了能够实现只加载当前页面的数据，其它页面当打开时，数据才能够加载，这里用到的是异步组件。
>
> 在路由配置中，设置为不立即加载所有页面的组件，而是当加载页面时，再加载组件获取数据。

```vue
component: () => import('@/pages/Home/Home')
```



### 二、父子组件数据传递

> 父组件往子组件传值。



#### 1、父组件向子组件传值

```html
// 传递值
<HomeHeader :city="city"></HomeHeader>
```

```vue
// 接受父组件的传值,并验证
props: {
	city: String
}

// 带有默认值的
props: {
    imgs: {
        type:Array,
        default () {
        	return []
        }
    }
},
```

注意：会出现一个问题，请求是耗时的，所以在渲染的时候，传递的是空，所以需要 if 判断。一般需要放到计算属性中。

```vue
computed: {
    showSwiper () {
    	return this.list.length
    }
}
```



#### 2、子组件向父组件传值

> 通过自定义函数 $emit('事件名'，传递的参数)

```vue
// 子组件
<template>  
    <div>
        <ul class="Alist">
            <li v-for="(item, key) of alphabet" 
                :key="key" 
                @click="handleLetterClick">
                {{key}}
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'CityAlphabet',
    props: {
        alphabet: Object
    },
    methods: {
        handleLetterClick (e) {
            this.$emit('change',e.target.innerText)
        }
    }
}
</script>
```

```vue
// 父组件: 通过 @事件名来接受值
  <City-alphabet
         :alphabet="alphabet"
         @change="handleLetterChange"
         ></City-alphabet>

 handleLetterChange (letter) {
	this.letter = letter;
 }
```



### 三、兄弟组件之间传值

> 方法一：**先子传父，再父传子**
>



### 四、页面设计

- `pages` 目录：这个目录下主要是配置页面，每个页面单独一个文件夹，如:
  - `city` 页面：s然后整个页面是由多个组件组成的，所有的组件在页面中新建文件夹，所有的组件都要在当前页面根目录下进行整合汇总。每个页面的所有数据请求也都是在这个页面进行的。
    - components:
      - Header
      - List
      - fotter
      - ......



### 五、Vuex

#### 1、定义

> vuex 为状态管理模式，主要集中管理状态。



#### 2、什么情况下使用？

① 如果您不打算开发大型单页应用，使用 Vuex 可能是繁琐冗余的。如果应用够简单，您最好不要使用 Vuex。一个简单的 store 模式简单状态管理起步使用)就足够所需了。

② 需要构建一个中大型单页应用，您很可能会考虑如何更好地在组件外部管理状态，Vuex 将会成为自然而然的选择



#### 3、Vuex 的思想

> State 主要存储数据的状态，并且界面所有的数据都是单向绑定的。
>
> Action: 主要写异步任务改变数据状态的。
>
> Mutations：主要是顺序改变数据状态的。
>
> 整个流程是：在组件直接可以通过 `this.$store.state.属性名`  拿到状态值。然后通过 `this.$store.dispatch('Action 中的名'，参数值)` 传递给 Actions。最后在 Actions 中 `context.commit('mutations中的名',参数值)` 改变 state 状态的值。

![1564711196442](C:\Users\10405\AppData\Roaming\Typora\typora-user-images\1564711196442.png)



#### 4、Vuex 的使用

> 1）在根目录下新建 store 文件夹，然后新建 index.js 文件。
>
> 2）引入 vuex，定义 Vuex.Store。

```
import Vuex from 'vuex'
Vue.use(Vuex)

export default new Vuex.Store({
    state: {
       
    },
    actions: {
       
    },
    mutations: {
       
    }
})
```

> 3）在 main.js 中导入并实例化。

```
import store from './store'
new Vue({
  el: '#app',
  router,
  store,
  components: { App },
  template: '<App/>'
})
```

> 4）组件获取数据。

```
{{this.$store.state.city}} 
```

> 5）传入数据

先将数据传入 actions，然后再将数据通过 mutations 更新数据。

```
// 将数据通过 dispath 传入actions
this.$store.dispatch('changeCity',cityName)

// 然后通过 commit 提交到 mutations 进行数据更新

export default new Vuex.Store({
    state: {
       city: '上海'
    },
    actions: {
        // 参数一：上下文 参数二：传递的参数
        changeCity(context, city) {
            context.commit('changeCity',city)
        }
    },
    mutations: {
        // 参数一：state 对象 参数二：传递的参数
        changeCity(state, city) {
            state.city = city
        }
    }
})
```



### 六、Router 路由

#### 1、页面跳转

> 可以通过以下 进行页面的跳转。

```
this.$router.push('/')
```



### 七、Vie-cli

#### 1、在配置文件设置别名路径

> 打开 build 文件夹里边的 webpack.base.conf.js 文件，找到 resolve下的 alias 设置别名路径。修改完需要重启服务器



## 八、Vue 的后续学习

#### 1、官网文档（router、vuex、服务器端渲染都要过一遍）

#### 2、生态系统中的 vue 插件的使用

#### 3、源码的学习 commit 的追溯
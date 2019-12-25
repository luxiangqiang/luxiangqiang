---
title:  玩转 ES6 之【promise对象】
categories:
- 前端
- ES6
tags:
- 前端
- ES6
---

深入了解 ES6 中的 Promise 的原理 。

<!--more-->



### 一、使用 Promise 的原因

#### 1、原因一

> 由于 JS 的运行时单线程的，所以当执行耗时的任务时，就会造成 UI 渲染的阻塞。当前的解决方法是使用回调函数来解决这个问题，当任务执行完毕会，会调用回调方法。



#### 2、原因二

> 回调函数能解决上述的问题，但是有几个缺点：

- 不能捕捉异常（错误处理困难）——回调函数的代码和开始任务代码不在同一事件循环中；
- 回调地域问题（嵌套回调）；
- 处理并行任务棘手（请求之间互不依赖）；



### 二、简单的 Promise

```javascript
let promise = new Promise(function(resolve, reject) {
    resolve();
});

promise.then((res)=>{
    console.log("回调成功！")
},(err) =>{
    console.log("回调失败！")
});
```

1、通过内置的 Promise 构造函数可以创建一个 Promise 对象，构造函数中传入两个函数参数：resolve，reject。两个参数的作用是，在函数内手动调用 resolve 的时候，就说明回调成功了；调用 reject 说明调用失败。通常在 promise 中进行耗时的异步操作，响应是否成功，我们根据判断就可以调用对应的函数。

2、调用 Promise 对象内置的方法 then，传入两个函数，一个是成功回调的函数，一个失败回调的函数。当再 promise 内部调用 resolve 函数时，之后就会回调 then 方法里的第一个函数。当调用了 reject 方法时，就会调用 then 方法的第二个函数。

3、promise 相当于是一个承诺，当承诺兑现的时候（调用了 resolve 函数），就会调用 then 中的第一个回调函数，在回调函数中做处理。当承诺出现未知的错误或异常的时候（调用了 reject 函数），就会调用 then 方法的第二个回调函数，提示开发者出现错误。



### 三、promise 的原理

> 其实 Promise 对象用作异步任务的一个占位符，代表暂时还没有获得但在未来获得的值。



#### 1、promise 的状态

> promise 共有三种状态，完成状态和拒绝状态状态都是由等待状态转变的。一旦 promise 进入了拒绝或完成状态，它的状态就不能切换了。

- **等待状态（pending）**
- **完成状态（resolve）**
- **拒绝状态（reject）**



#### 2、promise 的拒绝状态

> promise 共有两种拒绝状态：显示拒绝（直接调用 reject）和隐式拒绝（抛出异常）。



### 四、一个 promise 实例

> 客户端请求服务器是最常用的异步任务，下面实现一下。

```javascript
function getJson(url){
    // 返回一个 promise 对象
    return new Promise((resolve,reject)=>{
        const request = new XMLHttpRequest();
        request.open("GET",url);

        // 服务器响应成功
        request.onload = function(){
            //try-catch 考虑到 JSON 可能解析会出现错误
            try{
                // 响应码正确且 JSON 解析正确
                if(this.status == 200){
                    // 调用成功承诺
                    resolve(JSON.parse(this.response))
                }else{
                    // 调用失败承诺
                    reject(this.status + " " +this.statusText);
                }
            }catch(e){
                // 调用失败承诺
                reject(e.message)
            }
        }

        // 服务器响应失败
        request.onerror = function(){
            // 调用失败承诺
            reject(this.status + " " +this.statusText);
        }

        // 发送请求
        request.send();

    })
}

// 调用上述异步请求
getJson("url").then((res)=>{
    console.log("解析的JSON数据为:"+res)
},(res)=>{
    console.log("错误信息："+res)
})
```



### 五、嵌套处理异步任务

> 如果连续进行一连串异步请求，并且各个请求之间相互依赖，使用回调函数解决出现 “回到地狱” 现象，所以我们可以使用 promise 的链式回调。

```javascript
// 链式回调
getJson("url")
    .then(n => {getJson(n[0].url)})
    .then(m => {getJson(m[0].url)})
    .then(w => {getJson(w[0].url)})
    .catch((error => {console.log("异常错误！")}))
```

1、因为 then 方法会返回一个 promise 对象，所以连续调用 then 方法可以进行链式调用 promise 。

2、多个异步任务中可能出现错误，只需要调用一个 catch 方法并向其传入错误处理的回调函数。



### 六、并行处理异步任务

> 上述的链式调用主要处理的是多个异步任务之间存在依赖性的，如果同时执行多个异步任务，就是用 promise 中的 all 方法。

```javascript
 // 并行处理多个异步任务
Promise.all([getJson(url),
             getJson(url),
             getJson(url)]).then(resule => {
                 // 如果三个请求都响应成功
                 if(result[0] == 1 && result[1] == 1 && result[2] == 1){
                     console.log("请求成功！")
                 }
             }).catch(error => {console.log("异常错误！")})
```

1、使用 Promise.all() 方法进行异步请求，将多个请求任务封装数组进行同步请求。

2、返回的结果值会打包成一个数组，可以通过数组的下标获取值对返回的结果进行判断。

3、只有全部请求成功才会进入成功的方法，否则就会调用 catch 抛出异常。

4、与 Promise.race() 方法不同的是，race 方法只要其中一个返回成功，就会调用成功的方法。



### 七、生成器和 Promise 相结合

> 生成器和 promise 的结合可以实现更加优雅的异步代码，为了能够解决同步请求导致 UI 阻塞的问题。



#### 1、结合的过程

> 将一个异步的任务放入到一个生成器中，然后执行生成器函数。每执行到一个异步任务，就创建一个 promise 用于代表异步的执行结果。因为不知道 promise 的承诺什么时候兑现，所以此时的执行权交给生成器，生成器挂起，从而不会导致 UI 阻塞。一旦承诺被兑现，就会立即执行生成器的 next 方法，一直重复执行这个过程。



#### 2、async 函数

> 生成器和 promise 的结合代码非常的糟糕，所以需要一个函数管理所有 promise 的调用，还要管理向生成器的发出的所有请求。所以 js 在关键字中新增加了两个关键字分别是 async 和 await 。



① async 表明当前的函数依赖一个异步返回的值。

② 每个异步请求的位置前都要返回一个 await 关键字，告诉浏览器引擎，请在不阻塞应用执行的情况下在这个位置上等待执行结果。





## Promise 面试题

##### ※ 答✔：promise 是什么？promise 的特点是什么？

> promise 是承诺的意思，在未来会有一个确切的回复，并且该有三种状态：承诺一旦从等待状态变为其他状态就会不更改状态。

1. 等待状态 （pending）
2. 完成状态 （resolved）
3. 拒绝状态 （rejected）



##### ※ 答✔：解决了什么问题？

> promise 解决了函数回调地狱的问题。



##### ※ 答✔：如何使用 promise ?

> 通过构造函数创建一个 promise 对象并立即执行，在未来会有一个确切的回复，一旦承诺从等待状态转换为 resolve 或 reject 就不会再更改，通过 then 执行回调函数处理对应的任务。

Promise 的两种使用方法：

- Promise.all() ：将多个请求任务封装数组进行同步请求。
- Promise.race()：race 方法只要其中一个返回成功，就会调用成功的方法。 



##### ※ 答✔：什么是 Promise 链？

> promise 可以实现链式调用，每次 then 之后返回的都是一个 promise 对象，可以紧接着使用 then 继续处理接下来的任务，这样就实现了链式调用。如果在 then 中使用了 return，那么 return 的值也会被 Promise.resove() 包装。

```javascript
Promise.resolve(1)
  .then(res => {
    console.log(res) // => 1
    return 2 // 包装成 Promise.resolve(2)
  })
  .then(res => {
    console.log(res) // => 2
  })
```



##### ※ 答✔：Promise 构造函数执行和 then 函数执行有什么区别？

> 构造函数内部的代码是立即执行的，而 then 是回调之后执行的。



##### ※ 答✔：promise 的优缺点是什么？

> 优点：
>
> 1、链式调用。
>
> 2、解决回调地狱的问题。
>
> 3、代码实现简洁优雅。
>
> 缺点：
>
> 1、无法取消promise。
>
> 2、promise 错误需要通过回调函数捕获。



##### ※ 答✔：Promise 在事件循环中的执行过程是怎样的？





##### ※ 答✔：能不能手写一个 Promise ?

> promise 的执行顺序：
>
> 1、先执行 MyPromise 构造函数；
>
> 2、注册 then 函数；
>
> 3、此时的 promise 挂起，UI 非堵塞，执行其他的同步代码；
>
> 4、执行回调函数。

```javascript
// 三种状态
const PENDING = "pending";
const RESOLVE = "resolve";
const REJECT = "reject";

// promise 函数
function MyPromise(fn){
    const that = this;           // 回调时用于保存正确的 this 对象
    that.state = PENDING;        // 初始化状态
    that.value = null;           // value 用于保存回调函数(resolve/reject 传递的参数值)
    that.resolvedCallbacks = []; // 用于保存 then 中的回调
    that.rejectedCallbacks = [];

    // resolve 和 reject 函数 
    function resolve(value) {
        if(that.state === PENDING){
            that.state = 'resolve';
            that.value = value;
            that.resolvedCallbacks.map(cb => cb(that.value));
        }
    }

    function reject(value) {
        if (that.state === PENDING) {
            that.state = 'reject'
            that.value = value
            that.rejectedCallbacks.map(cb => cb(that.value))
        }
    }

    // 实现如何执行 Promise 中传入的函数
    try {
        fn(resolve, reject)
    } catch (e) {
        reject(e)
    }
}

// 实现 then 函数
MyPromise.prototype.then = function(onResolved, onRejected) {
    const that = this;
    // 判断两个参数是否为函数类型(如果不是函数，就创建一个函数赋值给对应的参数)
    onResolved = typeof onResolved === 'function' ? onResolved : v => v;
    onRejected = typeof onRejected === 'function' ? onRejected : r => {throw r}

    // 判断当前的状态
    if (that.state === 'pending') {
        that.resolvedCallbacks.push(onResolved)
        that.rejectedCallbacks.push(onRejected)
    }
    if (that.state === 'resolve') {
        onResolved(that.value)
    }
    if (that.state === 'reject') {
        onRejected(that.value)
    }
}

new MyPromise((resolve, reject) => {
    setTimeout(() => {
        resolve(1)
    }, 0)
}).then(value => {
    console.log(value)
})
```



##  async 及 await

> 面试题：async 及 await 的特点，它们的优点和缺点分别是什么？await 原理是什么？

 

#####  ※ 答✔：async 的特点？

> 一个函数前加上 async 关键字，就将该函数返回一个 Promise，async  直接将返回值使用 Promise.resolve() 进行包裹（与 then 处理效果相同）。



#####  ※ 答✔：await 的特点？

> `await` 只能配套 `async` 使用，`await` 内部实现了 `generator`，`await` 就是 `generator` 加上 `Promise` 的语法糖，且内部实现了自动执行 `generator`。

 

#####  ※ 答✔：async  和 await 的优缺点？

**优点：**

> 1、**内置执行器。** Generator 函数的执行必须靠执行器，所以才有了 co 函数库，而 async 函数自带执行器。也就是说，async 函数的执行，与普通函数一模一样，只要一行。
>
> 2、**更好的语义。** async 和 await，比起星号和 yield，语义更清楚了。async 表示函数里有异步操作，await 表示紧跟在后面的表达式需要等待结果。
>
> 3、**更广的适用性。** co 函数库约定，yield 命令后面只能是 Thunk 函数或 Promise 对象，而 async 函数的 await 命令后面，可以跟 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时等同于同步操作）。

**缺点：**

> 因为 `await` 将异步代码改造成了同步代码，如果多个异步代码没有依赖性却使用了 `await` 会导致性能上的降低。
>



##### ※ 答✔：await 原理是什么？

> async 函数的实现，就是将 Generator 函数和自动执行器，包装在一个函数里。

```javascript
function spawn(genF) {
  return new Promise(function(resolve, reject) {
    var gen = genF();
    function step(nextF) {
      try {
        var next = nextF();
      } catch(e) {
        return reject(e); 
      }
      if(next.done) {
        return resolve(next.value);
      } 
      Promise.resolve(next.value).then(function(v) {
        step(function() { return gen.next(v); });      
      }, function(e) {
        step(function() { return gen.throw(e); });
      });
    }
    step(function() { return gen.next(undefined); });
  });
}
```
























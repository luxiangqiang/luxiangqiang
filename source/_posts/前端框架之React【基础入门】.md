---
title: Reach 
categories:
- Reach 
tags:
- Reach 
---

![](/images/react.png)

小鹿带你一起学 React。

<!--more-->



# React 详细教程

##  React 安装

### 首先安装 Node.js 

### 在Node.js 使用 npm 包管理工具来安装React

#### 步骤一：安装全局的 React ，在哪里都可以创建 React 文件

```
npm install -g create-react-app
```

**注意：如果是 max 系统 在命令前增加 sudo 全局。**

#### 步骤二：在你的文件夹中创建项目（脚手架）

```
create-react-app  项目名
```

#### 步骤三：切换到该目录下，可以启动该项目

```
npm start
```



## Rect基础入门

### 文件结构和 JSX 语法

#### React的目录结构

在 `Package.json` 配置文件中的一些依赖包。

-  `"react": "^16.5.2"` 为 React 库
-  `"react-dom": "^16.5.2" `将一些 JSX 语法渲染到 DOM中
-  ` "react-scripts": "1.1.5"` 

#### JSX 语法

> React 使用 JSX 来替代常规的 JavaScript，JSX 是一个看起来很像 XML 的 JavaScript 语法扩展。

优点：

- JSX 执行更快，因为它在编译为 JavaScript 代码后进行了优化。
- 它是类型安全的，在编译过程中就能发现错误。
- 使用 JSX 编写模板更加简单快速。

### 使用 JSX

JSX 看起来类似 HTML ，我们可以看下实例: 

```
ReactDOM.render(
    <h1>Hello, world!</h1>,
    document.getElementById('example')
);
```

添加自定义属性需要使用 **data-** 前缀。 

```
<div>
    <h2>欢迎学习 React</h2>
    <p data-myattribute = "somevalue">这是一个很不错的 JavaScript 库!</p>
</div>
,
document.getElementById('example')
);
```

### React 组件的作用

自定义组件名：**HelloMessage**

方法一：用函数来自定义创建组件

```
function HelloMessage(props) {
    return <h1>Hello World!</h1>;
}

//ES6语法函数定义
//const HelloMessage = () => {
//    return <h1>Hello World!</h1>;
//}
 
const element = <HelloMessage />;
 
ReactDOM.render(
    element,
    document.getElementById('example')
);
```

方法二：使用 ES6 class 来定义一个组件: 

创建一个模块 Person

```
import React from 'react';
import './Person.css';

const person = ( ) => {
    return (
        <div className="Person">
            <h1>你好，我是,我已经拥有本书</h1>
        </div>
    )
}

export default person;//暴露接口
```

其他文件调用

```
import Person from './Person/Person';//导入

class App extends Component {
    render() {
		<div>
			 <Person />
		</div>
    }
}
export default App;
```

调用自定义组件

```
import App from './App';

ReactDOM.render(
    <App />,
    document.getElementById('root')
);

```

**注意：**原生 **HTML** 元素名**以小写字母开**头，而自定义的 **React** 类名以**大写字母开**头，比如 **HelloMessage** 不能写成 **helloMessage**。除此之外还需要注意组件类只能包含一个顶层标签，否则也会报错。 

### 组件 - 模块间通信

在创建组件的 render（）函数中，调用模块函数的同时加入传值

```
class App extends Component {
    render() {
		<div>
			 <Person  name="张三"/>
			 <Person>我是闭合标签，可以通过props.children获取</Person>
		</div>
    }
}
```

```
import React from 'react';
import './Person.css';

const person = (props) => {
    return (
        <div className="Person">
            <h1>你好，我是{props.name},我已经拥有本书</h1>
            <p>{props.children}</p>
        </div>
    )
}

export default person;//暴露接口
```

### State 状态

```
class App extends Component {
state = {
  persons:[
    {name:"逯相强",count:1},
    {name:"Heimal",count:2},
    {name:"Aim",count:3},
  ],
  anything:"张三",
  showPersons:false
}

//定义一个函数
switchNmmeHandle = (newName) =>{
  // console.log("你好");
  this.setState({
    persons:[
      {name:newName,count:1},
      {name:"Heimal",count:222},
      {name:"Aim",count:3},
    ]
  })	
}
    render() {
    <button onClick={() => this.switchNmmeHandle("米修")}>更改</button>
		<div>
			<Person name={this.state.persons[0].name} />
		</div>
    }
}
export default App;
```

#### state 与 props 的区别：

**state ** ：用于改变组件内容状态的值（动态）

**props**：用于组件通信进行传值

### 属性传值

```
 class App extends Component {
 
 
 render() {
		<div>
			<Person 
				myClick={this.switchNmmeHandle.bind(this,"米修missu")} />
		</div>
    }
}
export default App;
```

```
const person = ( props) => {
    return (
        <div className="Person">
            <h1 onClick={props.myClick}>你好，我是{props.name},我已经拥有{props.count}本书</h1>
            <p>{props.children}</p>
          
        </div>
    )
}

export default person;
```

### 双向数据绑定

```
import React from 'react';
import './Person.css';

const person = ( props) => {
    return (
        <div>
            <input type="test" onChange={props.changed} defaultValue={props.name}></input>
        </div>
    )
}

export default person;
```

```
  class App extends Component {
  
 changedNameHandler = (event)=>{
  this.setState({
    persons:[
      {name:event.target.value,count:1},
      {name:"Heimal",count:222},
      {name:"Aim",count:3},
    ]
  })
}
 
 render() {
		<div>
			 <Person 
                  changed={this.changedNameHandler}
                  name={this.state.persons[0].name}
                  count={this.state.persons[0].count} />
		</div>
    }
}
export default App;
 

```

### 组件样式的两种方式

#### 方式一：

> 在 CSS 文件中设置，然后倒入引用

Person.css:

```
.Person{
    width: 60%;
    margin: 16px auto;
    border: 1px solid #eee;
    box-shadow: 0 2px 3px #ccc;
    padding: 16px;
    text-align: center;
}
```

Person.js引用

```
import './Person.css';

//用 className
 <div className="Person">
            <h1 onClick={props.myClick}>你好，我是{props.name},我已经拥有{props.count}本书</h1>
            <p>{props.children}</p>
            <input type="test" onChange={props.changed} defaultValue={props.name}></input>
        </div>
```

#### 方式二：

```
class App extends Component {
state = {
  persons:[
    {name:"逯相强",count:1},
    {name:"Heimal",count:2},
    {name:"Aim",count:3},
  ],
  anything:"张三",
  showPersons:false
}
render(
    return(
    //引用样式
    <button style={style} onClick={this.switchNmmeHandle.bind(this,"missu")}>更改</button> 
    )
)
}
```

### React的 if 分支

> 在React中的if判断使用的三元运算符

使用代码抽离，大大减少代码的冗余度。

```
class App extends Component {
    render (
        let person = null;
        if(this.state.showPersons){
          person = (
            <div>
                <Person 
                  changed={this.changedNameHandler}
                  name={this.state.persons[0].name}
                  count={this.state.persons[0].count} />
                <Person
                  myClick={this.switchNmmeHandle.bind(this,"米修missu")}
                  name={this.state.persons[1].name}
                  count={this.state.persons[1].count} />
                <Person 
                  name={this.state.persons[2].name}
                  count={this.state.persons[2].count} >你们好，欢迎购买书籍</Person>
          </div>
     	 )
      
      return (
      	<div className="App">
      	//调用
      	    {person}
      	</div>
      )
  
 }
```

### React - 使用循环

> 使用ES6语法

```
 <div>
     {
         this.state.persons.map((person,index) =>{
             return <Person 
                 myClick={() => this.delectPersonHandler(index)}
                 key={person.id}
                 name={person.name} 
                 count={person.count}/>
         })
     }
  </div>
```

 React 使用循环输出每个值都要有一个 key 与之对应，要映射到虚拟的 DOM 上，虚拟的DOM和真实的DOM通过key对应起来。

### React 动态改变样式和添加类名

```
 style.backgroundColor = 'red'
```

```
// const classes = ["red","bold"].join(" ");
    const classes = [];

    if(this.state.persons.length <= 2 ){
        classes.push("red");
    }

    if(this.state.persons.length <= 1 ){
      classes.push("bold");
  }

    return (
      <h1>Hello Word!</h1>
      <p className={classes.join(" ")}>Hello React</p>//通过数组来动态改变值
      </div>
    );
```

定义的css样式

```
.red{
    color: red;
}

.bold{
    font-weight: bold;
}
```



# React 的项目目录结构







## React元素渲染

> 元素是构成 React 应用的最小单位，它用于描述屏幕上输出的内容。 

```
const element = <h1>Hello, world!</h1>;
```

与浏览器的 DOM 元素不同，React 当中的元素事实上是普通的对象，React DOM 可以确保 浏览器 DOM 的数据内容与 React 元素保持一致。 

### 将元素渲染到 DOM 中

首先我们在一个 HTML 页面中添加一个 id="example" 的 <div>: 

```
<div id="example"></div>
```

要将React元素渲染到根DOM节点中，我们通过把它们都传递给 ReactDOM.render() 的方法来将其渲染到页面上： 

```
const element = <h1>Hello, world!</h1>;
ReactDOM.render(
element,
document.getElementById('example')
);
```

### 更新元素渲染

React 元素都是不可变的。当元素被创建之后，你是无法改变其内容或属性的。

目前更新界面的唯一办法是创建一个新的元素，然后将它传入 ReactDOM.render() 方法：

**实例：**

```
function tick() {
    const element = (
        <div>
            <h1>Hello, world!</h1>
            <h2>现在是 {new Date().toLocaleTimeString()}.</h2>
        </div>
    );
    ReactDOM.render(
        element,
        document.getElementById('example')
    );
}
setInterval(tick, 1000);
```

以上实例通过 setInterval() 方法，每秒钟调用一次 ReactDOM.render()。 

我们可以将要展示的部分封装起来，以下实例用一个函数来表示： 

**语法：**

```
function Clock(props) {
    return (
       
	);
}

function tick() {
    ReactDOM.render(
       
    );
}
setInterval(tick, 1000);
```

**实例：**

```
function Clock(props) {
    return (
        <div>
            <h1>Hello, world!</h1>
            <h2>现在是 {props.date.toLocaleTimeString()}.</h2>
        </div>
    );
}
function tick() {
    ReactDOM.render(
        <Clock date={new Date()} />,
        document.getElementById('example')
    );
}
setInterval(tick, 1000);
```

除了函数外我们还可以创建一个 React.Component 的 ES6 类（需要使用 this.props 替换 props ）

**语法：**

```
class Clock extends React.Component {
    render() {
        return (
           
        );
	}
}
function tick() {
    ReactDOM.render(
       
    );
}
setInterval(tick, 1000);
```

**实例：**

```
class Clock extends React.Component {
    render() {
        return (
            <div>
                <h1>Hello, world!</h1>
                <h2>现在是 {this.props.date.toLocaleTimeString()}.</h2>
            </div>
        );
	}
}
function tick() {
    ReactDOM.render(
        <Clock date={new Date()} />,
        document.getElementById('example')
    );
}
setInterval(tick, 1000);

```



### 独立文件

你的 React JSX 代码可以放在一个独立文件上，例如我们创建一个 `helloworld_react.js` 文件，代码如下： 

```
<body>
<div id="example"></div>
<script type="text/babel" src="helloworld_react.js"></script>
</body>
```



### JavaScript 表达式

们可以在 JSX 中使用 JavaScript 表达式。表达式写在花括号 **{}** 中。实例如下： 

```
ReactDOM.render(
    <div>
        <h1>{1+1}</h1>
    </div>
,
document.getElementById('example')
);
```

在 JSX 中不能使用 **if else** 语句，但可以使用 **conditional (三元运算)** 表达式来替代。以下实例中如果变量 **i** 等于 **1** 浏览器将输出 **true**, 如果修改 i 的值，则会输出 **false**. 

```
ReactDOM.render(
    <div>
        <h1>{i == 1 ? 'True!' : 'False'}</h1>
    </div>
,
document.getElementById('example')
);	
```

### 样式

> React 推荐使用内联样式。,我们可以使用 **camelCase** 语法来设置内联样式. React 会在指定元素数字后自动添加 **px** 。

```
var myStyle = {
    fontSize: 100,
    color: '#FF0000'
};
ReactDOM.render(
    <h1 style = {myStyle}>菜鸟教程</h1>,
    document.getElementById('example')
);	
```

### 注释

注释需要写在花括号中，实例如下： 

```
ReactDOM.render(
<div>
<h1>哈哈哈</h1>
{/*注释...*/}
</div>,
document.getElementById('example')
);
```

### 数组

```
var arr = [
    <h1>react</h1>,
    <h2>学的不仅是技术，更是梦想！</h2>,
];
ReactDOM.render(
    <div>{arr}</div>,
    document.getElementById('example')
);
```





### 参数传递

函数定义的组件：**props.name**

```
function HelloMessage(props) {
    return <h1>Hello {props.name}!</h1>;
}
 
const element = <HelloMessage name="Runoob"/>;
 
ReactDOM.render(
    element,
    document.getElementById('example')
);
```

ES6创建的组件：**this.props.name**

```
class HelloMessage extends React.Component {
    render() {
        return (
            <h1>Hello, {this.props.name}</h1>
        );
    }
}	
    HelloMessage.defaultProps = {
        name: 'Runoob'
    };	
const element = <HelloMessage/>;
    ReactDOM.render(
        element,
        document.getElementById('example')
    );
```

### 复合组件

```
function Name(props) {
    return <h1>网站名称：{props.name}</h1>;
}
function Url(props) {
    return <h1>网站地址：{props.url}</h1>;
}
function Nickname(props) {
    return <h1>网站小名：{props.nickname}</h1>;
}
function App() {
    return (
    <div>
        <Name name="菜鸟教程" />
        <Url url="http://www.runoob.com" />
        <Nickname nickname="Runoob" />
    </div>
    );
}
 
ReactDOM.render(
     <App />,
    document.getElementById('example')
);
```



## React State(状态)

> React 把组件看成是一个状态机（State Machines）。通过与用户的交互，实现不同状态，然后渲染 UI，让用户界面和数据保持一致。React 里，只需更新组件的 state，然后根据新的 state 重新渲染用户界面（不要操作 DOM）。

以下实例创建一个名称扩展为 React.Component 的 ES6 类，在 render() 方法中使用 this.state 来修改当前的时间。

```
class Clock extends React.Component {
//构造函数
    constructor(props) {
        super(props);
        this.state = {date: new Date()};
    }
    render() {
        return (
            <div>
                <h1>Hello, world!</h1>
                <h2>现在是 {this.state.date.toLocaleTimeString()}.</h2>
            </div>
        );
    }
}
ReactDOM.render(
    <Clock />,
    document.getElementById('example')
);
```

### 将生命周期方法添加到类中

> 在具有许多组件的应用程序中，在销毁时释放组件所占用的资源非常重要。 

① 每当 Clock 组件第一次加载到 DOM 中的时候，我们都想生成定时器，这在 React 中被称为**挂载**。

② 每当 Clock 生成的这个 DOM 被移除的时候，我们也会想要清除定时器，这在 React 中被称为**卸载**。

```
class Clock extends React.Component {
    constructor(props) {
        super(props);
        this.state = {date: new Date()};
    }
    //生成定时器
    componentDidMount() {
        this.timerID = setInterval(
        () => this.tick(),
        1000
        );
    }
    //卸载定时器
    componentWillUnmount() {
        clearInterval(this.timerID);
    }
    //给stata赋值
    tick() {
        this.setState({	
            date: new Date()
        });
    }
}
```

**实例解析：**

① `componentDidMount()`与 `componentWillUnmount()`方法被称作**生命周期钩子。**

② 在组件输出到 `DOM` 后会执行 `componentDidMount()`钩子，我们就可以在这个钩子上设置一个定时器。

③ `this.timerID `为计算器的 `ID`，我们可以在 `componentWillUnmount() `钩子中卸载计算器。

**代码执行顺序：**

1. 当 `<Clock />` 被传递给 `ReactDOM.render()` 时，React 调用 `Clock` 组件的构造函数。 由于 `Clock` 需要显示当前时间，所以使用包含当前时间的对象来初始化 `this.state` 。 我们稍后会更新此状态。
2. React 然后调用 `Clock` 组件的 `render()` 方法。这是 React 了解屏幕上应该显示什么内容，然后 React 更新 DOM 以匹配 `Clock` 的渲染输出。
3. 当 `Clock` 的输出插入到 DOM 中时，React 调用 `componentDidMount()` 生命周期钩子。 在其中，`Clock` 组件要求浏览器设置一个定时器，每秒钟调用一次 `tick()`。
4. 浏览器每秒钟调用 `tick()` 方法。 在其中，`Clock` 组件通过使用包含当前时间的对象调用 `setState()` 来调度UI更新。 通过调用 `setState()`，React 知道状态已经改变，并再次调用 `render()` 方法来确定屏幕上应当显示什么。 这一次，`render()` 方法中的 `this.state.date` 将不同，所以渲染输出将包含更新的时间，并相应地更新 DOM。
5. 一旦 `Clock` 组件被从 DOM 中移除，React 会调用 `componentWillUnmount()` 这个钩子函数，定时器也就会被清除。

### State 和 Props

以下实例演示了如何在应用中组合使用 state 和 props 。我们可以在父组件中设置 state， 并通过在子组件上使用 props 将其传递到子组件上。在 render 函数中, 我们设置 name 和 site 来获取父组件传递过来的数据。 

```
class WebSite extends React.Component {
  constructor() {
      super();
 
      this.state = {
        name: "菜鸟教程",
        site: "https://www.runoob.com"
      }
    }
  render() {
    return (
      <div>
        <Name name={this.state.name} />
        <Link site={this.state.site} />
      </div>
    );
  }
}
 
class Name extends React.Component {
  render() {
    return (
      <h1>{this.props.name}</h1>
    );
  }
}
 
class Link extends React.Component {
  render() {
    return (
      <a href={this.props.site}>
        {this.props.site}
      </a>
    );
  }
}
 
ReactDOM.render(
  <WebSite />,
  document.getElementById('example')
);
```






































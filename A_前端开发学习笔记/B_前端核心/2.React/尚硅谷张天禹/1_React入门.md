## 一、初识React

### 1. 全家桶

> - React：基础
> - React-Router：路由库
> - PubSub：消息管理的库
> - Redux：集中式状态管理的库
> - Ant-Design：UI组件库

### 2. 简介

>- 官网链接:[中文官网](https://react.docschina.org/)
>- 用于动态构建用户界面的JavaScript(只关注视图)
>  - 由Facebook开源

### 3. 特点

> 1. 声明式编程
>
> 2. 组件化编程
>
> 3. React Native编写原生应用 
>
>    React Native (简称RN)是Facebook于2015年4月开源的跨平台移动应用开发框架，是Facebook早先开源的JS框架 React 在原生移动应用平台的衍生产物
>
> 4. 高效 (优秀的Diffing算法)

### 4. 高效的原因

>1. 使用虚拟(virtual)DOM,不总是直接操作页面真实DON
>2. DOM Diffing算法,最小化页面重绘
>3. `注意`：React并不会提高渲染速度,反而可能会增加渲染时间,真正高效的原因是它能有效`减少渲染次数`

## 二、React的基本使用

### 1.创建虚拟DOM的两种方式

- #### 	js创建虚拟DOM(`不推荐`)

==**React**.createElement(component, props, ...children)==

```js
//1.创建虚拟DOM,创建嵌套格式的doms
const VDOM=React.createElement('h1',{id:'title'},React.createElement('span',{},'hello,React'))

//2.渲染虚拟DOM到页面
ReactDOM.render(VDOM,docoment.getElementById('test'))
```

- #### 	jsx创建虚拟DOM

==**ReactDOM**.render==

```jsx
//1.创建虚拟DOM
	const VDOM = (  /* 此处一定不要写引号，因为不是字符串 */
    	<h1 id="title">
			<span>Hello,React</span>
		</h1>
	)
//2.渲染虚拟DOM到页面
	ReactDOM.render(VDOM,document.getElementById('test'))

//3.打印真实DOM与虚拟DOM,这一步不是jsx创建虚拟dom必须，我只是为了方便查阅
	const TDOM = document.getElementById('demo')
    console.log('虚拟DOM',VDOM);
	console.log('真实DOM',TDOM);
```

> 可以看到，上下两种方式，明显`jsx`的写法更符合我们的习惯,当出现多重嵌套时,js创建方法会使我们编程出现很大麻烦
>
> 但是jsx其实也只是帮我们做了一层编译,当我们写完jsx代码后,最终我们的代码也会被编译成js的书写方式

### 2.关于虚拟DOM

>1. 本质时Object类型的对象(一般对象)
>2. 虚拟DOM比较'轻',真实DOM比较'重',因为虚拟DOM是React内部在用,无需真实DOM上那么多的属性(只有React需要的属性)
>3. 虚拟DOM最终会被React转化为真实DOM,呈现在页面上

### 3.渲染虚拟DOM

1.	语法:  ReactDOM.render(virtualDOM, containerDOM)
2.	作用: 将虚拟DOM元素渲染到页面中的真实容器DOM中显示
3.	参数说明
	1)	参数一: 纯js或jsx创建的虚拟dom对象
	2)	参数二: 用来包含虚拟DOM元素的真实dom元素对象(一般是一个div)

## 三、jsx语法规则

>1.	全称:  JavaScript XML
>2.	react定义的一种类似于XML的JS扩展语法: JS + XML本质是React.createElement(component, props, ...children)方法的语法糖
>3.	作用: 用来简化创建虚拟DOM 
>   	1)	写法：var ele = <h1>Hello JSX!</h1>
>   	2)	注意1：它不是字符串, 也不是HTML/XML标签
>   	3)	注意2：它最终产生的就是一个JS对象
>4.	标签名任意: HTML标签或其它标签
>5.	标签属性任意: HTML标签属性或其它

### 1.规则

>1. 遇到 <开头的代码, 以标签的语法解析: html同名标签转换为html同名元素, 其它标签需要特别解析
>2. 遇到以 { 开头的代码，以JS语法解析: 标签中的js表达式必须用{ }包裹
>3. 定义虚拟DOM时,不要写引号
>4. 样式的类名指定不要用class,要用className
>5. 内联样式,要用style={{key:value}}的形式去写
>   -  (`双{}代表对象,单{}代表表达式`)
>5. 只有一个跟标签(整个虚拟DOM在外层有且仅有一个容器包裹)
>6. 标签必须闭合
>7. 标签首字母
>   - 若`小写字母开头`,则将该标签转为html中同名元素,若html中无该标签对应的同名元素,则`报错`
>   - 若`大写字母开头`,ract就去渲染对应组件,若组件没有定义,则`报错`
>

### 2.区分【js语句(代码)】与【js表达式】

> 1. 表达式:一个表达式会产生一个值,可以放在任何一个需要值的地方
>
> 下面这些都是表达式
>
>     1. a
>     2. a+b
>     3. demo(1)
>     4. arr.map()
>     5. function test(){}
>
>   6. 语句:不能放在创建虚拟dom语句中
>
>         - if(){}
>         - for(){}
>         - switch(){}

### 3.babel.js的作用

- 浏览器不能直接解析JSX代码, 需要babel转译为纯JS的代码才能运行
- 只要用了JSX，都要加上type="text/babel", 声明需要babel来处理

## 四、两种组件定义区别、组件与模块理解

### 1.定义组件

#### ①函数式声明组件

> 执行了ReactDOM.render(<MyComponent/>.......之后，发生了什么？
>
> 1.React解析组件标签，找到了MyComponent组件。
>
> 2.发现组件是使用函数定义的，随后调用该函数，将返回的虚拟DOM转为真实DOM，随后呈现在页面中。

#### ②类式组件(下面的实例都是指类组件)

>执行了ReactDOM.render(<MyComponent/>.......之后，发生了什么？
>
>​	1.React解析组件标签，找到了MyComponent组件。
>
>​	2.发现组件是使用类定义的，随后new出来该类的实例，并通过该实例调用到原型上的render方法。
>
>​	3.将render返回的虚拟DOM转为真实DOM，随后呈现在页面中。
>
>组件中的render是放在哪里的？
>
>​	MyComponent的原型对象上，供实例使用。
>
>组件中的render中的this是谁？
>
>​	MyComponent的实例对象 <=> MyComponent组件实例对象。

### 2.模块与模块化

#### ① 模块

>1. 理解:向外提供特定功能的js程序,一般就是一个js文件
>2. 为什么要拆成模块:随着业务逻辑增加,代码越来越多且复杂
>3. 作用:复用js,简化js的编写,提高js运行效率

#### ② 模块化

> 当应用的js都以模块来编写,这个应用就是一个模块化的应用

### 3.组件与组件化

#### ① 组件

>1. 理解:用来实现局部功能效果的代码和资源的集合(html/css/js/img等等)
>
>2. 为什么要用组件:一个界面的功能复杂
>3. 作用:复用编码,简化项目编码,提高运行效率

#### ② 组件化

>当应用是以多组件的方式实现,这个应用就是组件化的应用




















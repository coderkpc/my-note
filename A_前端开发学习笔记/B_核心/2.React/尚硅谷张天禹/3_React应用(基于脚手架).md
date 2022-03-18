## 一、React脚手架

>1. xxx脚手架:用来帮助程序原快速创建一个基于xxx库的模板项目
>
>   a) 包含了所有需要的配置(语法检查,jsx编 译,devServer...)
>
>   b)下载好了所有相关的依赖
>
>   c) 可以直接运行一个简单效果
>
>2. react提供了一个用于创建react项目的脚手架库:create-react-app
>
>3. 项目的整体技术架构为:react+webpack+es6+eslint
>
>4. 使用脚手架开发的项目的特点:模块化,组件化,工程化

## 二、创建项目并启动

>1. 全局安装:`npm i -g create-react-app`
>
>2. 切换到想创建项目的目录,使用命令:`create-react-app hello-react`
>3. 进入项目文件夹
>4. 启动项目:`npm start`

## 三、react脚手架项目结构

> public ---- 静态资源文件夹
>
> ​            favicon.icon ------ 网站页签图标
>
> ​            **index.html --------** **主页面**
>
> ​            logo192.png ------- logo图
>
> ​            logo512.png ------- logo图
>
> ​            manifest.json ----- 应用加壳的配置文件
>
> ​            robots.txt -------- 爬虫协议文件
>
> src ---- 源码文件夹
>
> ​            App.css -------- App组件的样式
>
> ​            **App.js --------- App**组件
>
> ​            App.test.js ---- 用于给App做测试
>
> ​            index.css ------ 样式
>
> ​            **index.js -------** **入口文件**
>
> ​            logo.svg ------- logo图
>
> ​            reportWebVitals.js
>
> ​                    --- 页面性能分析文件(需要web-vitals库的支持)
>
> ​            setupTests.js
>
> ​                    ---- 组件单元测试的文件(需要jest-dom库的支持)

## 四、功能界面的组件化编码流程

>1. 拆分组件: 拆分界面,抽取组件
>
>2. 实现静态组件: 使用组件实现静态页面效果
>
>3. 实现动态组件
>
>​	3.1 动态显示初始化数据
>
>​		3.1.1 数据类型
>
>​		3.1.2 数据名称
>
>​		3.1.2 保存在哪个组件?
>
>​	3.2 交互(从绑定事件监听开始)

## 五、TodoList部分代码

>1、更新时可以利用赋值解构后再传入重复字段会自动覆盖的方式进行更新数据
>
>​	-->if(todoObj.id === id) return `{...todoObj,done}`
>
>2、数组批量删除可以用filter过滤实现

```js
//updateTodo用于更新一个todo对象
updateTodo = (id,done)=>{
		//获取状态中的todos
		const {todos} = this.state
		//匹配处理数据
		const newTodos = todos.map((todoObj)=>{
			if(todoObj.id === id) return {...todoObj,done}
			else return todoObj
		})
		this.setState({todos:newTodos})
	}

	//deleteTodo用于删除一个todo对象
	deleteTodo = (id)=>{
		//获取原来的todos
		const {todos} = this.state
		//删除指定id的todo对象
		const newTodos = todos.filter((todoObj)=>{
			return todoObj.id !== id
		})
		//更新状态
		this.setState({todos:newTodos})
	}

	//checkAllTodo用于全选
	checkAllTodo = (done)=>{
		//获取原来的todos
		const {todos} = this.state
		//加工数据
		const newTodos = todos.map((todoObj)=>{
			return {...todoObj,done}
		})
		//更新状态
		this.setState({todos:newTodos})
}
```

## 六、todoList案例相关知识点

		1.拆分组件、实现静态组件，注意：className、style的写法
		2.动态初始化列表，如何确定将数据放在哪个组件的state中？
					——某个组件使用：放在其自身的state中
					——某些组件使用：放在他们共同的父组件state中（官方称此操作为：状态提升）
		3.关于父子之间通信：
				1.【父组件】给【子组件】传递数据：通过props传递
				2.【子组件】给【父组件】传递数据：通过props传递，要求父提前给子传递一个函数
		4.注意defaultChecked 和 checked的区别，类似的还有：defaultValue 和 value
		5.状态在哪里，操作状态的方法就在哪里

## 

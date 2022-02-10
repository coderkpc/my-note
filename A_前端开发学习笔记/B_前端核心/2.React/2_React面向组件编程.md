## 一、React面向组件编程

>1. 使用React开发者工具调试
>
>   `React Developer Tools`
>
>2. 注意
>
>         a) 组件名必须是首字母大写
>
>         b) 虚拟DOM元素只能有一个根元素
>
>         c) 虚拟DOM元素必须有结束标签 < />
>
>3. 渲染类组件标签的基本流程
>
>         a) React内部会创建组件实例对象
>
>         b) 调用render()得到虚拟DOM,并解析为真实DOM
>
>         c) 插入到指定的页面元素内部

## 二、组件三大属性1:state

### 1. 理解

>1. state是组件对象最重要的属性,值是对象(可以包含多个key:value的组合)
>2. 组件被称为`状态机`,通过更新组件的state来更新对应的页面显示(重新渲染组件)

### 2. 强烈注意

>1. 组件中的render方法中的this为组件实例对象
>
>2. 组件自定义方法中this为undefined,如何解决?
>
>         a) 强制绑定this:通过函数对象的bind()
>
>         b) 箭头函数`推荐`
>
>3. 状态数据,不能直接修改或者更新

### 3. 代码示例

#### ①  正常的用函数对象的bind()

```jsx
//1.创建组件
class Weather extends React.Component{
    //构造器调用几次？ ———— 1次
    constructor(props){
        console.log('constructor');
        super(props)
        //初始化状态
        this.state = {isHot:false,wind:'微风'}
        //解决changeWeather中this指向问题,也可以在调用出直接使用
        this.changeWeather = this.changeWeather.bind(this)
    }
    //render调用几次？ ———— 1+n次 1是初始化的那次 n是状态更新的次数
    render(){
        console.log('render');
        //读取状态
        const {isHot,wind} = this.state
        return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}，{wind}</h1>
    }
    //changeWeather调用几次？ ———— 点几次调几次
    changeWeather(){
        //changeWeather放在哪里？ ———— Weather的原型对象上，供实例使用
        //由于changeWeather是作为onClick的回调，所以不是通过实例调用的，是直接调用
        //类中的方法默认开启了局部的严格模式，所以changeWeather中的this为undefined
        console.log('changeWeather');
        //获取原来的isHot值
        const isHot = this.state.isHot
        //严重注意：状态必须通过setState进行更新,且更新是一种合并，不是替换。
        this.setState({isHot:!isHot})
        console.log(this);
        //严重注意：状态(state)不可直接更改，下面这行就是直接更改！！！
        //this.state.isHot = !isHot //这是错误的写法
    }
}
//2.渲染组件到页面
ReactDOM.render(<Weather/>,document.getElementById('test'))
```

#### ② 简写方式：赋值语句的形式+箭头函数

```jsx
//1.创建组件
class Weather extends React.Component{
    //初始化状态
    state = {isHot:false,wind:'微风'}
    render(){
        const {isHot,wind} = this.state
        return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}，{wind}</h1>
    }
    //自定义方法————要用赋值语句的形式+箭头函数
    changeWeather = ()=>{
        const isHot = this.state.isHot
        this.setState({isHot:!isHot})
    }
}
//2.渲染组件到页面
ReactDOM.render(<Weather/>,document.getElementById('test'))
```

## 三、组件三大属性2:props

### 1. 理解

>1. 每个组件对象都会有props(properties的简写)属性
>2. 组件标签的所有属性都保存在props中

### 2. 作用

>1. 通过标签属性从组件外向组件内传递变化的数据
>2. 注意:组件内部不要修改props数据

### 3. 代码示例:

#### ① 类组件使用props

```jsx
//创建组件
class Person extends React.Component{
    render(){
        // console.log(this);
        const {name,age,sex} = this.props
        return (
            <ul>
                <li>姓名：{name}</li>
                <li>性别：{sex}</li>
                <li>年龄：{age+1}</li>
            </ul>
        )
    }
}
//渲染组件到页面
ReactDOM.render(<Person name="jerry" age={19}  sex="男"/>,document.getElementById('test1'))
ReactDOM.render(<Person name="tom" age={18} sex="女"/>,document.getElementById('test2'))
const p = {name:'老刘',age:18,sex:'女'}
// console.log('@',...p);
// ReactDOM.render(<Person name={p.name} age={p.age} sex={p.sex}/>,document.getElementById('test3'))
//此处使用赋值解构方式,使得代码更简洁
ReactDOM.render(<Person {...p}/>,document.getElementById('test3'))
```

#### ② 函数组件使用props

```jsx
//创建组件
		function Person (props){
			const {name,age,sex} = props
			return (
					<ul>
						<li>姓名：{name}</li>
						<li>性别：{sex}</li>
						<li>年龄：{age}</li>
					</ul>
				)
		}
//此处限制可以换成typrScript
		Person.propTypes = {
			name:PropTypes.string.isRequired, //限制name必传，且为字符串
			sex:PropTypes.string,//限制sex为字符串
			age:PropTypes.number,//限制age为数值
		}

		//指定默认标签属性值
		Person.defaultProps = {
			sex:'男',//sex默认值为男
			age:18 //age默认值为18
		}
		//渲染组件到页面
		ReactDOM.render(<Person name="jerry"/>,document.getElementById('test1'))
	
```

## 四、组件三大属性3:refs

### 1. 理解

> 组件内的标签可以定义ref来标识自己

### 2. 代码示例:

#### ① 字符串形式的ref(`不推荐,将被淘汰`)

```jsx
//展示左侧输入框的数据
	showData = ()=>{
		const {input1} = this.refs
		alert(input1.value)
	}
	//展示右侧输入框的数据
	showData2 = ()=>{
		const {input2} = this.refs
		alert(input2.value)
	}
	render(){
		return(
			<div>
				<input ref="input1" type="text" placeholder="点击按钮提示数据"/>&nbsp;
				<button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
				<input ref="input2" onBlur={this.showData2} type="text" placeholder="失去焦点提示数据"/>
			</div>
		)
	}
}
```

#### ② 回调形式的ref

```jsx
/**下面的this指的是组件实例,我直接this.input1 = c 意思是给实例上的input1赋值,之后直接通过调用打印得到*/
//展示左侧输入框的数据
	showData = ()=>{
		const {input1} = this
		alert(input1.value)
	}
	//展示右侧输入框的数据
	showData2 = ()=>{
		const {input2} = this
		alert(input2.value)
	}
	render(){
		return(
			<div>
				<input ref={c => this.input1 = c } type="text" placeholder="点击按钮提示数据"/>&nbsp;
				<button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
				<input onBlur={this.showData2} ref={c => this.input2 = c } type="text" placeholder="失去焦点提示数据"/>&nbsp;
			</div>
		)
	}
}

```

#### ③ createRef创建ref容器`最推荐的`

```jsx
/*React.createRef调用后可以返回一个容器，该容器可以存储被ref所标识的节点,该容器是“专人专用”的*/
myRef = React.createRef()
myRef2 = React.createRef()
//展示左侧输入框的数据
showData = ()=>{
	alert(this.myRef.current.value);
}
//展示右侧输入框的数据
showData2 = ()=>{
	alert(this.myRef2.current.value);
}
render(){
	return(
		<div>
			<input ref={this.myRef} type="text" placeholder="点击按钮提示数据"/>&nbsp;
			<button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
			<input onBlur={this.showData2} ref={this.myRef2} type="text" placeholder="失去焦点提示数据"/>&nbsp;
		</div>
	)
}
}	
```

## 五、事件处理与收集表单数据

### 1. 事件处理

>1. 通过onXxx属性指定事件处理函数(注意大小写)
>
>         a) React使用的是自定义(合成事件,而不是使用的原生DOM事件) ----为了更好的兼容性
>
>         b) React中的事件是通过事件委托的方式处理的(委托给组件最外层的元素)----为了更高效
>
>2. 通过event.target得到发生事件的DOM元素对象 -----不要过度使用ref

### 2. 表单组件的分类

> 就形式上来说，**`受控组件`就是为某个form表单组件添加`value`属性；`非受控组件`就是没有添加`value`属性的组件**

#### ① 受控组件

```jsx
state = {//初始化状态
	username:'', //用户名
	password:'' //密码
}

//保存用户名到状态中
saveUsername = (event)=>{
	this.setState({username:event.target.value})
}

//保存密码到状态中
savePassword = (event)=>{
	this.setState({password:event.target.value})
}

//表单提交的回调
handleSubmit = (event)=>{
	event.preventDefault() //阻止表单提交
	const {username,password} = this.state
	alert(`你输入的用户名是：${username},你输入的密码是：${password}`)
}

render(){
	return(
		<form onSubmit={this.handleSubmit}>
			用户名：<input onChange={this.saveUsername} type="text" name="username"/>
			密码：<input onChange={this.savePassword} type="password" name="password"/>
			<button>登录</button>
		</form>
	)
}
}
```

#### ② 非受控组件

```jsx
handleSubmit = (event)=>{
		event.preventDefault() //阻止表单提交
		const {username,password} = this
		alert(`你输入的用户名是：${username.value},你输入的密码是：${password.value}`)
	}
	render(){
		return(
			<form onSubmit={this.handleSubmit}>
				用户名：<input ref={c => this.username = c} type="text" name="username"/>
				密码：<input ref={c => this.password = c} type="password" name="password"/>
				<button>登录</button>
			</form>
		)
	}
}
```

## 六、高阶函数与函数柯里化

### 1. 高阶函数:

>如果一个函数符合下面两个规范中的任何一个,那该函数就是高阶函数
>
>1. 若A函数,接受的参数是一个函数,那么A就可以称之为高阶函数
>2. 若A函数,调用的返回值依然是一个函数,那么A就可以称之为高阶函数
>
>常见的高阶函数有:Promise、setTimeout、arr.map()等等 

### 2. 函数的柯里化

>通过函数调用继续返回函数的方式,实现对此接受参数最后统一处理的函数编码形式
>
>```js
>function sum(a){ 
>        return (b)=>{
>             return c=>{ 
>                 return a+b+c
>             } 
>        }
>}
>```

### 3. 不用函数柯里化实现事件的绑定

>直接使用回调函数,因为他本身就是以一个函数为返回值
>
>```jsx
><input onChange={event => this.saveFormData('username',event) } type="text" name="username"/>
>```

## 七、生命周期

>1. 组件从创建到死亡它会经历一些特定的阶段
>2. React组件中包含一系列钩子函数(生命周期回调函数),会在特定的时刻调用
>3. 我们在定义组件时,会在特定的生命周期回调函数中,做特定的工作

### 1. React生命周期(旧)

>各个生命周期钩子调用顺序
>
>1. `初始化阶段`:由ReactDOM.render()触发 --初次渲染
>   1. constructor()
>    2. componentWillMount()
>   3. render()
>    4. componentDidMount() ==>`常用` 组件渲染完毕
>
>   一般在这个钩子中做一些初始化的事情,如:开启定时器,发送网络请求,订阅消息等
>
>2. `更新阶段`:由组件内部的this.setState()或者父组件的render触发
>
>   1. shouldComponentUpdate() 组件/页面是否应该更新
>        - return 布尔值
>   2. componentWillUpdate() 组件将要更新
>   3. render()   ===>`必须使用`的一个
>        - return 虚拟DOM
>   4. componentDidUpdate() 组件更新完毕
>
>3. `卸载组件`:由ReactDOM.unmountComponentAtNode(`卸载节点上的组件`)触发
>     
>     1. componentWillUnmount() ===>`常用` 组件将要卸载
>
>一般在这个钩子中做一些首位的事情,如:关闭定时器,取消订阅等

### 2. React生命周期(新)

>1. `初始化阶段`:由ReactDOM.render()触发 ---初次渲染
>    1. constructor()
>    
>     2. `static` getDerivedStateFromProps(props,state) 
>          
>           - 从Props获得派生状态
>          
>          - return 状态对象 这个状态对象会和原有的进行合并，同名状态替换
>          - 注意：==当组件某个状态是由props派生的时候，不能更改，props也不能更改==
>          
>     3. render()
>    
>          - return 虚拟DOM
>    
>     4. componentDidMount() ====>`常用` 
>2. `更新阶段`:由组件内部的this.setState()或者父组件的render触发
>    1. `static` getDerivedStateFromProps(props,state)  从Props获得派生状态
>         - return 状态对象
>    2. shouldComponentUpdate() 组件应该更新
>         - return 布尔值
>     3. render()
>          - return 虚拟DOM
>     4. getSnapshotBeforeUpdate(prevProps,prevState) 在更新前获得快照
>           - return 一个快照值
>     5. componentDidUpdate(prevProps,prevState,snapshot)
>3. `卸载组件`:由ReactDOM.unmountComponentAtNode()触发
>      
>      1. componentWillUnmount() ===>`常用`
>
>一般在这个钩子中做一些收尾的事情,如:关闭定时器、取消订阅消息

### 3. 重要的钩子

>1. render:初始化渲染或者更新渲染调用
>2. componentDidMount() :开启监听,发送ajax请求
>3. componentWillUnmount(): 做一些收尾工作,如:清理定时器

### 4. 即将废弃的钩子

>1. componentWillMount
>2. componentWillReceiveProps
>3. componentWillUpdate
>
>`ps`:现在使用会出现警告,之后版本可能需要加上`UNSAFE_`前缀才能使用,以后可能会被彻底废弃,不建议使用。推测React团队认为提高使用成本将会间接影响我们,让我们去适应新的钩子,所以加上这个

## 八、react/vue中的key

>经典面试题:
>
>1). react/vue中的key有什么作用？（key的内部原理是什么？）
>
>2). 为什么遍历列表时，key最好不要用index?

### 1. 虚拟DOM中key的作用:

>1. 简单的说:key是虚拟DOM对象的标识,在更新显示时key起着极其重要的作用
>
>2. 详细的说:当状态当中的数据发生变化时,react会根据`新数据`生成`新的虚拟DOM`,随后React进行`新虚拟DOM`与`旧虚拟DOM`的diff比较,比较规则如下:
>
>  3. 旧虚拟DOM中找到了与新虚拟DOM相同的key：
>
>     a)若虚拟DOM中内容没变,直接使用之前的真实DOM
>
>     b)若虚拟DON中的内容变了,则生成新的真实DOM,随后替换掉页面中之前的真实DOM
>
>    4. 旧虚拟DOM中未找到与新虚拟DOM相同的key
>
>       根据数据创建新的真实DOM,随后渲染到页面

### 2. 用index作为key可能会引发的问题:

>1. 若对数据进行:逆序添加,逆序删除等破坏顺序操作:
>
>   会产生没有必要的真实DOM更新 ==>界面效果没问题,但是效率低
>
>2. 如果结构中还包含输入类的DOM:
>
>   会产生错误DOM更新 ===>界面有问题
>
>3. 注意:
>
>   如果不存在对数据的逆序添加、逆序删除等破坏顺序操作,仅用于渲染列表用于展示,使用index作为key是没有问题的

### 3. 开发中如何选择key?

>1. 最好使用每条数据的唯一标识作为key,比如id,手机号,身份证号,学号等
>2. 如果确定只是简单的展示诗句,用index也是可以的

```json
//慢动作回放----使用index索引值作为key
初始数据：
{id:1,name:'小张',age:18},
{id:2,name:'小李',age:19},
初始的虚拟DOM：
<li key=0>小张---18<input type="text"/></li>
<li key=1>小李---19<input type="text"/></li>

更新后的数据：
{id:3,name:'小王',age:20},
{id:1,name:'小张',age:18},
{id:2,name:'小李',age:19},
更新数据后的虚拟DOM：
<li key=0>小王---20<input type="text"/></li>
<li key=1>小张---18<input type="text"/></li>
<li key=2>小李---19<input type="text"/></li>

-----------------------------------------------------------------

//慢动作回放----使用id唯一标识作为key

初始数据：
{id:1,name:'小张',age:18},
{id:2,name:'小李',age:19},
初始的虚拟DOM：
<li key=1>小张---18<input type="text"/></li>
<li key=2>小李---19<input type="text"/></li>

更新后的数据：
{id:3,name:'小王',age:20},
{id:1,name:'小张',age:18},
{id:2,name:'小李',age:19},
更新数据后的虚拟DOM：
<li key=3>小王---20<input type="text"/></li>
<li key=1>小张---18<input type="text"/></li>
<li key=2>小李---19<input type="text"/></li>
```


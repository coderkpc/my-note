[TOC]

## 一、原生过渡动画

直接使用transition标签 定义v-开头的css动画样式 即可

##### 示例：

```html
<style>
    .v-enter,
    .v-leave-to {
        opacity: 0;
    }

    .v-enter-active,
    .v-leave-active {
        transition: all .5s linear;
    }
</style>
<transition>
    <div v-if="flag">显示隐藏</div>
</transition>
```



## 二、自定义过渡动画

使用transition标签的时候加上一个name属性

在定义css样式的时候 使用自定义前缀 即可使不同的trasition标签有不同的动画效果

##### 示例

```html
<style>
    //可以设置不同的进入和离开动画
    //设置持续时间和动画函数
    .pig-enter-active,
    .pig-leave-active {
        transition: all .5s linear;
    }
    //离场动画
    .pig-enter,
    .pig-leave-to {
        opacity: 0;
        transform: translateY(300px);
    }
</style>
<transition name="pig">
    <div v-show="flag">我是一只猪</div>
</transition>
```

```html
<div id="example-1">
  <button @click="show = !show">
    Toggle render
  </button>
  <!--取别名类名-->
  <transition name="hongjilin">
    <p v-if="show">hello</p>
  </transition>
</div>
new Vue({
  el: '#example-1',
  data: {
    show: true
  }
})
```

## 三、组件过渡动画

通过mode属性来指定动画样式

in-out先进后出 out-in先出后进

```html
<transition>
    <component :is="cName"></component>
</transition>
```



## 

## 四、animate动画库的使用

##### 	2.1)导入animate.css文件

```html
<link rel="stylesheet" href="animate.css">
```

##### 	2.2)animate动画使用

>1.enter-active-class="指定进入的时候绑定的动画类名"
>2.leave-active-class="指定离开的时候绑定的动画类名"
>3.duration：设置动画的时间 只传数值就是统一设置 传对象就是分开设置


	 注意:如果元素默认就是显示的,那么第一次不会触发动画,如果想第一次就触发动画.可以再添加一个appear属性	

```html
<transition name="pig" enter-active-class="bounceIn" leave-active-class="bounceOut" :duration={enter:800,leave:300}>
    <div v-show="flag" class="animated" >你是一只猪</div>
</transition>
```

##### 2.3)animate官网

> https://animate.style/

## 五、钩子动画函数

##### 	3.1)代码解析：

```html
   <div>
   <button type="button" @click="flag=!flag">快跑</button>
      <!-- 定义js的钩子函数 -->
<transition
    @before-enter="beforeEnter"
    @enter="enter"
    @after-enter="afterEnter"
>
 <div v-show="show" style="width:100px;height:100px;border-radius: 50px;background-color: red;"></div>
</transition>
        </div>



```

```js
let vm = new Vue({
    el: "#app",
    data: {
        flag: false,
    },
    methods: {
        // el 表示要执行动画的那个DOM元素, 是原生的 js DOM 对象
        beforeEnter(el) {
            // 设置动画开始之前的初始位置
            el.style.transform = "translate(0, 0)"
        },
        enter(el, done) {
            // 为了触发浏览器flush渐进回流队列 刷新动画效果
            el.offsetWidth;
            // 动画完成后的样式
            el.style.transform = "translate(550px, 350px)";
            // 设置动画的过渡效果
            el.style.transition = "all 3s ease";
            // done 其实是 afterEnter() 的引用
            done();
        },
        afterEnter(el) {
            // 动画完成之后调用
            this.flag = !this.flag
        }
    }
})

```

## 五、列表动画

```html
<!DOCTYPE html>
<html lang="en">
<head>
</html>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<title>Document</title>
<script src="./lib/vue-2.4.0.js"></script>

<style>
  /* 列表项样式 */
  li{
    border: 1px dashed #999;
    margin:5px;
    line-height: 35px;
    padding-left: 5px;
    font-size: 12px;
    width:100%;
  }
  /* 鼠标列表项悬停效果 */
  li:hover{
    background-color: grey;
    transition: all 0.8s ease;
  }
  /* transition渐变出入场必写基础项 */
  .v-enter,.v-leave-to{
    opacity: 0;
    transform:translateY(80px);
  }
  .v-enter-active,.v-leave-active{
    transition: all 0.6s ease;
  }

 /* transition 在删除列表项的同时，使剩余未删除项可同步移动对原位置填充*/
  .v-move{
    transition:all 0.6s ease;
  }
  .v-leave-active{
    position: absolute;
  }
 
</style>
</head>

<body>
  <div id="app">
    <div>
      <label>
        Id: 
        <input type="text" v-model="id">
      </label>

      <label>
        Name: 
        <input type="text" v-model="name">
      </label>

      <input type="button" value="添加" @click="add">
    </div>

    <!-- 列表项必须用transition-group代替transition包围 -->
    <!-- appear属性实现列表项入场式效果，必须写上否则入场没效果 -->
    <!-- tag属性如果不设置，则默认为span -->
    <transition-group appear tag="ul">
      <li v-for="(item,i) in list" :key="item.id" @click="del(i)">
        {{item.id}}---{{item.name}}
      </li>
    </transition-group>
  </div>

  <script>
    var vm=new Vue({
      el:'#app',
      data:{
        id:'',
        name:'',
        //[]不是{}
        list:[
        {id:1,name:'ess'},
          {id:2,name:'red'},
          {id:3,name:'rdne'},
          {id:4,name:'dnje'}
        ]
      },
      methods:{
        add(){
          this.list.push({id:this.id,name:this.name})
          this.id=this.name=''
        },
        del(i){
          this.list.splice(i,1)
        }
      }
    })
  </script>
</body>
</html>

```

## 六、Vue CLI

### 1. 项目结构：

	├── node_modules 
	├── public
	│   ├── favicon.ico: 页签图标
	│   └── index.html: 主页面
	├── src
	│   ├── assets: 存放静态资源
	│   │   └── logo.png
	│   │── components: 存放组件
	│   │   └── HelloWorld.vue
	│   │── plugins: 存放插件
	│   │   └── HelloWorld.vue
	│   │── utils: 工具类
	│   │   └── request.js
	│   │── api: 所有的网络请求接口统一管理
	│   │   └── index.js
	│   │── store: 状态管理
	│   │   └── HelloWorld.vue
	│   │── router: 路由管理
	│   │   └── HelloWorld.vue
	│   │── App.vue: 汇总所有组件  //管理所有组件的
	│   │── main.js: 入口文件  //是用来引入App.vue和vue创建vm的
	├── .gitignore: git版本管制忽略的配置
	├── .eslintrc.js: eslint的配置文件
	├── babel.config.js: babel的配置文件
	├── package.json: 应用包配置文件 
	├── README.md: 应用描述文件
	├── package-lock.json：包版本控制文件

### 2. 关于不同版本的Vue文件

- vue.js与vue.runtime.xxx.js的区别：
  - vue.js是完整版的Vue，包含：核心功能 + 模板解析器。
  - vue.runtime.xxx.js是运行版的Vue，只包含：核心功能；没有模板解析器。
- 因为vue.runtime.xxx.js没有模板解析器，所以不能使用template这个配置项，需要使用render函数接收到的createElement函数去指定具体内容。

### 3. vue.config.js配置文件

- 使用vue inspect > output.js可以查看到Vue脚手架的默认配置。
- 在根目录创建一个vue.config.js可以对脚手架进行个性化定制，详情见：https://cli.vuejs.org/zh
- 采用的是CommonJS规范暴露模块，因为这是webpack的配置，而webpack基于Nodejs，Nodejs基于CommonJS规范
- 修改vue.config.js一定要重新启动服务，配置项要么就不写，要写就写好写清楚

## 七、ref属性和props配置

### 1. [ref属性](https://cn.vuejs.org/v2/api/#ref)

>  `ref`接收一个字符串，被用来给元素或子组件注册引用信息。引用信息将会注册在父组件的 `$refs` 对象上。

- 被用来给元素或子组件注册引用信息（id的替代者）
- 应用在html标签上获取的是真实DOM元素，应用在组件标签上是组件实例对象（vc）
- 使用方式：
    1. 打标识：`<h1 ref="xxx">.....</h1>` 或 `<School ref="xxx"></School>`
    2. 获取：`this.$refs.xxx`

### [2. props配置](https://cn.vuejs.org/v2/api/#props)
- 功能：用于接收来自父组件的数据

- 传递数据：```<Demo name="xxx"/>```

- 接收数据：

    - 第一种方式（只接收）：```props:['name'] ```

    - 第二种方式（限制类型）：```props:{name:String}```

    - 第三种方式（限制类型、限制必要性、指定默认值）：

        ```js
        props:{
        	name:{
        	type:String, //原生构造函数、任何自定义构造函数、或上述内容组成的数组
        	required:true, //必要性
        	default:'老王' //默认值
            validator://自定义验证函数
        	}
        }
        ```
    ```
    备注：props是只读的，Vue底层会监测你对props的修改，如果进行了修改，就会发出警告，若业务需求确实需要修改，那么请复制props的内容到data中一份，然后去修改data中的数据。
    ```


## 八、Vue提供的一些机制和API

### 1. [mixin](https://cn.vuejs.org/v2/guide/mixins.html)

- 功能：可以把多个组件共用的配置提取成一个混入对象

- 使用方式：

    第一步定义混合：

    ```
    {
        data(){....},
        methods:{....}
        ....
    }
    ```

    第二步使用混入：

    ​	全局混入：```Vue.mixin(xxx)```
    ​	局部混入：```mixins:['xxx']	```

### 2. [插件](https://cn.vuejs.org/v2/guide/plugins.html)

- 功能：用于增强Vue

- 本质：包含install方法的一个对象，如果插件是一个函数，它会被作为 install 方法。 install的第一个参数是Vue，第二个以后的参数是使用插件时传递的数据`Vue.use(chajian, 123, 321)`。 

- 定义插件：

    ```js
    对象.install = function (Vue, options) {
        // 1. 添加全局过滤器
        Vue.filter(....)
    
        // 2. 添加全局指令
        Vue.directive(....)
    
        // 3. 配置全局混入(合)
        Vue.mixin(....)
    
        // 4. 添加实例方法
        Vue.prototype.$myMethod = function () {...}
        Vue.prototype.$myProperty = xxxx
    }
    ```

- 使用插件：```Vue.use()``` 需要在调用 `new Vue()`之前完成 

- 使用场景：添加全局方法或属性、添加全局资源(指令、过滤器、过渡等)，通过全局混入添加一些组件选项(路由)，给Vue原型添加方法等等

### 3. [scoped样式](https://cn.vuejs.org/v2/guide/comparison.html#%E7%BB%84%E4%BB%B6%E4%BD%9C%E7%94%A8%E5%9F%9F%E5%86%85%E7%9A%84-CSS)

1. 作用：让样式在局部生效，防止冲突。

2. 写法：```<style scoped>```

3. 注意：App组件中的样式即使加了scoped，假如子组件的根标签上用了同名样式，那么App中的样式也会在子组件上生效

### 4. $nextTick

1. 语法：```this.$nextTick(回调函数)```
2. 作用：在下一次 DOM 更新结束后执行其指定的回调。
3. 什么时候用：当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行。

## 九、 组件化编码流程

1. 拆分静态组件：组件要按照功能点拆分，命名不要与html元素冲突。
2. 实现动态组件：考虑好数据的存放位置，数据是一个组件在用，还是一些组件在用：

   - 一个组件在用：放在组件自身即可。

   -  一些组件在用：放在他们共同的父组件上（<span style="color:red">状态提升</span>）。
3. 实现交互：从绑定事件开始。








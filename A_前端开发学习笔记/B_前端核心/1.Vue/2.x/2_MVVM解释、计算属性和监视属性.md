## 一.MVVM解释

```js
1. M：模型(Model) ：data中的数据
2. V：视图(View) ：模板代码
3. VM：视图模型(ViewModel)、视图和模型的衔接桥梁：Vue实例 vue的实例一般取名为vm
	vm帮助我们完成了根据我们制定的绑定关系做数据或者页面的渲染
观察发现：
	1.data中所有的属性，最后都出现在了vm身上。
	2.vm身上所有的属性 及 Vue原型上所有属性，在Vue模板中都可以直接使用。
    3.MVVM采用双向数据绑定，(data-binding) ： View 的变动。自动反映在 ViewModel, 反之亦然。 Angular 和 Ember 都采用这种模式
```

**我想定义多个vue实例可以吗？**

> 答案：可以的，在h5里头，我们才有机会定义多个vue实例。
> 	spa应用里头是没有机会定义多个vue实例的：
> 取而代之：用具备自己作用域的组件来实现多个vue实例。

### MVC又是什么

>  **MVC(Model-View-Controller), 模块-视图-控制器， 由 MVC 衍生出的 MVP, MVVM。** 
>
>  View 视图层 (常用html、jsp、ejs作为视图层)：传送指令 到 Controller
>
>  Controller控制层：router分发路由、service 业务处理，
>
>  ​	完成业务逻辑后， 要求 Model 改变状态。
>
>  Model数据连接层(连接数据库)：将新的数据发送到 View. 用户得到反馈
>
>  
>
>  MVVM是由MVC演变而来 就前端而言的一种分层模式
>
>  

## 二.el和data的两种写法

### 1.el的2种写法

(1).new Vue时候配置el属性。

(2).先创建Vue实例，随后再通过vm.$mount('#root')指定el的值。

```js
const v = new Vue({
    //el:'#root', //第一种写法
    data:{
        name:'尚硅谷'
    }
})
console.log(v)
v.$mount('#root') //第二种写法 */
```

### 2.data的2种写法

(1).对象式

(2).函数式

​	如何选择：目前哪种写法都可以，以后学习到组件时，data必须使用函数式，否则会报错。

```js
//data的两种写法
new Vue({
    el:'#root',
    //data的第一种写法：对象式
    /* data:{
name:'尚硅谷'
} */

    //data的第二种写法：函数式
    data(){
        console.log('@@@',this) //此处的this是Vue实例对象
        return{
            name:'尚硅谷'
        }
    }
})
```

**一个重要的原则：**

​	**由Vue管理的函数，一定不要写箭头函数，一旦写了箭头函数，this就不再是Vue实例了。**

------

## 三、监视属性watch

### 1.说明

>`watch`属性，可以监听`data`中指定数据的变化，然后可以触发这个`watch`对象中对应的处理函数。
>监听的数据名作为`watch`对象的属性名，指向对应的处理函数

处理函数参数：

- 参数一，newVal，新数据。
- 参数二，oldVal，旧数据

1. 当被监视的属性变化时, 回调函数自动调用, 进行相关操作
2. 监视的属性必须存在，才能进行监视！！
3. 监视的两种写法：
   - new Vue时传入watch配置
   - 通过vm.$watch监视 回调函数不能写成箭头函数 会丢失this

### 2.监听变量

```js
//通过vm.$watch监视
vm.$watch('firstName',{
    immediate:true, //初始化时调用handler
    //当isHot发生改变时,handler自动调用
    handler(newValue,oldValue){
        console.log('firstName被修改了',newValue,oldValue)
        this.fullName = newVal+"-"+this.lastName;
    }
})
//简写 如果不需要其他配置项，只需要监听数据变化并操作的话可以简写
vm.$watch('isHot',function (newValue,oldValue){
    console.log('isHot被修改了',newValue,oldValue,this)
})
----------------------------------------
//new Vue时传入watch配置
watch:{
    isHot:{
        immediate:true, //初始化时让handler调用一下
        handler(newValue,oldValue){
            console.log('isHot被修改了',newValue,oldValue)
        }
    }
    //简写
    //当和watch中的函数名同名的data中的属性发生改变时 自动调用该函数
    firstName(newVal,oldVal){
        console.log('监视到了firstName的变化');
        //设置全名
        this.fullName = newVal+"-"+this.lastName;
    }
}
```



### 3.深度监视

1. Vue中的watch默认不监测对象内部值的改变，只监视引用地址是否发生变化

2. 配置deep:true可以监测对象内部值改变（多层），默认是false。

   ```js
   //用字符串的key可以监视多级结构中某个属性的变化
   'numbers.a':{
       handler(){
           console.log('a被改变了')
       }
   }
   //监视多级结构中所有属性的变化
   numbers:{
       deep:true,
           handler(){
           console.log('numbers改变了')
       }
   }
   ```

3. 备注：

   Vue自身可以监测对象内部值的改变，但Vue提供的watch默认不可以！

   使用watch时根据数据的具体结构，决定是否采用深度监视。

   > 在控制台上 vm.numbers.a = 999 会触发numbers.a的handler函数
   >
   > 也就是说Vue自身能发现 为了提高效率 Vue提供的watch默认不开启深度监视

   

## 四、计算属性computed

就是用已有的属性进行加工计算，生成一个全新的属性

> 1. 定义：要用的属性不存在，要通过已有属性计算得来。必须是Vue的实例上的属性
>
> 2. 原理：底层借助了`Objcet.defineproperty`方法提供的getter和setter。
>
> 3. get函数什么时候执行？
>
>    - 初次读取时会执行一次。
>
>
>    - 当依赖的数据发生改变时会被再次调用。
>
> 4. 优势：与methods实现相比，内部有缓存机制（复用），效率更高，调试方便。
>
> 5. 备注：
>
>    - 计算属性最终会出现在vm上，直接读取使用即可。
>
>    - 如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时依赖的数据发生改变。

```js
computed:{
    //完整写法
    fullName:{
        //get有什么作用？当有人读取fullName时，get就会被调用，且返回值就作为fullName的值
        //get什么时候调用？1.初次读取fullName时。2.所依赖的数据发生变化时。
        get(){
            console.log('get被调用了')
            // console.log(this) //此处的this是vm
            return this.firstName + '-' + this.lastName
        },
            //set什么时候调用? 当fullName被修改时。
            set(value){
                console.log('set',value)
                const arr = value.split('-')
                this.firstName = arr[0]
                this.lastName = arr[1]
            }
    },
        //简写 当只需要读取 不需要设置的时候 可以把fullName直接写成一个函数 读取的时候自动调用
        fullName(){
            console.log('get被调用了')
            return this.firstName + '-' + this.lastName
        }
}
-------------------------------------------
	全名：<span>{{fullName}}</span>		//调用
```

## 五、watch和computed实现列表过滤

用计算属性computed写更简单

用计算属性过滤的同时还可以判断是否需要排序 过滤排序不分家

```js
<li v-for="(p,index) of filPerons" :key="index">
    {{p.name}}-{{p.age}}-{{p.sex}}
</li>
--------------------------------------
new Vue({
    el:'#root',
    data:{
        keyWord:'',
        persons:[
            {id:'001',name:'马冬梅',age:19,sex:'女'},
            {id:'002',name:'周冬雨',age:20,sex:'女'},
            {id:'003',name:'周杰伦',age:21,sex:'男'},
            {id:'004',name:'温兆伦',age:22,sex:'男'}
        ],
        filPersons:[]
    },
    watch:{
        keyWord:{
            immediate:true,
            handler(val){
                this.filPerons = this.persons.filter((p)=>{
                    return p.name.indexOf(val) !== -1
                })
            }
        }
    },
    computed:{
        filPersons(){
            return this.persons.filter((p)=>{
                return p.name.indexOf(this.keyWord) !== -1
            })
        }
    }
}) 
```



## 六、比较computed和methods以及watch里头的方法

| 比较来源 | 使用方法                                                     | 总结                          |
| -------- | ------------------------------------------------------------ | ----------------------------- |
| methods  | 1.@事件名="方法名" 触发事件时调用 要传参就加小括号  \|2.{{方法名()}} 获取调用的返回值插入模板 \|3. | 可以是变量也可以是方法        |
| computed | a(不能绑给事件) a(变量,没有方法) a(变量,直接结果值)          | 定义是方法,使用是变量         |
| watch    | 方法名就是已存在的变量名 方法名不能被当作函数调用            | 方法是主动调用的,非被动调用的 |

## 七、methods、watch、computed的区别

| methods  | 一般用于逻辑处理，网络请求                                   |
| -------- | ------------------------------------------------------------ |
|          | 数据不会缓存，使用的时候需要使用小括号调用                   |
| watch    | 适合监听单个数据的变化。可以监听vue实例上的属性，有一些功能需要每个属性都监听 |
| computed | 计算的数据可以缓存 通过数据代理实现  被访问的时候才去计算    |
|          | 方法体内部引用变量(能监听多个变量),被引用的变量发生改变,会重新出发计算 |

1. computed和watch之间的区别：
   - computed能完成的功能，watch都可以完成。
   - watch能完成的功能，computed不一定能完成，例如：watch可以进行异步操作。
2. 两个重要的小原则：
   - 所被Vue管理的函数，最好写成普通函数，这样this的指向才是vm 或 组件实例对象。
   - 所有不被Vue所管理的函数（定时器的回调函数、ajax的回调函数等、Promise的回调函数），最好写成箭头函数，这样this的指向才是vm 或 组件实例对象。
   - props data computed methods 里头的变量或者方法必须互斥 不同名














[TOC]

## 一、回顾数据代理知识点

Vue2中，主要通过 `Object.defineProperty() `来实现数据代理

何为数据代理？由一个对象代理对另一个对象中属性的操作（读/写）

顾名思义，不直接操作一个对象的数据，对另一个对象操作，由另一个对象去操作目标对象的某个属性  

`Object.defineProperty()的常见配置`

```js
let number = 18
let person = {
    name:'张三',
    sex:'男',
}
//第一个参数是代理人 第二个参数是代理业务  第三个参数是配置对象
Object.defineProperty(person,'age',{
    value:18,
    enumerable:true, //控制属性是否可以枚举，默认值是false
    writable:true, //控制属性是否可以被修改，默认值是false
    configurable:true //控制属性是否可以被删除，默认值是false

    //当有人读取person的age属性时，get函数(getter)就会被调用，且返回值就是age的值
    get(){
        console.log('有人读取age属性了')
        return number
    },

    //当有人修改person的age属性时，set函数(setter)就会被调用，且会收到修改的具体值
    set(value){
        console.log('有人修改了age属性，且值是',value)
        number = value
    }
})
```

---

### Vue2中的数据代理

1. **Vue中的数据代理：**

   通过vm对象来代理data对象中属性的操作（读/写）

2. **Vue中数据代理的好处：**

   更加方便的操作data中的数据

3. **基本原理：**

   通过Object.defineProperty()把data对象中所有属性添加到vm上。

   为每一个添加到vm上的属性，都指定一个getter/setter。

   在getter/setter内部去操作（读/写）data中对应的属性。

 ![img](https://img2020.cnblogs.com/blog/2406559/202108/2406559-20210808200937219-762246666.png) 

## 二、模拟一个数据监测

```js
let obj = {
    name: "陈颖聪",
    age: 10
}
let obj2 = new Observer(obj)
function Observer(obj) {
    // 这个函数是构造函数 new的时候this指向被创建的实例对象
    // 先收集传入对象的每一个key
    let arr = Object.keys(obj)
    // 然后通过数据代理挂载给实例对象
    arr.forEach(key => {
        // 第一个参数是代理业务的对象
        // 第二个参数是代理的属性
        Object.defineProperty(this, key, {
            // 有人获取这个属性的时候自动调用
            get() {
                // 直接返回原对象中对应属性的值
                return obj[key]
            },
            // 有人设置这个属性的时候自动调用
            set(value) {
                console.log(`有人在修改obj的值把${key}的值修改成${value}`);
            }
        })
    })
}
```



## 三、Vue监测数据改变的原理

1. vue会监视data中所有层次的数据。

2. 如何监测对象中的数据？

   ​	通过setter实现监视，且要在new Vue时就传入要监测的数据。

   - 对象中后追加的属性，Vue默认不做响应式处理

   - 如需给后添加的属性做响应式，请使用如下API：

     ​	`Vue.set(target，propertyName/index，value) `
     
     或  `vm.$set(target，propertyName/index，value)`

3. 如何监测数组中的数据？

   ​	通过包裹数组更新元素的方法实现，本质就是做了两件事：

   ​		(1).调用原生对应的方法对数组进行更新。

   ​		(2).重新解析模板，进而更新页面。

4. 在Vue修改数组中的某个元素一定要用如下方法：

   - 使用这些API:push()、pop()、shift()、unshift()、splice()、sort()、reverse()
   - Vue.set() 或 vm.$set()

5. 特别注意：Vue.set() 和 vm.$set() 不能给vm 或 vm的根数据对象 添加属性！！！

## 四、收集表单数据

1. 若：<input type="text"/>，则v-model收集的是value值，用户输入的就是value值。

2. 若：<input type="radio"/>，则v-model收集的是value值，且要给标签配置value值。

3. 若：<input type="checkbox"/>

   - 没有配置input的value属性，那么收集的就是checked（勾选 or 未勾选，是布尔值）

   - 配置input的value属性:

     ​	(1)v-model的初始值是非数组，那么收集的就是checked（勾选 or 未勾选，是布尔值）

     ​	(2)v-model的初始值是数组，那么收集的的就是value组成的数组

4. 备注：v-model的三个修饰符：

   - lazy：失去焦点再收集数据
   - number：输入字符串转为有效的数字
   - trim：输入首尾空格过滤
   
```html
账号：<input type="text" v-model.trim="userInfo.account"> <br/><br/>
密码：<input type="password" v-model="userInfo.password"> <br/><br/>
年龄：<input type="number" v-model.number="userInfo.age"> <br/><br/>
性别：
男<input type="radio" name="sex" v-model="userInfo.sex" value="male">
女<input type="radio" name="sex" v-model="userInfo.sex" value="female"> <br/><br/>
爱好：
学习<input type="checkbox" v-model="userInfo.hobby" value="study">
打游戏<input type="checkbox" v-model="userInfo.hobby" value="game">
吃饭<input type="checkbox" v-model="userInfo.hobby" value="eat">
<br/><br/>
所属校区
<select v-model="userInfo.city">
    <option value="">请选择校区</option>
    <option value="beijing">北京</option>
    <option value="shanghai">上海</option>
    <option value="shenzhen">深圳</option>
    <option value="wuhan">武汉</option>
</select>
<br/><br/>
其他信息：
<textarea v-model.lazy="userInfo.other"></textarea> <br/><br/>
<input type="checkbox" v-model="userInfo.agree">阅读并接受<a href="http://www.atguigu.com">《用户协议》</a>
<button>提交</button>
```

   收集完表单数据，最好把所有数据放在data中的xx对象中，这样可以通过this.xx获取到这个对象，用于发送请求。当然也可以直接放在data中，通过this._data获取全部数据，能用但是不建议这么用，直接访问这个不太好，建议封装成一个对象。






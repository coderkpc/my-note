# typescript 

## 1. 环境配置

### 1.1 安装typescript 

```bash 
  npm i typescript -g 
  tsc -v  
```
### 1.2 编译ts 

#### 1.2.1 命令编译 

```bash 
  tsc index.ts  
```

#### 1.2.2 vscode 编译  

1. 初始化ts配置 

```bash 
  tsc --init 
```

2. 修改tsconfig.json 配置  

```json 
{
  "compilerOptions": {
    "target": "es5", 
    "module": "commonjs", 
    "outDir": "./js"
  }
}
```

3. 点击 **终端** --> **运行生成任务**

   ![](./images/run.png)

## 2. 变量和数据类型

### 2.1 变量 

+ 变量语法  

```js
  // let 变量名: 变量类型 = 值  
  let str: string = 'fly'  

```
+ PS

  在ts中，为变量指定了类型，就只能给该变量设置相同类型的值

```js
  let str: string = 'fly'

  str = 1 // 报错 Type '1' is not assignable to type 'string'.
```

### 2.2 数据类型 

+ 原有类型

  string、number、boolean、Array、null、undefined、Symbol、Object 、function

+ 新增类型 

  tupel元组 enum枚举 any任意类型 never void  

#### 2.2.1 undefined 类型

```js
  let str: undefined = undefined
```
#### 2.2.2 string 类型

```js
  let str: string = 'fly'
```
#### 2.2.3 number 类型

```js
  let str: number = 1 
```
#### 2.2.4 boolean 类型

```js
  // ts中的布尔类型只能是true或者false
  let str: boolean = true
```
#### 2.2.5 Array 类型

+ 使用方式 

```js
  // 方式1: let 数组变量名:数据类型[] = [值1,值2]
  var arr: number[]= [1, 2, 3, 4]

  // 方式2: let 数组变量名:Array<类型> = [值1,值2]
  var arr: Array<number>= [1, 2, 3, 4]
```
+ 特点

  元素类型固定

  长度不限制

#### 2.2.6 tupel(元组) 类型

+ 概念

  一个规定了元素数量和每个元素类型的”数组“，而每个元素的类型可以不相同。

+ 使用方式 

```js
  // 方式1: let 元组名:[类型1,类型2,类型3] = [值1,值2,值3]
  let arr: [string, number, boolean] = ['fly', 18, true]
```
+ 为什么需要元组？

  TS中数组元素类型必须一致，如果需要不同元素，可以使用***元组***。

+ 特点

  声明时，需要制定元素的个数

  声明时，需要为每个元素设置类型


#### 2.2.7 enum(枚举) 类型

+ 问题：性别标识 男（1），女（2），未知（3）

+ 使用方式 

```js
  // 方式1   
    enum Sex {
    Boy=0,
    Girl=1,
    Unknown=2
  }

  // 方式2,枚举值会自动从0开始生成
   enum Sex {
    Boy,
    Girl,
    Unknown
  }

  console.log(Sex)
  // 使用枚举
  let fly: Sex = Sex.Boy
  console.log(fly)

```
#### 2.2.8 any 类型

+ 概念

  any 代表任意类型，一般在获取DOM的时候使用

+ 使用示例 

```js

  let textName: any = document.getElementById('txtName')

```

#### 2.2.9 void 类型

+ 概念

  void 代表没有类型，一般用于无返回类型的函数中

+ 使用示例 

```js

  // 没有返回值
  function show(): void {
    console.log(1)
  }


  // 返回string
  function show(): string {
    return 'str'
  }
  console.log(show())

```

#### 2.2.10 never 类型

+ 概念

  never代表不存在的值的类型，一般用于抛出异常或者无限循环的 函数返回类型

+ 使用示例 

```js

  // 死循环
  function show(): never {
    while (true) {

    }
  }

  // 抛出异常
  function show(): never {
    throw new Error('err')
  }

```
+ PS 

  never类型是ts中的底部类型，所有类型都是never的子类型，所有never类型可以赋值给任意类型

#### 2.2.11 联合类型  

+ 概念

  联合类型（Union Types）可以通过管道(|)将变量设置多种类型，赋值时可以根据设置的类型来赋值。

  注意：只能赋值指定的类型，如果赋值其它类型就会报错。

+ 使用示例 

```js 
  var val:string|number 
  val = 12 
  console.log("数字为 "+ val) 
  val = "Runoob" 
  console.log("字符串为 " + val)
```

### 2.3 类型推论

+ 概念

  ts 会在没有明确的指定类型的时候推测出一个类型，这就是类型推论

+ 示例代码

```js
  let a = 'fly'
  // 推断出a是string类型所有不能再赋值为number类型
  a = 1 // Type '1' is not assignable to type 'string'.
```

## 3. ts 函数

### 3.1 ts函数返回值和参数

#### 3.1.1 返回值 

```js

  function 函数名():返回值类型 {

  }

  let 变量名:变量类型 = 函数名()

```

#### 3.1.2 形参

```js

  function 函数名(形参1:类型,形参2:类型):返回值类型 {

  }

  let 变量名:变量类型 = 函数名(实参1,实参2)

```

+ 小结  
  函数必须定义返回值类型，如果没有返回值类型，则定义返回值类型为void 
  ***实参*** 和 ***形参*** 的 ***类型*** 保持一致  
  ***实参*** 和 ***形参*** 的 ***数量*** 保持一致  

### 3.2 ts可选参数和默认参数及剩余参数

#### 3.2.1 可选参数

```js

  function 函数名(形参?:类型):返回值类型 {

  }

  // 调用可以不传参数  
  函数名()
  // 调用传递参数  
  函数名(实参)
  
```

#### 3.2.2 默认参数

```js

  function 函数名(形参1:类型=默认值1,形参2:类型=默认值2):返回值类型 {

  }

  // 调用可以不传参数  
  函数名()
  // 调用传递1个参数  
  函数名(实参)
  // 调用传递2个参数  
  函数名(实参1,实参2)
  // 只传递第2个实参
  函数名(undefined,实参2)

```


#### 3.2.3 剩余参数

+ 使用语法

```js

  function 函数名(形参1:类型,形参2:类型,...形参2:类型[]):void {
    console.log(a+b)
  }

```

+ 示例代码 

```js 
function show(x: number, y: number, ...rest: number[]): void {
  let result: number = x + y

  for (let ele of rest) {
    result += ele
  }
  console.log(result)
}

show(1, 2)
show(1, 2, 3, 4, 4,)
  
```

+ PS 

    剩余参数 只能 定义有一个
    剩余参数 只能 定义为数组
    剩余参数 只能 定义在形参列表最后

#### 3.2.4 重载

+ 使用语法

```js
// 上边是声明
function add(arg1: string, arg2: string): string
function add(arg1: number, arg2: number): number

// 下边是实现
//function add(arg1: any, arg2: any):any {
// 或者
function add(arg1: string | number, arg2: string | number):any {
  // 在实现上我们要注意严格判断两个参数的类型是否相等
  if (typeof arg1 === 'string' && typeof arg2 === 'string') {
    return arg1 + arg2
  } else if (typeof arg1 === 'number' && typeof arg2 === 'number') {
    return arg1 + arg2
  }
}

console.log(add('fly', 'zs'))
console.log(add(1, 2))

```

## 4. ts class（类）

封装、继承、多态 

### 4.1 基本使用方式  

```js
class Person {
  // 成员变量
  name: string
  age: number
  // 构造函数
  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }
  // 成员方法
  say():void {
    console.log(this.name, this.age)
  }
}


let person = new Person('fly', 18)

person.say()

```


### 4.2 封装

```js
class Person {
  // 成员变量
  name: string
  age: number
  // 构造函数
  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }
  // 封装
  getName(): string {
    return this.name
  }
  setName(name: string): void {
    this.name = name
  }
}

let person = new Person('fly', 18)
console.log(person.getName())

```


### 4.3 继承

```js

class Person {
  // 成员变量
  name: string
  age: number
  // 构造函数
  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }
  // 封装
  getName(): string {
    return this.name
  }
  setName(name: string): void {
    this.name = name
  }
}

// 继承
class ZS extends Person {
  constructor(name: string, age: number) {
    super(name, age)
  }
}

let zs = new ZS('zs', 18)

console.log(zs.getName())

```



### 4.4 多态

#### 4.4.1 基本使用
```js
abstract class Person {
  // 成员变量
  name: string
  age: number
  // 构造函数
  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }
  // 封装
  getName(): string {
    return this.name
  }
  setName(name: string): void {
    this.name = name
  }
  //非抽象方法，无需要求子类实现、重写
  sayHai() {
    console.log('你好，' + this.getName() + '，qf你最胖')
  }
  abstract run(): any
}

// 继承
class ZS extends Person {
  constructor(name: string, age: number) {
    super(name, age)
  }
  //非抽象方法，无需要求子类实现、重写
  sayHai() {
    console.log('你好，' + this.getName() + '，qf你最骚')
  }
  run(): void {
    console.log(this.getName() + '跑起来')
  }
}
let zs = new ZS('zs', 18)
zs.sayHai()
zs.run()

// 继承
class LS extends Person {
  constructor(name: string, age: number) {
    super(name, age)
  }
  //非抽象方法，无需要求子类实现、重写
  sayHai() {
    console.log('你好，' + this.getName() + '，qf你最骚')
  }
  run(): void {
    console.log(this.getName() + '飞起来')
  }
}

let ls = new LS('李四', 18)
ls.sayHai()
ls.run()

```

#### 4.4.2 特点

  抽象类不能够被实例化

  子类中必须定义对应的抽象方法

### 4.5 类修饰符 

#### 4.5.1 常用修饰符 

+ public 公有类型： 在当前类里面、子类里面、外部类都可以访问

+ protected 保护类型：在当前类里面、子类里面可以访问，外部类无法访问

+ private 私有类型：在当前类里面可以访问，子类里面和外部类无法访问

+ ps: 如果不添加修饰符，默认为公有类型

#### 4.5.2 实例代码

```js

class Person {
  // 成员变量
  public name: string
  protected age: number
  private sex: string
  // 构造函数
  constructor(name: string, age: number, sex: string) {
    this.name = name
    this.age = age
    this.sex = sex
  }
  say(): void {
    console.log(this.name)
  }
}

class ZS extends Person {

  constructor(name: string, age: number, sex: string) {
    super(name, age, sex)
  }
  say(): void {
    console.log(this.age + '111')
  }
}

const zs = new ZS('zs', 18, '男')

```
### 4.6 静态属性和静态方法

#### 4.6.1 基本使用

```js

class Person {
  // 成员变量
  public name: string
  protected age: number
  private sex: string
  static staticName = 'zs'
  // 构造函数
  constructor(name: string, age: number, sex: string) {
    this.name = name
    this.age = age
    this.sex = sex
  }
  say(): void {
    console.log(this.name)
  }
  static run(): void {
    console.log(this.staticName)
  }
}
Person.staticName
```

#### 4.6.2 特点  

静态方法中不能使用类型属性，但是可以使用静态属性


## 5. 接口
在面向对象编程中，接口是一种规范的定义，它定义了行为和动作规范
在程序设计里面，接口起到一种限制和规范的作用。接口定义了某一批所需要遵守的规范
接口不关心这些类内部状态数据，也不关心这些类里面方法的实现细节它只规定了这批类里必须提供某些方法，提供这些方法的类就可以满足实际需要。
typescript中的接口类似java，同时还增加了更灵活的接口类型，包括属性、函数、可索引和类等
### 5.1 属性类接口

#### 5.1.1 基本使用
```js
// 使用interface 定义传入的对象必须携带firstname和lastname，以及对应的类型 
interface FullName {
  firstname: string
  lastname: string
}

function printName(obj: FullName) {
  console.log(obj.firstname + obj.lastname)
  //限制可使用的参数
  console.log(obj.firstname + obj.lastname + obj.age)   //  报错 Property 'age' does not exist on type 'FullName'.
}

printName({
  firstname: 'zs',
  lastname: 'ls'
})

// 如果想设置其他的属性需要定义对象接收
// const obj = {
//   age: 18,
//   firstname: 'zs',
//   lastname: 'ls'
// }
// printName(obj)
```
#### 5.1.2 可选属性

1. 基本使用
```js
interface FullName {
  firstname: string
  // 设置可选属性
  lastname?: string
}

function printName(obj: FullName) {
  console.log(obj.firstname)
}

printName({
  firstname: 'ls'
})
```

2. 案例
```js
interface Config {
  type: string
  url: string
  data?: string
  dataType?: 'json'
  success: Function
  // 或者写成
  // success(res: any): void
}

function ajax(config: Config) {
  let xhr = new XMLHttpRequest()
  xhr.open(config.type, config.url)
  xhr.send(config.data)
  xhr.onreadystatechange = function () {
    if (xhr.readyState === 4 && xhr.status === 200) {
      if (config.dataType === 'json') {
        config.success && config.success(JSON.parse(xhr.responseText))
      } else {
        config.success && config.success(JSON.parse(xhr.responseText))
      }
    }
  }
}

ajax({
  type: 'get',
  url: 'http://jsonplaceholder.typicode.com/todos',
  success(res: Object) {
    console.log(res)
  }
})

```
### 5.2 函数类型接口
```js
interface encrypt {
  (key: string, val: string): string
}

const md5: encrypt = (key: string, val: string): string => key + val

console.log(md5('name', 'zs'))
```
### 5.3 可索引接口
#### 5.3.1 特性 
可索引接口,主要是对数组和对象的约束 
#### 5.3.2 示例代码

```js
// 约束数组
interface UserArr {
  [index: number]: string
}

let arr: UserArr = ['fly', 'sky', 'ssr']

console.log(arr[0])

// 约束对象
interface UserObj {
  [index: string]: string
}

const obj: UserObj = { 'name': 'fly' }

console.log(obj.name)
```
### 5.4 类类型接口
```js
interface Person {
  name: string
  age: number
  run(): void
}

class ZS implements Person {
  name: string
  age: number
  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }
  run(): void {
    console.log(this.name + '吃饭第一名')
  }
}

const zs = new ZS('张三', 18)
zs.run()

```
### 5.4 接口扩展

#### 5.4.1 特性 

接口可以继承接口

#### 5.4.1 示例代码   

1. 方式1 
```js
// 定义父接口
interface Person {
  eat(): void
}
// 继承Person接口
interface Father extends Person {
  run(): void
}
// 实现接口
class ZS implements Father {
  name: string
  constructor(name: string) {
    this.name = name
  }
  eat() {
    console.log(this.name + '吃的最多')
  }
  run() {
    console.log(this.name + '跑的最快')
  }
}

const zs = new ZS('张三')

zs.eat()
zs.run()

```
2. 方式2
```js
interface Person {
  eat(): void
}
// 继承接口
interface Father extends Person {
  run(): void
}
// 实现接口
class ZS {
  name: string
  constructor(name: string) {
    this.name = name
  }
  eat() {
    console.log(this.name + '吃的最多')
  }
  run() {
    console.log(this.name + '跑的最快')
  }
}

class LS extends ZS implements Father {
  constructor(name: string) {
    super(name)
  }
}

// const zs = new ZS('张三')

// zs.eat()
// zs.run()
const ls = new LS('李四')
ls.run()

```
### 5.5 泛型

软件工程中，我们不仅要创建一致的定义良好的API，同时也要考虑可重用性。 组件不仅能够支持当前的数据类型，同时也能支持未来的数据类型，这在创建大型系统时为你提供了十分灵活的功能。

#### 5.5.1 泛型变量、泛型函数、泛型类

泛型可以支持不特性的类似，一般使用T表示

泛型具体的类型是调用该方法的时候决定的

1. 泛型变量、泛型函数
```js
function getData<T>(value: T): T {
  return value
}

console.log(getData<number>(2))

```
2. 泛型类
```js
class MinClass<T> {
  public list: T[] = []
  add(value: T): void {
    this.list.push(value)
  }
  min(): T {
    let minNum = this.list[0]
    this.list.forEach(item => {
      if (minNum > item) {
        minNum = item
      }
    })
    return minNum
  }
}

const m1 = new MinClass<number>()

m1.add(11)
m1.add(1)
m1.add(3)

console.log(m1.min())


const m2 = new MinClass<string>()

m2.add('a')
m2.add('b')
m2.add('c')

console.log(m2.min())


```
#### 5.5.2 泛型接口

1. 方式1 
```js
// 方式1 
interface ConfigFn {
  <T>(value: T): T
}

// let getData: ConfigFn = function <T>(value: T): T {
//   return value
// }

let getData: ConfigFn = <T>(value: T): T => value
console.log(getData<string>('fly'))

```

2. 方式2
```js

interface Config<T> {
  (value: T): T
}

// const getData: Config<string> = function <T>(value: T): T {
//   return value
// }

const getData: Config<string> = <T>(value: T): T => value

console.log(getData('fly'))

```

#### 5.5.3  泛型类

1. 类参数类型
```js
class User {
  username!: string
  password!: string
}

class DB {
  add(user: User): boolean {
    console.log(user)
    return true
  }
}

const user = new User()

user.username = 'fly'
user.password = '123456'

const db = new DB()
db.add(user)
```
2. 泛型类参数类型
```js
// 泛型类参数类型
class DB<T>{
  add(info: T): boolean {
    console.log(info)
    return true
  }
}

class User {
  username!: string
  password!: string
}
class Goods {
  title!: string
  ctime!: string
}

var user = new User()
user.username = 'fly'
user.password = '123456'

var goods = new Goods()
goods.title = '可爱多'
goods.ctime = '2020-09-08 14:25:03'

var db1 = new DB<User>()
db1.add(user)
var db2 = new DB<Goods>()
db2.add(goods)

```

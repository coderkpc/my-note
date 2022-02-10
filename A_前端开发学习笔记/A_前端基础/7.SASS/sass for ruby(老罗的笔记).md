### 一、Sass 和 SCSS 有什么区别？

​		SCSS 是 Sass 3 引入新的语法，其语法完全兼容 CSS3，并且继承了 Sass 的强大功能。Sass 和 SCSS 其实是同一种东西，我们平时都称之为 Sass，两者之间不同之处有以下两点：

1. 文件扩展名不同，Sass 是以“.sass”后缀为扩展名，而 SCSS 是以“.scss”后缀为扩展名

2. 语法书写方式不同，Sass 是以严格的缩进式语法规则来书写，不带大括号({})和分号(;)，而 SCSS 的语法书写和我们的 CSS 语法书写方式非常类似。

   

#### SCSS 语法：

 **它还是支持css3,语法和css很近似**

```scss
$font-stark Helvetica, sans-serif;
$primary-color: #333;
body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

#### SASS语法 

```sass
$font-stack: Helvetica, sans-serif  //定义变量
$primary-color: #333 //定义变量
body
  font: 100% $font-stack
  color: $primary-color
```



### 二、安装
```js

    //1.删除原gem源
    gem sources --remove https://rubygems.org/

    //添加国内淘宝源
    gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
    //2.打印是否替换成功
    gem sources -l
    //3.更换成功后打印如下
    *** CURRENT SOURCES ***
    https://ruby.taobao.org/
```

安装 sass  

```
 gem install sass
 gem install compass
```
sass 四种风格

```js
　　* nested：嵌套缩进的css代码，它是默认值。
　　* expanded：没有缩进的、扩展的css代码。
　　* compact：简洁格式的css代码。
　　* compressed：压缩后的css代码。
```

实例代码
```js
    sass --style compressed test.sass test.css
```

### 三、基本用法

#### **3.1 变量**

SASS允许使用变量，所有变量以$开头。

> ```css
> $blue : #1875e7;　
> div {
> 　color : $blue;
> }
> ```
>
> 

如果变量需要镶嵌在字符串之中，就必须需要写在#{}之中。

> ```scss
> $side : left;
> .rounded {
> 　　border-#{$side}-radius: 5px;
> }
> ```
>
> 

#### **3.2 计算功能**

SASS允许在代码中使用算式：

> ```css
> body {
> 　　margin: (14px/2);
> 　　top: 50px + 100px;
> 　　right: $var * 10%;
> }
> ```
>

#### **3.3 嵌套**

SASS允许选择器嵌套。比如，下面的CSS代码：

> ```css
> div h1 {
> 　　color : red;
> }
> ```
>

可以写成：

> ```css
> div {
> 　　hi {
> 　　　　color:red;
> 　　}
> }
> ```
>

属性也可以嵌套，比如border-color属性，可以写成：

> ```css
> p {
> 　　border: {
> 　　　　color: red;
> 　　}
> }
> ```
>

注意，border后面必须加上冒号。

在嵌套的代码块内，可以使用&引用父元素。比如a:hover伪类，可以写成：

> ```css
> a {
> 　　&:hover { color: #ffb3ff; }
> }
> ```
>
> 

#### **3.4 注释**

SASS共有两种注释风格。

标准的CSS注释 /* comment */ ，会保留到编译后的文件。

单行注释 // comment，只保留在SASS源文件中，编译后被省略。

在/*后面加一个感叹号，表示这是"重要注释"。即使是压缩模式编译，也会保留这行注释，通常可以用于声明版权信息。

> ```css
> /*! 
> 　　重要注释！
> */
> ```
>

### **四、代码的重用**

#### **4.1 继承**

SASS允许一个选择器，继承另一个选择器。比如，现有class1：

> ```css
> .class1 {
> 　　border: 1px solid #ddd;
> }
> ```
>

class2要继承class1，就要使用@extend命令：

> ```css
> .class2 {
> 　　@extend .class1;
> 　　font-size:120%;
> }
> ```
>
> 

#### **4.2 Mixin**

Mixin有点像C语言的宏（macro），是可以重用的代码块。

使用@mixin命令，定义一个代码块。

> ```css
> @mixin left {
> 　　float: left;
> 　　margin-left: 10px;
> }
> ```
>

使用@include命令，调用这个mixin。

> ```ss
> div {
> 　　@include left;
> }
> ```
>

mixin的强大之处，在于可以指定参数和缺省值。

> ```css
> @mixin left($value: 10px) {
> 　　float: left;
> 　　margin-right: $value;
> }
> ```
>

使用的时候，根据需要加入参数：

> ```css
> div {
> 　　　@include left(20px);
> }
> ```
>

下面是一个mixin的实例，用来生成浏览器前缀。

> ```css
> @mixin rounded($vert, $horz, $radius: 10px) {
> 　　border-#{$vert}-#{$horz}-radius: $radius;
> 　　-moz-border-radius-#{$vert}#{$horz}: $radius;
> 　　-webkit-border-#{$vert}-#{$horz}-radius: $radius;
> }
> ```
>

使用的时候，可以像下面这样调用：

> ```css
> #navbar li { @include rounded(top, left); }
> 
> #footer { @include rounded(top, left, 5px); }
> ```
>

#### **4.3 颜色函数**

SASS提供了一些内置的颜色函数，以便生成系列颜色。

> ```css
> 　lighten(#cc3, 10%) // #d6d65c
> 　darken(#cc3, 10%) // #a3a329
> 　grayscale(#cc3) // #808080
> 　complement(#cc3) // #33c
> ```
>

#### **4.4 插入文件**

@import命令，用来插入外部文件。

> ```css
> @import "path/filename.scss";
> ```
>

如果插入的是.css文件，则等同于css的import命令。

> ```css
> @import "foo.css";
> ```
>

### **五、高级用法**

#### **5.1 条件语句**

@if可以用来判断：

> ```css
> 　p {
> 　　　　@if 1 + 1 == 2 { border: 1px solid; }
> 　　　　@if 5 < 3 { border: 2px dotted; }
> 　　}
> ```
>

配套的还有@else命令：

> ```css
> 　@if lightness($color) > 30% {
> 　　　　background-color: #000;
> 　} @else {
> 　　　　background-color: #fff;
> 　}
> ```
>

#### **5.2 循环语句**

SASS支持for循环：

> ```scss
> @for $i from 1 to 10 {
> 　　.border-#{$i} {
> 　　　 border: #{$i}px solid blue;
> 　　}
> }
> ```

for循环 10个

>```css
>@for $i from 1 through 10 {
>   	.item-#{$i}{
>        		width: 2em * $i;
>    	}
>}
>```

也支持while循环：

> ```css
> 　$i:6;
> 　@while $i > 0 {
> 　　　.item-#{$i} { width: 2em * $i; }
> 　　　$i: $i - 2;
> 　}
> ```
> 

each命令，作用与for类似：

> ```css
> @each $member in a, b, c, d {
> 　　.#{$member} {
> 　　　　background-image: url("/image/#{$member}.jpg");
> 　　}
> }
> ```
>

#### **5.3 自定义函数**

SASS允许用户编写自己的函数。

> ```css
> @function double($n) {
> 　　@return $n * 2;
> }
> #sidebar {
> 　　width: double(5px);
> }
> ```
> 
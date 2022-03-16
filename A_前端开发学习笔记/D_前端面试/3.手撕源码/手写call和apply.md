# call

```js
Function.prototype.mycall = function (context) {
    // 如果传入的是null 则this指向window
    context = context || window
    // 首先要获取调用call的函数，用this可以获取
    context.fn = this;
    // 生成一个字符串数组
    for (var i = 1, args = []; i < arguments.length; i++) {
        args.push('arguments[' + i + ']')
    }
    var str = args.join(",")
    // 用eval调用js解析引擎去调用函数  将bar的调用返回值存下
    var res = eval(`context.fn(${str})`)
    delete context.fn;
    // 把刚刚存下的返回值当做call调用的返回值return
    return res
}
```

## 调用

```js
// 测试一下
var foo = {
    value: 1
};

function bar (...args) {
    console.log(...args);
    console.log(this.value);
    return {
        name: "abc",
        age: 18
    }
}
console.log(bar.call2(foo, 1, 2, 3, 4));

输出
1 2 3 4
1
{name: 'abc', age: 18}
```

<hr/>

# apply

```js
Function.prototype.myapply = function (context) {
    // 如果传入的是null 则this指向window
    context = context || window
    // 首先要获取调用call的函数，用this可以获取
    context.fn = this;
    // 根据传入的参数数组生成一个字符串
    var str = arguments[1].join(",")
    // 用eval调用js解析引擎去调用函数  将bar的调用返回值存下
    var res = eval(`context.fn(${str})`)
    delete context.fn;
    // 把刚刚存下的返回值当做call调用的返回值return
    return res
}
```

## 调用

```js
// 测试一下
var foo = {
    value: 1
};

function bar (...args) {
    console.log(...args);
    console.log(this.value);
    return {
        name: "abc",
        age: 18
    }
}
console.log(bar.myapply(foo, [1, 2, 3, 4]));

输出
1 2 3 4
1
{name: 'abc', age: 18}
```


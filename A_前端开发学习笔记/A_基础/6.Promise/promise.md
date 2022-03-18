# Promise
## 回调地狱
回调函数嵌套回调函数，过多的嵌套会形成回调地狱，不方便维护和理解 用promise + async + await解决

## promise
promise本身是一个构造函数，它是一个类，需要被实例化<br>
promise 有三种状态
1. pending 等待 默认状态
2. fulfilled 完成
3. rejected  拒绝

#### promise本身是同步的
==三种状态不可逆==
```
var p = new Promise(function(resolved,rejected){
    resolved()
    rejected()
})
```
## promise的原型方法

  * then() 里面有2个函数,第1个函数取resolve的结果,第2个参数取reject的结果
  * * ==then是异步的==
  * catch() 捕获 reject的结果
  * finally() 只要执行resolve或reject后,都会执行finally

如果只会成功 then可以只写一个回调函数
如果会失败 则以下两种写法都行
```
p.then(function (str) {
	console.log(str)
}).catch(function (str) {
	console.log(str)
})

p.then(function (str) {
	console.log(str)
},function (str) {
	console.log(str)
})
```

## promise的静态方法
* Promise.all() 并发执行(同时执行)<br>
后面.原型方法+回调函数<br>
all方法里需要填入一个数组,数组里必须都是支持promise的方法,当所有都被被执行完后才返回执行结果 返回值也是数组

* Promise.race(),race<br> 
方法里需要填入一个数组,谁先执行,就取谁的结果

* Promise.resolve() 只执成功,并返回一个新的promise对象

* Promise.reject()  只执失败,并返回一个新的promise对象


## 回调地狱的解决
```
	function get(url, data = {}) {
		// 返回一个promise 对象
		return new Promise(function (resolved, rejected) {
			// 创建核心对象
			let xhr = createXHR()
			// 初始化 设置请求方式
			xhr.open("get", url + "?" + getParams(data))
			// 发送请求
			xhr.send(null)
			// 处理请求
			xhr.onreadystatechange = function () {
				if (xhr.status >= 200 && xhr.status < 300) {
					// 如果状态码是ok的
					if (xhr.readyState === 4) {
						// 则在接受全部数据后处理数据
						resolved(xhr.response)
					}
				} else {
					rejected()
				}
			}
		})
	}
	function post(url, data = {}) {
		return new Promise(function (resolved, rejected) {
			// 创建核心对象
			let xhr = createXHR()
			// 初始化 设置请求方式和地址
			xhr.open("post", url)
			// 设置请求体数据类型
			xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded")
			// 发送请求
			xhr.send(getParams(data))
			// 处理请求
			xhr.onreadystatechange = function () {
				if (xhr.status >= 200 && xhr.status < 300) {
					if (xhr.readyState === 4) {
						resolved(xhr.response)
					}
				} else {
					rejected()
				}
			}
		})
	}
```
## 配合ES7语法async+await使用
* await关键字后面必须接promise对象,
* 有await关键字的地方,必须是一个async 异步函数
* 它能解决,**把异步像同步一样的被调用**
```
async function getResult() {
	console.log(111)
	let res1 = await axios.get("http://useker.cn/getUsers")
	console.log(222)
	let res2 = await axios.get("http://useker.cn/getUsers")
	console.log(333)
	let res3 = await axios.get("http://useker.cn/getUsers")
	console.log(444)
	console.log(res1)
	console.log(res2)
	console.log(res3)
}
```
## axios的使用
#### GET请求
要传递的参数在{params:}中传递
```
axios.get('/user?ID=12345')
  .then(function (response) {
    // 处理成功情况
    console.log(response);
  })
  .catch(function (error) {
    // 处理错误情况
    console.log(error);
  })
  .then(function () {
    // 总是会执行
  });
```
上述请求也可以按以下方式完成（可选）
```
gantt
dateFormat YYYY-MM-DD
section S1
T1: 2014-01-01, 9d
section S2
T2: 2014-01-11, 9d
section S3
T3: 2014-01-02, 9d
```

```
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  })
  .then(function () {
    // 总是会执行
  });  
```
支持async/await用法
```
async function getUser() {
  try {
    const response = await axios.get('/user?ID=12345');
    console.log(response);
  } catch (error) {
    console.error(error);
  }
}
```
#### POST请求
将要传递的参数封装在一个对象传递
```
axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```
**发起多个并发请求**
```
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}

Promise.all([getUserAccount(), getUserPermissions()])
  .then(function (results) {
    const acct = results[0];
    const perm = results[1];
  });
```
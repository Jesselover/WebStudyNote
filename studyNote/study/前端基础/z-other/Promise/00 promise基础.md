# Promise
概念：
- 抽象表达：Promise 是 ES6 引入的异步编程的新解决方案。（旧的方案是单纯的回调函数）
- 具体表达：Promise 是一个构造函数，由 Promise 产生的对象用来封装一个异步操作，并且可以获取其成功/失败的最终结果。
 
## 1.基本使用

构造函数 `Promise()` ，实例化的时候接受一个函数类型的参数，此函数有两个函数类型的形参，分别为resolve，reject。promise实例对象可以包裹一个异步任务。
1. 参数：`executor`
 `executor`的参数：
	- resolve：异步任务成功时的回调函数,调用完后，将promise对象的状态设置为成功
	- reject：异步任务失败时的回调函数，调用完后，将promise对象的状态设置为失败
2. 方法：
	1. p.then((value)=>{ },(reason)=>{ })
		- 参数：两个函数。第一个函数是对象成功的回调，第二个是对象失败的回调
		- value，resolve（）传来的参数
		- reason，reject（）传来的参数
	
## 1.promise封装

方法一：原始方法
```js
// 封装一个minReadfile函数
// 参数：path 文件路径
// 返回：promise对象
function minReadfile(path) {
//promise函数封装
    return new Promise((resolve, reject) => {
        require('fs').readFile(path, (err, data) => {
            if (err) reject(err);
            else resolve(data);
        })
    });
};
minReadfile('./file.txt').then(value => {
    console.log(value.toString());
}, reason => {
    console.log(reason);
})
```

方法二：`util.promisify(要封装的函数)`
	采用遵循常见的错误优先的回调风格的函数（也就是将 `(err, value) => ...` 回调作为最后一个参数），并返回一个返回 promise 的版本。
```js
//引入util
const util = require('util');
const fs = require('fs');
//promise函数封装
const minReadfile = util.promisify(fs.readFile);
minReadfile('./file.txt').then(value => {
    console.log(value.toString());
}, reason => {
    console.log(reason);
})
```

## 2.promise状态`PromiseState`
1. promise实例中的属性 `PromiseState`
	- 待定（`pending`）: 未决定（初始状态）
	- 成功（`fulfilled/resoved`）: 成功
	- 失败（`rejected`）: 失败
2. 变换（有且只有两种可能）
	- 由 `pending` 变为 `fulfilled`
	- 由 `pending` 变为 `rejected`
	- 一个promise实例对象，只能改变一次
	- 成功的结果数据一般称为`value` , 失败的结果数据一般称为`reason`
3. 状态的改变 #important
	1. `resolve` 函数：由 `pending` => `fulfilled`
	2. `reject` 函数：由 `pending` => `rejected`
	3. 抛出错误`throw xxx`：由 `pending` => `rejected`


## 3.Promise的值 `PromiseResult`
1.  promise实例中的属性`PromiseResult`
	- 保存异步任务【成功/失败】的结果
2. `resolve` `reject` 函数可以对 `PromiseResult` 进行修改

# 优势
#### 1.指定回调函数的方式更加灵活

- 在出现 Promise 以前，纯回调方式：在启动异步任务的同时，必须先指定好回调函数，否则会拿不到数据。
- Promise：启动异步任务 => 返回 Promise 对象 => 给 promise 绑定回调函数。甚至可以在异步任务结束后再指定回调函数，而这在单纯的回调方式中是不行的。


#### 2. 支持链式调用，解决回调地狱 #important
回调地狱：
	回调地狱：回调函数嵌套调用，外部回调函数异步执行的结果是嵌套的回调函数执行的条件。  
	回调地狱的缺点：不便于阅读，不便于异常处理
```js
		doSomething(function(result) {
		  doSomethingElse(result, function(newResult) {
		    doThirdThing(newResult, function(finalResult) {
		      console.log('Got the final result: ' + finalResult);
		    }, failureCallback);
		  }, failureCallback);
		}, failureCallback);
```


一个 `Promise` 对象具有 `then` 和 `catch` 方法，而 `then` 和 `catch` 的返回结果依然是一个 `Promise` 对象，因此可以使用 `.` 符号进行链式调用。

对于上述伪代码，使用 `Promise`的形式改写如下：

```js
// 把回调绑定到返回的 Promise 上，形成一个 Promise 链
doSomething().then(function(result) {
  return doSomethingElse(result);
})
.then(function(newResult) {
  return doThirdThing(newResult);
})
.then(function(finalResult) {
  console.log('Got the final result: ' + finalResult);
})
.catch(failureCallback);
```

链式调用格式：
```js
const p = new Promise(resolve=>{}, reject=>{});
p.then(value=>{}, reason=>{})
.then(value=>{}, reason=>{})
.then(value=>{}, reason=>{})
...
```

#### 3.终极方案：使用 async/await

`Promise` 链式调用已经解决了回调地狱函数层层嵌套的问题了，还需要解决什么问题么？没错，我们还要砍掉 `Promise` 的回调函数，**回调函数太多，代码写的很长**。

`async/await` 是 ES8 出的一个更加优雅的异步编程解决方案，它可以让异步操作代码变得像同步代码一样，看起来非常优雅而直观。

上述例子，使用 `async/await` 改写的形式：
```js
async function request() {
  try {
    const result = await doSomething()
    const newResult = await doSomethingElse(result)
    const finalResult = await doThirdThing(newResult)
    console.log('Got the final result: ' + finalResult);
  } catch (error) {
    failureCallback(error)
  }
}
```

这里提一下，后面会详细讲解。


# API


> [!TIP]
> 本节参考MDN，加上自己的理解，以及老师的PPT，强烈建议阅读原文。
> MDN链接：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/all

#### 1. Promise 构造函数
`Promise(excutor){}`
- `executor` 函数：执行器 `(resolve, reject) => { }`  
- `resolve` 函数：内部定义，成功时我们调用的函数 `value => {}`
- `reject` 函数：内部定义，失败时我们调用的函数 `reason => {}`

> [!TIP]
> executor 会在 Promise 内部立即同步调用，而异步操作则在 executor 执行器中执行。

#### 2. Promise.prototype.then()

指定异步任务【成功/失败】后的回调函数
`Promise.prototype.then(onResolve,onRejected)`
- `onFulfilled/onResolve` 函数：成功的回调函数 `(value) => {}`
- `onRejected` 函数：失败的回调函数 `(reason) => {}`
- 返回一个新的promise对象
语法：
```js
p.then(onFulfilled, onRejected);
p.then(value => {
  // fulfillment
}, reason => {
  // rejection
});
```

#### 3. Promise.prototype.catch()

指定异步任务【失败】后的回调函数
`Promise.prototype.catch(onRejected)`
- `Promise.prototype.then(undefined, onRejected)`的语法糖

语法：
```js
p.catch(onRejected);
p.catch(reason => {
   // 操作失败
});
```
等同于：
```js
p.then(undefined, onFulfilled);
p.then(undefined, reason => {
  // 操作失败
})
```

#### 4. Promise.resolve()

```js
Promise.resolve(value);
```
参数：
- `value`：被解析的参数，可以是一个值，也可以是一个 Promise 对象，或者是一个 thenable
返回值：【promise对象】
- 如果传入的参数为【非promise对象】，则返回的结果为【成功的promise对象】
- 如果传入的参数为【promise对象】，则直接返回【这个promise对象】

#### 5. Promise.reject()

`Promise.reject()` 方法快速返回一个失败状态的 Promise 对象，接收一个 `value` 参数，表示失败的原因。
语法：
```js
Promise.reject(reason);
```
参数：
- `reason`：表示操作失败的原因
返回值：
- 返回一个失败（状态为 `rejected` ）的 Promise 对象
%%就算reason是一个成功的promise对象,其结果也是`rejected`%%

#### 6. Promise.all()

`Promise.all(iterable)`  返回一个新的promise,只有所有的promise都成功了才成功,不然就失败.
```js
Promise.all(iterable);
Promise.all(promise1, promise2, ...)
const p = Promise(promise1, promise2)
p.then(values=>)
```

参数：
- `iterable`：包含多个promise对象的数组
返回值：
- 成功:每一个promise成功的结果组成的数组
- 失败:第一个promise失败的值
##### 手写`promise.all`的实现
```js

```


#### 7. Promise.race()

`Promise.race(iterable)`  返回一个新的promise,类似于`Promise.all(iterable)` 结果第一个改变状态(先完成)的promise对象决定。

参数：
- `iterable`：可迭代对象，一般为包含多个 promise 的数组

返回值：
- 返回一个 promise，只要给定的可迭代对象中的 promise 有一个先完成（成功或失败），就采用它的值作为最终 promise 的值，状态与这个完成的 promise 相同。



# （二）Promise 的理解和使用

## 1. Promise 的理解
%%定义：
MDN 上的定义：一个 `Promise` 对象代表一个在这个 `Promise` 被创建出来时不一定已知的值。它让您能够 **把异步操作最终的成功返回值或者失败原因和相应的处理程序关联起来**。 这样使得异步方法可以像同步方法那样返回值：**异步方法并不会立即返回最终的值，而是会返回一个 promise，以便在未来某个时候把值交给使用者**%%。

- 抽象表达：Promise 是 ES6 引入的异步编程的新解决方案。（旧的方案是单纯的回调函数）
- 具体表达：Promise 是一个构造函数，由 Promise 产生的对象用来封装一个异步操作，并且可以获取其成功/失败的最终结果。

### 1.2 Promise 的状态

一个 `Promise` 必然处于以下几种状态之一：
- 待定（`pending`）: 初始状态，既没有被兑现，也没有被拒绝。
- 成功（`fulfilled`）: 意味着操作成功完成。
- 失败（`rejected`）: 意味着操作失败。

一个 `Promise` 的状态只能由 `pending` 变为 `fulfilled`，或者变为 `rejected`，且只能改变一次。

链式调用：`Promise` 对象具有 `then` 和 `catch` 方法来处理下一步的操作。因为 `Promise.prototype.then` 和 `Promise.prototype.catch` 方法返回的是 `Promise`，所以它们可以被链式调用。

### 1.3 Promise 的基本流程

![Promise](https://cdn.jsdelivr.net/gh/Hacker-C/Picture-Bed@main/front-end/Promise.rtseidrtnm8.webp)

一个 `Promise` 对象刚被创建时的初始状态为 `pending`。接下来会执行异步操作，若异步操作成功，则执行 `resolve()` 函数，可将成功返回的数据通过 `resolve(data)` 返回。若异步操作失败，则执行 `reject()` 函数，将失败的信息返回。

无论成功还是失败，返回的都是 `Promise` 对象，具有 `then()` 和 `catch()` 方法。

返回的 `Promise` 使用 `then()` 调用：
```js
promise.then(successCallback, failureCallback);
```
其中，`successCallback` 是成功的回调函数，`failureCallback` 是失败的回调函数。一般格式为：
```js
promise.then(value=>{}, reason=>{})
```

### 1.4 Promise 的基本使用
- [ ] 
%%构造函数 `Promise()`内部自动执行一个 **执行器函数**，也叫 `executor`。这个执行器函数又接收两个参数，第一个为执行成功的回调函数（`resolve`），第二个为操作成功的回调函数（`reject`）。%%
构造函数 `Promise()` ，实例化的时候接受一个函数类型的参数，此函数有两个函数类型的形参，分别为resolve，reject。promise可以包裹一个异步任务。
resolve：异步任务成功时调用
reject：异步任务失败时调用

需要注意，**executor 执行器函数是同步回调函数**。  
例如有以下代码：
```js
new Promise(
  // 执行器函数开始
  (resolve, reject) => {
    console.log(1)
    // 异步操作
    setTimeout(() => {
      console.log(2)
    }, 0)
    console.log(3)
  }
)
console.log(4)
```
打印结果是：1、3、4、2

**基本使用流程**
```js
const promise = new Promise((resolve, reject) => {

  // 执行异步操作
  // 操作成功，可将数据传入
  resolve(data)
  // 操作失败，可将错误信息传入
  reject(err_msg)

})

// 处理返回结果
promise.then(
  // 操作成功的回调
  value => { },
  // 操作失败的回调
  reason => { }
)
```

具体例子如下：
```js
// 1. 创建 Promise 对象，此时为 pending 状态
const promise = new Promise((resolve, reject) => {
  // 2. 执行异步操作，这里是定时器函数
  setTimeout(() => {
    let time = Date.now()
    if (time % 2 === 1) {
      // 2.1 假定 time 为奇数时为操作成功，则执行 resolve
      resolve('成功的值：' + time)
    } else {
      // 2.1 假定 time 为偶时为操作失败，则执行 reject
      reject('失败的值：' + time)
    }
  }, 1000)
})

// 3. promise 能指定成功或失败的回调函数来获取成功的 value，或失败的 reason
promise.then(
  // 3.1 操作成功（resolve）的回调函数
  value => {
    console.log(value)
  },
  // 3.1 操作失败哦（reject）的回调函数
  reason => {
    console.log(reason)
  }
)
```

## 2. Promise优势

原因：
- 指定回调函数的方式更加灵活
- 支持链式调用，解决回调地狱问题

### 2.1 指定回调函数的方式更加灵活

- 在出现 Promise 以前，纯回调方式：在启动异步任务的同时，必须先指定好回调函数，否则会拿不到数据。
- Promise：启动异步任务 => 返回 Promise 对象 => 给 promise 绑定回调函数。甚至可以在异步任务结束后再指定回调函数，而这在单纯的回调方式中是不行的。

下面举例说明。
1. MDN 上的例子
	使用 MDN 上的例子：假设现在有一个名为 `createAudioFileAsync()` 的函数，它接收一些配置和两个回调函数，然后异步地生成音频文件。一个回调函数在文件成功创建时被调用，另一个则在出现异常时被调用。创建一个文件需要一些时间，因此是一个异步回调。
	[^参考：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises]
	为了方便说明，下面都是伪代码。
	```js
		// 成功的回调函数
		function successCallback(result) {
		  console.log("声音文件创建成功: " + result);
		}
		// 失败的回调函数
		function failureCallback(error) {
		  console.log("声音文件创建失败: " + error);
		}
	```

	传统的纯回调方式，需要先指定好成功和失败的回调函数。也就是说，在文件创建的结果出来之前，回调函数必须先指定好，否则文件的结果无法处理。
	
	```js
		createAudioFileAsync(audioSettings, successCallback, failureCallback)
	```

	对于 `Promise`，无需考虑这方面。**在启动异步任务之前或者之后，都可以指定好回调函数。这样，异步任务指定回调函数的方式就更加灵活了**。

	```js
		const promise = createAudioFileAsync(audioSettings);
		promise.then(successCallback, failureCallback);
	```

2. 实际例子

	上述说明比较抽象，下面来一个实际情况的例子。
	
	假如现在有一个 ajax 请求，获取服务器的某用户的名字。从发出请求到获得结果需要花费一些时间，这里使用定时器模拟请求。

	```js
		function success(name) {
		  console.log('成功：' + name)
		}
		
		function failure(msg) {
		  console.log('失败：' + msg)
		}
	```

	传统纯回调方式
	
	```js
		function ajax(success, failure) {
		  setTimeout(() => {
		    // 这里可以假设获取数据成功或失败
		    success('peter')
		  }, 1000)
		}
		
		// 在启动异步任务的同时，必须先指定好 success 和 failure
		ajax(success, failure)
		// 输出：peter
	```

	Promise 方式

	```js
		const promise = new Promise((resolve, reject) => {
		  // 模拟 ajax 请求，1s后获取到数据
		  setTimeout(() => {
		    resolve('peter')
		  }, 1000)
		})
		// 此时异步任务已启动，不必先指定回调
	```

	可以在异步任务启动之后，在指定好回调函数

	```js
		promise.then(value => {
		  success(value)
		}, reason => {
		  failure(reason)
		})
		// 输出：peter
	```

	甚至可以在异步任务完成后指定。请求在 1s 完成，而指定回调队列在 2s 完成。
	
	```js
		setTimeout(() => {
		  promise.then(value => {
		    success(value)
		  }, reason => {
		    failure(reason)
		  })
		}, 2000)
		// 输出：peter
	```

### 2.2 支持链式调用，解决回调地狱 #important

#### 1 回调地狱

回调地狱：回调函数嵌套调用，**外部回调函数异步执行的结果是嵌套的回调函数执行的条件**。  
回调地狱的缺点：**不便于阅读，不便于异常处理**。

%%开发中经常会遇到多重异步操作，就会出现下面的写法（伪代码）：
```js
doSomething(function(result) {
  doSomethingElse(result, function(newResult) {
    doThirdThing(newResult, function(finalResult) {
      console.log('Got the final result: ' + finalResult);
    }, failureCallback);
  }, failureCallback);
}, failureCallback);
```
例如，现在要获取一个用户所有的商品数据，首先要根据用户名获取用户ID，然后根据ID获取订单信息，根据订单信息获取商品信息。一共要发起三次请求，前面的请求结果是后面请求的条件。%%
#### 2.解决方法
##### 1 链式调用，解决回调地狱

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

## 3. 如何使用 Promise 

### 3.1 Promise API

> [!TIP]
> 本节参考MDN，加上自己的理解，以及老师的PPT，强烈建议阅读原文。
> MDN链接：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/all

#### 1. Promise 构造函数

Promise 构造函数接收一个执行器 `executor`，来启动这个 Promise。

```js
new Promise(executor)
```

这个 `executor` 执行器函数又接收两个参数 `resolve` 和 `reject`，分别表示操作成功和操作失败。

```js
new Promise((resolve, reject) => {
  resolve()
  // reject()
})
```

- `executor` 函数：执行器 `(resolve, reject) => { }`  
- `resolve` 函数：内部定义成功时我们调用的函数 `value => {}`
- `reject` 函数：内部定义失败时我们调用的函数 `reason => {}`

> [!TIP]
> executor 会在 Promise 内部立即同步调用，而异步操作则在 executor 执行器中执行。

#### 2. Promise.prototype.then()

`Promise.prototype.then` 方法绑定在原型 `prototype` 上，意味着这是一个实例方法，必须在实例对象上才能使用。该方法返回的结果依然是一个 `Promise` 对象，意味着我们可以进行链式调用。

语法：
```js
p.then(onFulfilled, onRejected);

p.then(value => {
  // fulfillment
}, reason => {
  // rejection
});
```

参数：
- `onFulfilled` 函数：成功的回调函数 `(value) => {}`
- `onRejected` 函数：失败的回调函数 `(reason) => {}`

返回值：  
如果 `then` 中的回调函数：
- 返回了一个值，那么 `then` 返回的 Promise 将会成为接受状态（`fulfilled`），并且将返回的值作为接受状态的回调函数的参数值。
- 没有返回任何值，那么 `then` 返回的 Promise 将会成为接受状态（`fulfilled`），并且该接受状态的回调函数的参数值为 undefined。
- 抛出一个错误，那么 `then` 返回的 Promise 将会成为拒绝状态（`rejected`），并且将抛出的错误作为拒绝状态的回调函数的参数值。
- 返回一个已经是接受状态（`fulfilled`）的 Promise，那么 `then` 返回的 Promise 也会成为接受状态，并且将那个 Promise 的接受状态的回调函数的参数值作为该被返回的Promise的接受状态回调函数的参数值。
- 返回一个已经是拒绝状态（`rejected`）的 Promise，那么 `then` 返回的 Promise 也会成为拒绝状态，并且将那个 Promise 的拒绝状态的回调函数的参数值作为该被返回的Promise的拒绝状态回调函数的参数值。
- 返回一个未定状态（`pending`）的 Promise，那么 `then` 返回 Promise 的状态也是未定的，并且它的终态与那个 Promise 的终态相同；同时，它变为终态时调用的回调函数参数与那个 Promise 变为终态时的回调函数的参数是相同的。

> [!TIP]
> 以上参考MDN，个人觉得讲的很详细。链接：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/then

#### 3. Promise.prototype.catch()

`catch()` 方法返回一个 `Promise`，并且处理操作失败的情况。它的行为与调用 `Promise.prototype.then(undefined, onRejected)` 相同。但是用起来更方便了，可以看作是一种 **语法糖**（让我们执行某些操作更加方便简洁的方法）。

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

参数：
- `onRejected`：操作失败时的回调函数 `(reason) => {}`

返回值：
- 返回一个 `Promise` 对象。如果 `onRejected` 抛出一个错误或返回一个本身失败的 `Promise`，那么 `catch()` 返回的 `Promise` 的状态为 `rejected`；否则，状态为 `fulfilled`。

#### 4. Promise.resolve()

`Promise.resolve()` 是一个函数对象方法，意味着只能用在函数对象 `Promise` 上。它接收一个 `value` 参数，返回一个这个给定值解析后的 `Promise` 对象。

语法：
```js
Promise.resolve(value);
```

参数：
- `value`：被解析的参数，可以是一个值，也可以是一个 Promise 对象，或者是一个 thenable

返回结果：
- 若给定值为一个普通数、字符串或数组等等之类的，则返回一个成功的 Promise
- 若参数本身就是一个 Promise 对象，则直接返回这个 Promise 对象。
- 由此可知，`resolve()` 方法可以返回一个成功或者失败的 Promise 对象

#### 5. Promise.reject()

`Promise.reject()` 方法返回一个失败的 Promise 对象，接收一个 `value` 参数，表示失败的原因。

语法：
```js
Promise.reject(reason);
```

参数：
- `reason`：表示操作失败的原因

返回值：
- 返回一个失败（状态为 `rejected` ）的 Promise 对象

#### 6. Promise.all()

`Promise.all()` 方法接收由多个 promise 组成的可迭代类型（Array、Map、Set等），然后返回一个新的 promise。它等待所有 promise 完成，或者第一个失败的 promise。

```js
Promise.all(iterable);

Promise.all(promise1, promise2, ...)

const p = Promise(promise1, promise2)
p.then(values=>)
```

参数：
- `iterable`：可迭代类型，一般为数组

返回值：
- 若传入的参数为空的可迭代对象，例如 `Promise.all([])`，则返回一个已完成状态（`fulfilled`）的 Promise
- 若传入的参数不包含任何 promise，则返回一个异步完成（`asynchronously resolved`）的 Promise。例如 `Promise.all([1, 2])`
- 其它情况下返回一个处理中（`pending`）的 Promise。只有接收的参数的 promise 全部成功，这个返回的 Promise 才会是成功的，只要有一个失败，则返回一个失败的 Promise，且返回的值为第一个失败的值。
- 若所有 promise 都成功，则返回的 Promise 的值为一个包含了多个结果的数组

Promise.all 的同步与异步：
- **当如果传入的可迭代对象是空的，就是同步；其他情况都是异步**。

举例:  
参数为非空的数组，异步的：
```js
const p = Promise.all([1])
setTimeout(() => {
  console.log(p)
})
console.log('同步代码')
console.log(p) // (1)
// 输出：
// 同步代码
// Promise {<pending>}
// Promise {<fulfilled>: Array(1)}
```
注意：此时（1）输出状态为 `pending`，因为同步代码先执行，而此时异步 promise 的结果还未知

参数为空数组，同步的：
```js
const p1 = Promise.all([])
setTimeout(() => {
  console.log(p1)
})
console.log('同步代码')
console.log(p1) // （1）
// 输出
// 同步代码
// Promise {<fulfilled>: Array(0)}
// Promise {<fulfilled>: Array(0)}
```
注意：此时（1）输出状态为 `fulfilled`，说明此时 `Promise.all` 是同步的。

**Promise.all 方法举例**

```js
// 参数为空的数组
const p1 = Promise.all([])

// 参数为不包含 promise 的数组
const p2 = Promise.all([1, 2, 3])

// 参数为全部是成功的 promise 的数组
let pm1 = Promise.resolve(1)
let pm2 = Promise.resolve(2)
let pm3 = Promise.resolve(3)
const p3 = Promise.all([pm1, pm2, pm3])

// 结果
setTimeout(() => {
  console.log(p1)
  console.log(p2)
  console.log(p3)
  // Promise {<fulfilled>: Array(0)}
  // Promise {<fulfilled>: Array(3)}
  // Promise {<fulfilled>: Array(3)}
})

// 参数为包含失败的 promise 的数组
let pe1 = Promise.resolve(1)
let pe2 = Promise.reject('error')
let pe3 = Promise.resolve(3)
const p4 = Promise.all([pe1, pe2, pe3])

setTimeout(() => {
  console.log(p4)
})
// 输出
// Promise {<rejected>: 'error'}
```

#### 7. Promise.race()

`Promise.race()` 也是接收一个可迭代类型，例如一个包含了多个 promise 的数组。然后返回一个新的 promise，第一个完成的 promise 的结果状态就是最终返回的结果状态。

需要注意，这里最终返回的结果的状态不一定是参数数组中位于前面的 promise，具体要看哪一个最先完成，所以 `race()` 方法的返回结果可能为成功或失败的。

语法：
```js
Promise.race(iterable);
```

参数：
- `iterable`：可迭代对象，一般为包含多个 promise 的数组

返回值：
- 返回一个 promise，只要给定的可迭代对象中的 promise 有一个先完成（成功或失败），就采用它的值作为最终 promise 的值，状态与这个完成的 promise 相同。

Promise.race 的异步性（没有同步性）：
```js
const p = Promise.race([Promise.resolve(10), Promise.resolve(20)])

// 立即输出p，此时p状态为 pending
console.log(p)

setTimeout(() => {
  // 等栈中代码执行完毕之后执行，可得到完成的promise
  console.log('the stack is now empty');
  console.log(p)
})

// 输出：
// Promise { <state>: "pending" }
// the stack is now empty
// Promise { <state>: "fulfilled", <value>: 33 }
```
注意：`Promise.race` 没有同步性

##### 2.终极方案：使用 async/await

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


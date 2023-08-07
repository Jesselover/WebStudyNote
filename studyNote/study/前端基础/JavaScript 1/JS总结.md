https://juejin.cn/post/6844904136161361933

## 1.变量类型

### 1.类型判断
#### 常见方法
1. typeof
	- string、number、undefined、boolean、symbol、bigint、**function**
	- null%%object%% array %%object%%
2. instanceof %%看原型链%%

4. xxx.constructor === xxx
5. Object.prototype.toString.call(xxx)
```js
Object.prototype.toString.call({});
<!--"[object Object]"-->
Object.prototype.toString.call([]);
<!--"[object Array]"-->
Object.prototype.toString.call(function(){});
<!--"[object Function]"-->
Object.prototype.toString.call('');
<!--"[object String]"-->
Object.prototype.toString.call(1);
<!--"[object Number]"-->
Object.prototype.toString.call(true);
<!--"[object Boolean]"-->
Object.prototype.toString.call(null);
<!--"[object Null]"-->
Object.prototype.toString.call(undefined);
<!--"[object Undefined]"-->
Object.prototype.toString.call();
<!--"[object Undefined]"-->
Object.prototype.toString.call(new Date());
<!--"[object Date]"-->
Object.prototype.toString.call(/at/);
<!--"[object RegExp]"-->
```
#### 类型	
1. 判断非null基本数据类型：typeof
2. 判断数组
	- 使用`Array.isArray()`判断数组
	- 使用`[] instanceof Array`判断是否在Array的原型链上，即可判断是否为数组%%console.log(arr instanceof Object);//true%%
	- `[].constructor === Array`通过其构造函数判断是否为数组%%console.log(arr.constructor === Object);//false%%
	- 也可使用`Object.prototype.toString.call([])`判断值是否为'[object Array]'来判断数组

3. 判断对象
	- `{} instanceof Object` 判断是否在Object的原型链上，即可判断是否为对象
	- `{}.constructor === Object` 通过其构造函数判断是否为对象
	- `Object.prototype.toString.call({})` 结果为 '[object Object]'则为对象
	
4. 判断函数
	- 使用 `func typeof function` 判断func是否为函数
	- 使用 `func instanceof Function` 判断func是否为函数
	- 通过 `func.constructor === Function` 判断是否为函数
	- 也可使用 `Object.prototype.toString.call(func)` 判断值是否为'[object Function]'来判断func

5. 判断null

	- 最简单的是通过 `null===null` 来判断是否为null %%`Object.prototype.__proto__===a` 判断a是否为原始对象原型的原型即null%% %%本质：null === null%%
	-   `typeof (a) == 'object' && !a` 通过typeof判断null为对象，且对象类型只有null转换为Boolean为false

6. 判断是否为NaN
	-  `isNaN(any)`直接调用此方法判断是否为非数值

一些其他判断
	-  `Object.is(a,b)` 判断a与b是否完全相等，与 `===` 基本相同，不同点在于Object.is判断 `+0不等于-0`，`NaN等于自身`


### 2. 类型转换
[JS类型转换机制 - 掘金 (juejin.cn)](https://juejin.cn/post/7175542145447641125#heading-4)

#### 隐式转换
布尔：需要布尔的时候
string： +
number： 除了+，其他所有的运算符

#### 显示类型转换
1. 转化为number
	1. paseInt/paseFloat(string) 
		1. 去单位
		%%2. 取整%%
		3. 返回值为number
		4. string首为字母直接返回NaN
	2. Number() 很严格，只要有一个字符无法转成数值，整个字符串就会被转为`NaN`

2. 转化为string
	1. xxx.toString() undefined,null会报错
	2. String() 啥都行
```js
    // 数值：转为相应的字符串
    String(1) // "1"
    //字符串：转换后还是原来的值
    String("a") // "a"
    //布尔值：true转为字符串"true"，false转为字符串"false"
    String(true) // "true"
    //undefined：转为字符串"undefined"
    String(undefined) // "undefined"
    //null：转为字符串"null"
    String(null) // "null"
    //对象
    String({a: 1}) // "[object Object]"
    String([1, 2, 3]) // "1,2,3"
```

3. 转化为布尔
Boolean()
false:0, "", NaN, undefined, null
true:所有引用类型

#### " == "与" === "

" == "先类型，后数值
1. undefined == null
2. string == number ,string转number
3. Boolean出现转null
4. 引用数据类型：string --> number/NaN --> 比较

![[Pasted image 20221228173915.png]]

注意：
```js
console.log(undefined == 0);//false

console.log(null == 0);//false
```

# other
## 1.垃圾回收 Garbage Collectation
- 周期性、开销较大、GC时停止响应其他操作
1. 局部变量才会被回收，全局变量的声明周期直至浏览器卸载页面才会结束


## 2. 进程与线程

进程：
	1. 程序的一次执行,它**占有一片独有的内存空间**
	2. 可以通过windows任务管理器查看进程
	3. 程序：多进程、单进程

线程：
	1. 是进程内的一个独立执行单元
	2. 是程序执行的一个完整流程 %%应用程序必须运行到某个进程的某个线程上%%
	3. 是CPU的最小的调度单元

注意：
	1. 应用程序必须运行在某个进程的某个线程上
	2. 一个进程中至少有一个运行的线程:主线程                 %%进程启动后自动创建%%
	3. 一个进程中也可以同时运行多个线程%%此时我们会说这个程序是多线程运行的%%
	4. 多个进程之间的数据是不能直接共享的                    %%内存相互独立(隔离)%%
	5. **线程池(thread pool**):保存多个线程对象的容器%%实现线程对象的复用%%

### 多线程与单线程
多线程:
- 优点:能有效提升CPU的利用率
- 缺点
  1. 创建多线程开销
  2. 线程间切换开销 
  3. 死锁与状态同步问题

单线程:
 - 优点:顺序编程简单易懂
 - 缺点:效率低

**JS是单线程运行的 , 但使用H5中的 Web Workers可以多线程运行**

浏览器是多线程

### 3. 浏览器内核

主线程
	1. js引擎模块 : 负责js程序的编译与运行
	2. html,css文档解析模块 : 负责页面文本的解析(拆解)
	3. dom/css模块 : 负责dom/css在内存中的相关处理
	4. 布局和渲染模块 : 负责页面的布局和效果的绘制
	5. 布局和渲染模块 : 负责页面的布局和效果的绘制

分线程


# 实例
## 1.轮播图
[2022年了，你还不会手撕轮播图？ - 掘金 (juejin.cn)](https://juejin.cn/post/7072683227625816072)

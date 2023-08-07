## 静态页面练习

  

##### 1. 字体设置后无法显示

  

##### 2.“收款方信息”效果如何实现

![[Pasted image 20230321104058.png]]

  

下载新字体

  

##### 3.代码格式化后自动将单引号变为双引号，不符合开发规范

根目录文件夹新建`.prettierrc.json`; 设置 `{ “singleQuote”: true }`

  
  

## 组件开发练习

  

1. props父组件给子组件传值，`value = "5"` 是string `:value = "5"` 是数字

  

## 注意事项

#### 1. 字体设置后无法显示

  

#### 2.“收款方信息”效果如何实现

![[Pasted image 20230321104058.png]]

  

如果字数确定，放一条白线盖住

#### 开发规范

  

1. 正则中不要使用控制符、空字符

2. 代码提交时，不使用debug、alert

3. 不对变量进行delete操作

4. 不使用eval  ()，注意饮食的eval ()

5. 不扩展原生对象

```js

Object.prototye.age = 11

```

6. 避免多余的1函数上下文绑定

7. 避免不必要的布尔转换

8. 不使用多余的括号包裹函数

9. 避免对类名进行重新赋值，避免对声明过的函数进行重新赋值

10. 外部变量不与对象属性重名

11. 不要使用原始包装器

```js

const massage = new String('holle')  //避免

```

12. return语句中的赋值必须由括号包裹

```js

return (res = a + b)

```

13. 禁止使用稀疏数组（稀疏数组就是数组中，大部分的元素值都未被使用（或都为0），在数组中仅有少 部分的空间使用。%%空间浪费%%）

14. 使用throw抛错，抛出Eorror对象而不是字符串

15. 不使用undefined初始化变量

16. 关系运算符的左值不做取反操作

17. 展开运算符与它的表达式间不要留白

```js

fn(...args)

```

18. 注释首尾留空格

19. 禁止使用Symbol构造器

20. **使用getPrototypeOf(obj) 代替__proto__**

     `Object.getPrototypeOf()`方法返回指定对象的原型（内部`[[Prototype]]`属性的值）。[Object.getPrototypeOf() - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getPrototypeOf)

21. 调用匿名函数使用()包裹、

  

#### 其他问题

  

##### 1. 路由起始页面配置

```JS

// src/config/index.js

startPage:'/index',

  

//src/router/constant-routes.js //路由功能配置

constantRoutes.push({

path: '/',

redirect: startPage

})

```

  

##### 2. `require.context()`

是 webpack 的一个特殊函数，用于在模块中请求一组模块。它返回一个函数，该函数有三个属性：`resolve`、`keys` 和 `id`。

-   `resolve` 用于查找模块的绝对路径。

-   `keys` 返回一个包含所有可能请求的模块名称的数组。

-   `id` 返回上下文模块的ID。

  

```js

const context = require.context('./test', false, /\.test\.js$/); console.log(context.keys());

// ["./a.test.js", "./b.test.js"] console.log(context('./a.test.js'));

// './test/a.test.js'

```

  

该例中第一个参数 './test' 指定了要搜索的文件夹

第二个参数 false 指定是否要搜索子文件夹

第三个参数 /.test.js$/ 指定了要搜索的文件正则表达式。

  

总结来说，`require.context`是 webpack 内置的读取文件夹模块的功能，可以在代码中调用并获取文件夹中的模块。

  

##### 3. vue组件注意事项

vue组件的模板会在某些情况下受到HTML的限制，比如：table 里面只能写 tr ，td，th 等。如果要使用组件，可以使用特殊的“is”属性来挂在组件
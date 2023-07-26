# 使用

node ：使用nvm管理
https://www.cnblogs.com/gaozejie/p/10689742.html
node版本：16.18.0 （eg.）

1. 安装nvm
2. 打开cmd
	`nvm install 16.18.0`
	`nvm use 16.18.0`

# 文档

1. 浏览器中js运行环境：引擎 + 内置API
	- 引擎：解析、执行代码
	- API：由运行环境提供的特殊接口，只能在所属的运行环境中被调用

2. node.js 基于chrome V8引擎的js后端运行环境([Node.js (nodejs.org)](https://nodejs.org/en/))

3. 注意：
	1. 浏览器是js前端运行环境；node.js是js后端运行环境
	3. node.js无法使用dom，bom


## 1.命令行窗口

命令行窗口,CMD窗口,终端,shell

terminal (终端)
	使用node运行js代码
	打开终端 ==> 输入`cd 目录名`

快捷键
	1. 上箭头 快速定位到上一次执行的命令
	2. tab 快速补全路径
	3. esc 快速清除
	4. 当前输入的命令
	5. cls 清除当前终端


### 1.1常用指令

1. `dir`:列出当前目录下的所有文件
2. `cd 目录名` 进入指定目录 %%eg:`cd Desktop`进入桌面%%

|  符号 | 含义 | 网页 | 
| -------| ----- | ---|
| `.` | 当前目录 | `./` |
| `..` |上一级目录| `../` |

3. `md 目录名` 创建一个文件夹
3. `rd 目录名` 删除一个文件夹
4. `目录名` 打开此文件夹
5. `node` enter 

### 1.2环境变量

- 控制面板=>系统属性=>高级=>环境变量
- Path
	-  里面是以`;`间隔的路径
	- 当我们在cmd中打开一个文件或者调用一个程序
		1. 先在当前目录下找,找到了直接打开
		2. 没找到依次到环境变量path的路径中寻找
		3. 没到到则报错
	-  应用:将经常访问的文件或者程序的路径放在path中,这样就可以在任意地方访问到他了.
	
## 2.进程与线程

- 进程:为程序的运行提供必备的环境
- 线程:负责执行保存进程中的程序
	- 单线程:**js是单线程**
	- 多前程:大多数服务器
	
## 3.模块化

### 3.1概念

1. 在node中,一个js文件就是一个模块
2. 在node中,每一个js文件中的js代码都是独立运行在一个函数中,而不是全局作用域.因此一个模块中的变量和函数在其他模块中无法访问.要通过export来暴露.
	1. node在执行模块中的代码时,会用下面的函数把代码包裹起来,并传进5个实参.
	```js
	function (exports, require, module, __filename, __dirname) { 
	
	}
	```
`
	`exports`: 对象,将变量或函数暴露到外部
	`require`: 函数,用来引入外部模块
	`module`:
		- 代表当前模块本身
		- exports就是module的属性`module.exports==exports`
		- 即可用`exports`暴露,也可以用`module.exports`暴露.本质无区别
	`__filename`:当前模块的完整路径
	`__dirname`:当前模块所在文件夹的完整路径
3.  模块分类与模块标识:
	- 核心模块:由node引擎提供,模块标识就是其名字
	- 文件模块:由用户自己创建,模块标识是它的路径

#### require import的区别

[[前端基础/JavaScript 1/ECMAScript6+/ch02#19. ES6 模块化|ch02]]

[面试：require 和 import 的区别？ - 掘金 (juejin.cn)](https://juejin.cn/post/7014011266796617736)

| |require|import|
|-|-------|------|
|标准|CommonJs|ES6|
|输出|值的拷贝|值的引用|
|执行|运行时执行，同步加载，是一个赋值过程|编译时执行，异步加载，是解构过程|
|性能|稍低（运行时才能引入模块并且赋值给某个对象）| 稍快（只需根据import中的接口，在编译时引入指定模块）|


### 3.2暴露与引入

1. 引入:在node中,通过require()函数来引入外部模块
	- 参数:一个文件的路径.使用相对路径必须以`.`或`..`开头
	- 返回值:一个对象,表示引入的模块
2. 暴露:在node中,通过exports向外部暴露变量或方法
	- 如何暴露:将要暴露的属性或方法设置为exports的属性
	- `module.exports`与`exports`的区别
		1. 浅拷贝.`exports = module.exports`,也就是说`exports`指向`module.exports`
		2. `exports`之可以通过`exports.属性=xxx`的形式暴露.
		   `module.exports`可以如此,也可以直接赋值
		   %% `module.exports = { }`%%
		   %% `module.exports.xxx = xxx`%%
		   

## 4.包与npm包管理器
### 4. 1包
[[02 包与 npm 包管理器]]
    
## 5.Buffer缓冲区
- 数组中不能存储二进制文件（图片/视频/音频等媒体文件）
- Buffer 与数组类似的对象， 专门用来保存二进制数据。
	1. Buffer输出的是十六进制。但只要数字在控制台或页面输出一定是10进制。
	2. 每一个元素占一个字节，范围是`00-ff`(0-255)
	3. Buffer是对底层内存的直接操作
		1. 大小一旦确定，不能修改
		2. 所以可能残留着别人用过的数据
- Buffer 不需要导入，直接用
- Buffer 构造函数都是不推荐使用的

1. 创建Buffer
	```js
	// 创建一个指定size大小的Buffer，分配空间的时候同时清空数据
	var buf = Buffer.alloc(size);  
	// 通过索引来操作buf中的元素
	buf[0] = 88;
	//创建一个指定size大小的Buffer，但不清空数据
	var buf = Buffer.allocUnsafe(size);   
	```
	- `Buffer.allocUnsafe`性能更好,但是其内可能含有敏感数据,容易泄露


## 6.文件系统

用来操作文件的模块

1. 导入fs模块
```js
const fs = require("fs")
```

####  常用方法与属性

1. 读取指定文件中的内容
	[fs 文件系统 | Node.js API 文档 (nodejs.cn)](http://nodejs.cn/api-v16/fs.html#fsreadfilepath-options-callback)
	
	```JS
	fs.readFile(path[,options],callback(err,datastr))
	```
	
	`path` : string,文件路径
	`options` : 以什么编码格式来读取文件，默认`utf8`
	`callback` : 文件读取完成后，通过回调函数拿到读取结果 
		读取成功：`err :null ; dataStr :成功的结果 `
		读取失败：`err :错误对象 ; dataStr : undefined`

2. 向指定文件夹中写入内容
[fs 文件系统 | Node.js API 文档 (nodejs.cn)](http://nodejs.cn/api-v16/fs.html#fswritefilefile-data-options-callback)

```js
fs.writeFile(file,data[,options],callback(err,data))
```

`path` : string,文件路径
`data` : 要写入的内容
`[,options]` :  以什么编码格式来写入文件，默认`utf8`
`callback` ：文件写入完成过，调用此函数
	写入成功：`err :null ; data :成功的结果 `
	写入失败：`err :错误对象 ; data : undefined`

# 注意:
1.golbal:node中没用window.但有一个全局对象global,它的作用和网页中的window类似,**在全局中创建的变量都会作为global的属性保存**


	
	
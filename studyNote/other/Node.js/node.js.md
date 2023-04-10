# node.js

## 1. 相关概念

1. 浏览器中js运行环境：引擎 + 内置API
	- 引擎：解析、执行代码
	- API：由运行环境提供的特殊接口，只能在所属的运行环境中被调用

2. node.js 基于chrome V8引擎的js后端运行环境([Node.js (nodejs.org)](https://nodejs.org/en/))

3. 注意：
	1. 浏览器是js前端运行环境；node.js是js后端运行环境
	3. node.js无法使用dom，bom


## 2. 终端
terminal
1. 使用node运行js代码
	打开终端 ==> 输入`cd 目录名`

#### 快捷键
1. 上箭头 快速定位到上一次执行的命令
2. tab 快速补全路径
3. esc 快速清除
4. 当前输入的命令
5. cls 清除当前终端

#### 常用命令
1. `dir`:列出当前目录下的所有文件
2. `cd 目录名` 进入指定目录 %%eg:`cd Desktop`进入桌面%%
|符号|含义|网页|
|--|--|--|
|`.`|当前目录|`./`|
|`..`|上一级目录|`../`|
3. `md 目录名` 创建一个文件夹
3. `rd 目录名` 删除一个文件夹
4. `目录名` 打开此文件夹
5. `node 文件名` 运行此文件





## 3. fs文件系统模块

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

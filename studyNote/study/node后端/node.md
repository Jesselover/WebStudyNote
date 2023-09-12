- 基于chromeV8引擎的JavaScript运行时

特性：
- 非阻塞io
- 事件驱动

nodemon：热部署 （解决node需要重启服务器才能显示新内容的问题;自动重启）


## 模块化

- 全局模块： 随时随地可访问，不需要被引用，包括全局对象、全局变量 
- 核心模块： 不需要npm 下载，直接用require()引用 (eg.:path模块,fs模块,http模块)
- 自定义模块

### path模块

[path 路径 | Node.js v18 文档 (nodejs.cn)](https://nodejs.cn/dist/latest-v18.x/docs/api/path.html)

#### 路径相关

- `normalize`: 用于规范化给定的path
- `join`: 将所有给定的path片段连接在一起
- `resolve`: 解析为绝对路径
- `isAbsolute`: 检查path是否为绝对路径

#### 文件相关

- `basename`: 返回路径中最后一部分的文件名
- `extname`: 返回路径最后文件的扩展名
- `dirname`: 返回path路径中的文件名

#### 路径解析
- `parse`: 返回一个对象，其属性表示path有效元素
- `format`: 把对象转为一个路径字符串 


### buffer 模块

- buffer是一个全局变量 %% 无需引入，直接使用 %%
- buffer用来处理二进制数据流
- buffer实力类似数组，大小固定
- buffer是使用c++ 代码在V8堆外分配的物理内存
- 可以解决乱码问题

### events 模块


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

```js
console.log(path.normalize("a/b/c///d\\e"), "normalize"); //a\b\c\d\e normalize
console.log(path.join("a", "b", "../c/d"), "join"); //a\c\d join
console.log(path.resolve("a/b/c"), "resolve"); //F:\UNIAPP\Tips\NODE\a\b\c resolve
console.log(path.isAbsolute("a/b/c"), "isAbsolute"); //false isAbsolute
console.log(path.isAbsolute("/a/b/c"), "isAbsolute"); //true isAbsolute
```
#### 文件相关

- `basename`: 返回路径中最后一部分的文件名
- `extname`: 返回路径最后文件的扩展名
- `dirname`: 返回path路径中的文件名

```js
console.log(path.basename("../Blob/blob01.html"), "basename"); //blob01.html basename
console.log(path.extname("../Blob/blob01.html"), "extname"); //.html extname
console.log(path.dirname("../Blob/blob01.html"), "dirname"); //../Blob dirname
```

#### 路径解析

- `parse`: 返回一个对象，**其属性表示path有效元素**
- `format`: 把对象转为一个路径字符串 
```js
console.log(path.parse("../Blob/blob01.html"), "parse");
/**
 * {
      root: '', //根目录
      dir: '../Blob', //dirname
      base: 'blob01.html', //basename
      ext: '.html',
      name: 'blob01'****
    } parse
 */
var path01 = {
  root: "/", //根目录
  dir: "../Blob", //dirname
  base: "blob01.html", //basename
  ext: ".html",
  name: "blob01",
};
console.log(path.format(path01), "format"); //../Blob\blob01.html format
```
### buffer 模块

- buffer是一个全局变量 %% 无需引入，直接使用 %%
- buffer用来处理二进制数据流
- buffer实力类似数组，大小固定
- buffer是使用c++ 代码在V8堆外分配的物理内存
- 可以解决乱码问题

### events 模块


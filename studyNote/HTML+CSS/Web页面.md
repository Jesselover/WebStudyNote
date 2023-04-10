>  页面布局思路：

1. 确定版心（可视区）
2. 分析页面中的行模块（标准流），每行中的列模块（常用浮动）%%第一准则%%
3. 列模块常采用浮动，先确定大小，在确定位置 %%第二准则%%
4. 先理清结构，在写样式

## 1. 初始化
### 目录命名
* 项目文件夹：shoping
	* 样式文件夹：css
		* css 初始化样式文件： base.css
		* css 公共样式文件：common.css %%公用的header、footer%%
	* 脚本文件夹：js
	* 样式类图片文件夹：img/images
	* 产品类图片文件夹： upload %%需要经常更新%%
	* 字体类文件夹： fonts
	* 首页：index.html
	* favicon.ico

注意：* 通配符选择器选择效率较低，追求性能尽量不用

### 网站favicon图标

1. 制作
	1. 图标切成.png
	2. 第三方网站转化为ico图标
2. 放入网站根目录下
3. html页面引入favicon图标
	`<link rel="shortcut icon" href=" /favicon.ico" />`

### SEO搜索引擎优化

1. 三大标签 
	titile: 网站名（产品名）- 网站优化 %%30字以内%%
	description： 简明说明网站是做什么的
	keywords： 6-8个关键字，英文“,”分开

## 2. 常规做法

#### 1. logo提权
1. h1 标签放入logo %%提权,告诉搜索引擎,这个很重要%%
2. h1 标签里放入**链接** ,用于返回首页,把logo图片当作背景给链接a
3. 链接里面放**文字**(网站名称)，%%方便搜索引擎收录%%，但是文字不能显示出来
	1. text-indent：-9999px 移到盒子外面，然后overflow：hidden %%`text-indent` 属性能定义一个块元素首行文本内容之前的缩进量%%
	2. font-size:0; %%京东做法%%
4. 给链接a一个**title**属性，鼠标放上去就可以看到提示文字
```html
        <div class="logo">
            <h1>
                <a href="index.html" title="品优购商城">品优购商城</a>
            </h1>
        </div>
```
```css
/* header logo */
.logo {
    position: absolute;
    top: 25px;
    width: 171px;
    height: 61px;
}

.logo a {
    /* a要和logo一样大，点到logo上都有链接跳转效果 */
    display: block;
    width: 171px;
    height: 61px;
    /* logo作为a的背景图片 */
    background:url(../images/logo.png) no-repeat;
    /* 隐藏文字 */
    font-size: 0;
}
```


#### 2. 背景颜色渐变
```css
.nav-common:nth-child(3) {
    /* todo 背景颜色渐变必须添加浏览器私有前缀 */
    background: -webkit-linear-gradient(left,#34c2a9,#6cd559);
}
```

#### 3. “小三角”
1. 字体图标
2. 下边框+右边框+2d转换-旋转
```css
.more::after {
    content: "";
    position: absolute;
    top: 9px;
    right: 9px;
    width: 7px;
    height: 7px;
    border-top: 2px solid #fff;
    border-right: 2px solid #fff;
    transform: rotate(45deg);
}
```
## 3. 小点
1. 当盒子宽度与其内部文字或内容有关系时，不要给宽度，让其内文字撑大盒子
2. 行高也可能把盒子撑高 解决方法 overflow：hidden
3. 一般情况下，a如果包含有宽度的盒子，a需要转为块级元素
4. 登录页面比较隐私，不要左seo优化
5. 注册表用form>ul>li>input 做，更整齐
6. border-radius 
[🧐神奇的CSS用法之border-radius - 掘金 (juejin.cn)](https://juejin.cn/post/7170229984160661518)
7. line-height
[line-height - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/line-height)
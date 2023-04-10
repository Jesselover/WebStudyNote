[flex 布局的基本概念 - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox)
[(2条消息) flex布局笔记_csdmwinter的博客-CSDN博客](https://blog.csdn.net/csdmwinter/article/details/115047808?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166485023916782414943811%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166485023916782414943811&biz_id=0&spm=1018.2226.3001.4187)
[深入浅出之 Flex 弹性布局 - 掘金 (juejin.cn)](https://juejin.cn/post/7019075844664459278)

## 1.概述
1. 布局原理：就是通过给父盒子(容器)添加flex属性，来控制子盒子(项目)的位置和排列方式
2. 我们说 flexbox 是一种一维的布局，是因为一个 flexbox 一次只能处理一个维度上的元素布局，一行或者一列。%%作为对比的是另外一个二维布局 [CSS Grid Layout](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Grid_Layout)，可以同时处理行和列上的布局。%%
3. flex容器：文档中采用了 flexbox 的区域就叫做 flex 容器。为了创建 flex 容器，我们把一个容器的 [`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 属性值改为 `flex` 或者 `inline-flex`。完成这一步之后，容器中的**直系**子元素就会变为 **flex 元素**。所有 CSS 属性都会有一个初始值，所以 flex 容器中的所有 flex 元素都会有下列行为：

	- 容器中的直系子元素就会变为 **flex 元素**。  
	- 元素排列为一行 (`flex-direction` 属性的初始值是 `row`)。
	-   元素从主轴的起始线开始。
	-   元素不会在主维度方向拉伸，但是可以缩小。
	-   元素被拉伸来填充交叉轴大小。
	-   [`flex-basis`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex-basis) 属性为 `auto`。
	-   [`flex-wrap`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex-wrap) 属性为 `nowrap`。
	- 这会让你的元素呈线形排列，并且把自己的大小作为主轴上的大小。如果有太多元素超出容器，它们会溢出而不会换行。如果一些元素比其他元素高，那么元素会沿交叉轴被拉伸来填满它的大小。

## 2.常见父项属性
1.  flex-direction：设置主轴的方向(默认主轴是X)
	- flex-direction:column;将主轴改为y轴，纵轴
	- flex-direction:row; 将主轴改为x轴，横轴
	- flex-direction:row- reverse;主轴为x轴，并且翻转
	- flex-direction:column- reverse;主轴为y轴，并且翻转
	- 
2.  justify-content：设置主轴上的子元素排列方式（使用时确定好主轴是X还是Y）
	- flex-start 所有子元素在主轴头部显示
	- flex-end 所有子元素在主轴尾部显示
	- center 所有子元素在主轴居中对齐
	- space-around 所有子元素平分剩余空间
	- space-between 所有子元素先两边贴边在平分剩余空间

3. flex-wrap：设置子元素是否换行
	- 默认不换行，换行为属性值为wrap
	
4. align-items：设置侧轴上的子元素排列方式（单行）
	- flex-start 
	- flex-end 
	- center 
	- stretch：stretch 拉伸，沿着侧轴拉升，此处子元素**不要给高度**才有效果

5. align-content：设置侧轴上的子元素的排列方式（多行）
	- flex-start 
	- flex-end 
	- center 
	- stretch：stretch 拉伸，沿着侧轴拉升，此处子元素不要给高度才有效果

6. flex-flow：复合属性，相当于同时设置了flex-direction和flex-wrap
``` css
        div {
            display: flex;
            width: 800px;
            height: 400px;
            background-color: pink;
            /* flex-direction: column;
            flex-wrap: wrap; */
            flex-flow: column wrap;
         }
```


## 3.子项常见属性

![[basics7.png]]

### 1.flex属性
- flex属性定义子项目分配剩余空间，用flex来表示占多少份数
```css
        p {
            display: flex;
            width: 60%;
            height: 150px;
            background-color: pink;
            margin: 0 auto;
        }
        
        p span {
            flex: 1;
        }
        /* 2号盒子大一点 */
        p span:nth-child(2) {
            flex: 2;
            background-color: red;
        }
```
![[20210321131223183.png]]
- 应用：移动端，圣杯布局
### 2.
1. **align-self**控制子项自己在侧轴上的排序方式，允许单个项目与其它项目不一样的对齐方式，可覆盖align-items属性，默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch
```css
        div span:nth-child(3) {
            /* 第三个子元素沿着侧轴低侧对齐 */
            align-self: flex-end;
        }
```
![[align-self.png]]
2. **order**属性定义项目的排列顺序：数值越小，排列越靠前，默认为0。 _注意：和z-index不一样_
	- order 仅仅对元素的视觉顺序 (visual order) 产生作用，并不会影响元素的逻辑或 tab 顺序。 order 不可以用于非视觉媒体，例如 speech
## 4.应用
### 1. 居中（父）
### 2.圣杯布局（子）
```css
        /* 圣杯布局 */
      section {
        display: flex;
        background-color: thistle;
        height: 150px;
        width: 60%;
        margin: 0 auto;
      }
      section div:nth-child(1) {
        width: 100px;
        height: 150px;
        background-color: aqua;
      }
      section div:nth-child(3) {
        width: 100px;
        height: 150px;
        background-color: aqua;
      }
      section div:nth-child(2) {
        flex: 1;
        background-color: pink;
      }
  
```
```html
  <body>
    <section>
      <div></div>
      <div></div>
      <div></div>
    </section>
  </body>

```

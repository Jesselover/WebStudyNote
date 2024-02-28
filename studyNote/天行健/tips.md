# TiPS

## js
1. array.concat() 不改变原数组，返回一个新的数组
```js
//concat()把两个或者多个数组连接在一起，但是不改变已经存在的数组
//而是返回一个连接之后的新数组
var a = [1,2,3];
a.concat([4,5]);
console.log(a);
//此处输出为 [1, 2, 3]

var a = [1,2,3];
a = a.concat([4,5]);
console.log(a);
//此处输出为 [1, 2, 3 ,4 ,5]
```
Similar ：`arr1.push(...arr2)`

2. 可选链式调用
[深入理解并解决Uncaught TypeError: Cannot read properties of null“的全面指南“-CSDN博客](https://blog.csdn.net/tombosky/article/details/134333073)
[可选链运算符（?.） - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Optional_chaining)

## vue

```js
折叠
#region
#end region

```

### tip
1.  使用this.$nextTick()获取不到数据更新后的this.$refs.xxx._this.$refs获取不到-CSDN博客](https://blog.csdn.net/Rised/article/details/128437042
	[看完这篇nextTick就别再问别人了 - 掘金 (juejin.cn)](https://juejin.cn/post/7266374711823171636?searchId=20230928173417F9D75B5E5F12B1CF5A9E)

2. [解决Vue项目中img的:src动态加载图片不成功_vue img :src-CSDN博客](https://blog.csdn.net/weixin_46074961/article/details/115522372#:~:text=%E5%8E%9F%E5%9B%A0%EF%BC%9A%E5%8A%A8%E6%80%81%E5%9C%B0%E5%9D%80%EF%BC%8C%E8%B7%AF%E5%BE%84%E8%A2%AB%E5%8A%A0%E8%BD%BD%E5%99%A8%E8%A7%A3%E6%9E%90%E4%B8%BA%E5%AD%97%E7%AC%A6%E4%B8%B2%EF%BC%8C%E6%89%80%E4%BB%A5%E5%9B%BE%E7%89%87%E6%89%BE%E4%B8%8D%E5%88%B0,%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95%EF%BC%9A%20%E8%AE%BE%E7%BD%AE%E7%BB%9D%E5%AF%B9%E8%B7%AF%E5%BE%84%E6%88%96%E8%80%85%E7%9B%B8%E5%AF%B9%E8%B7%AF%E5%BE%84%E6%98%AF%E6%94%B9%E4%B8%BA%E7%94%A8require%E5%BC%95%E5%85%A5%E6%89%8D%E8%83%BD%E6%88%90%E5%8A%9F%EF%BC%8C%E5%B0%B1%E5%8F%AF%E4%BB%A5%E5%8A%A8%E6%80%81%E4%BD%BF%E7%94%A8%E4%BA%86%E3%80%82)
	原因：动态地址，路径被加载器解析为字符串，所以图片找不到  
	解决方法： 设置绝对路径或者相对路径是改为用require引入才能成功，就可以动态使用了。
	
3. [vue data和methods可以重名吗？](https://segmentfault.com/q/1010000007103404)
	如果真的重名了，data中的变量会覆盖methods中的方法
## element ui

### tip
[【vue】element ui 表格优化——解决显示错位问题-CSDN博客](https://blog.csdn.net/coralime/article/details/122979010#:~:text=%E3%80%90%E9%97%AE%E9%A2%98%E6%8F%8F%E8%BF%B0%E3%80%91ElementUI%20el-table%20%E5%8A%A8%E6%80%81%E6%98%BE%E7%A4%BA%E8%A1%A8%E6%A0%BC%E7%9A%84%E6%97%B6%E5%80%99%EF%BC%8C%E4%BC%9A%E5%8F%91%E7%94%9F%E6%98%BE%E7%A4%BA%E9%94%99%E4%BD%8D%E7%9A%84%E6%83%85%E5%86%B5%EF%BC%8C%E6%8B%96%E6%8B%BD%E4%B8%80%E4%B8%8B%E5%8F%88%E6%81%A2%E5%A4%8D%E6%AD%A3%E5%B8%B8%E4%BA%86%EF%BC%8C%E8%BF%99%E6%98%AF%E8%A6%81%E9%80%BC%E6%AD%BB%E5%BC%BA%E8%BF%AB%E7%97%87%E3%80%90%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95%E3%80%911.%20%E7%BB%99%E8%A1%A8%E6%A0%BC%E6%B7%BB%E5%8A%A0ref%E6%A0%87%E5%BF%97%20%3Cel-table%20ref%3D%22tableRef%22%20%3Adata%3D%22tableData%22%3E%3C%2Fel-table%3E2.doLayout%E5%AF%B9%20Table,-%20The%20world%27s%20most%20popular%20Vue%20UI%20frameworkwa..)

封装select下拉框全选组件
[element select 添加全选_element select 全选_狗狗狗狗亮的博客-CSDN博客](https://blog.csdn.net/weixin_44046951/article/details/127259112?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169475590016800215062736%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=169475590016800215062736&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-2-127259112-null-null.142^v94^chatsearchT3_1&utm_term=element%20ui%20select%E5%86%85%E6%B7%BB%E5%8A%A0%E5%85%A8%E9%80%89%E5%8A%9F%E8%83%BD&spm=1018.2226.3001.4187)
[Element-ui中 选择器（select）多选下拉框实现全选功能 - 掘金 (juejin.cn)](https://juejin.cn/post/7091640007961608205?searchId=20230915151325E11DD15EC3D53CCF0BA3)

自定义序号
[element-UI——el-table添加序号 - cecelia - 博客园 (cnblogs.com)](https://www.cnblogs.com/ceceliahappycoding/p/10723702.html)
[min-width not working in table-掘金 (juejin.cn)](https://juejin.cn/s/min-width%20not%20working%20in%20table)


[el-input内容超出输入框鼠标悬浮展示_elinput展示不全信息鼠标移入展示-CSDN博客](https://blog.csdn.net/qq_45093219/article/details/133088813)
### 改变el-table某列样式

### 使用 v-for 动态渲染多个 el-form 如何进行校验

要实现 el-form 表单动态增删功能，可以定义一个 formList 数组，使用 v-for 进行动态渲染 。但是数组内部的每一个 el-form 表单如何验证呢？
 el-form 表单验证
 1. 规则设置：在 el-form 中设置 rules，或者在 el-form-item 中设置 rules。
 2. 验证：` his.$refs.formName.validate( valid => { }) ` ，返回的 valid 为 true 则校验成功，`formName` 为当前表格的 ref。
v-for 渲染出的el-form 表单验证
1.   规则设置：只能在  el-form-item 中设置 rules。（在 el-form 中设置 rules会失败，我的提示文字就没成功）
2. 验证：方法不变，但是循环出的 el-form 需要各自的、独一无二的 ref。我这里使用 `"'infoForm'+infoForm.index"` 来unique的表示 el-form，其中 `infoForm.index` 为当前 el-table 在 formList 中的索引，你可以使用其他的方法。

话不多说，看代码：

1.   结构：
```html
    <ul>
      <li v-for="(infoForm) in  infoList"
          :key="infoForm.index">
        <hr>
        <el-form :model="infoForm"
                 :ref="'infoForm'+infoForm.index">  
          <el-form-item label="描述"
                        :rules="infoRules.description"
                        prop="description">
              <el-input v-model="infoForm.description"/>
          </el-form-item>
        </el-form>
      </li>
    </ul>
```

	核心代码
	`:ref="'infoForm'+infoForm.index"` 
	`:rules="infoRules.description"`
	

2. 数据
```JS      
reportRules: {
        description: { required: true, trigger: 'blur', message: '请输入问题描述' },
      },
infoList:[
{
	description:"好心态决定女人的一生"，
	index:0, // 当前对象在数组中的索引
}
]
```

3. 验证
```js
for (let i = 0; i < this.infoList.length; i++) {
          const refStr = "infoForm" + i
          const form = this.$refs[refStr][0] // 注意 ！！！
          form.validate(valid => {
            if(valid){
              // 验证通过代码
            } else {
            this.$message.error("有必填项未填写！")
            }
          })
```
我将 `this.$refs[refStr]`打印出来后是个数组，因此要取 `this.$refs[refStr][0]`
![[Pasted image 20240202111622.png]]


## Echart
### Echart表格异常显示问题

#### 缩成一团

[解决ECharts图表切换后缩成一团的问题_echarts缩成一团_King汀的博客-CSDN博客](https://blog.csdn.net/qq_28691187/article/details/112302031?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~default-2.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~default-2.nonecase)


问题描述：使用 `v-show` 控制echart图标显示与否。发现，在图表未渲染出前，`v-show=false` ,图表会缩成一团。渲染出后，改变`v-show` 无异常。
网上查到的原因是，窗口一开始隐藏，初始化的时候获取不了你容器的高度和宽度。

解决方法：
1. 用一个盒子挤出echart图表，overflow：hidden
```html
<div class="contain">
  <div class="tool">
    将echart图表挤出contain的工具盒子
  </div>
  <div class="echart">
    echart图标
  </div>
</div>
```
2. 动态控制这个盒子的高度，显示echart图表时，设置tool盒子高度为0；不显示时，高度为echart图表的高度，将echart图表挤出。


## CSS
### 多行文本溢出

```css
.two_rows {
  text-align: left;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}
```
在处理多行文本溢出问题的时候，使用了 `-webkit-` 属性，没想到火狐浏览器也支持
还专门有一个chrome文件
火狐源代码索引工具 [搜索狐 (searchfox.org)](https://searchfox.org/)
https://searchfox.org/mozilla-central/source

![[Pasted image 20230831103454.png]]

其他有用的文档
[Microsoft Edge 开发人员文档 - Microsoft Edge Development | Microsoft Learn](https://learn.microsoft.com/zh-cn/microsoft-edge/developer/)


# TiPS
[使用this.$nextTick()获取不到数据更新后的this.$refs.xxx._this.$refs获取不到-CSDN博客](https://blog.csdn.net/Rised/article/details/128437042
[看完这篇nextTick就别再问别人了 - 掘金 (juejin.cn)](https://juejin.cn/post/7266374711823171636?searchId=20230928173417F9D75B5E5F12B1CF5A9E)

[【vue】element ui 表格优化——解决显示错位问题-CSDN博客](https://blog.csdn.net/coralime/article/details/122979010#:~:text=%E3%80%90%E9%97%AE%E9%A2%98%E6%8F%8F%E8%BF%B0%E3%80%91ElementUI%20el-table%20%E5%8A%A8%E6%80%81%E6%98%BE%E7%A4%BA%E8%A1%A8%E6%A0%BC%E7%9A%84%E6%97%B6%E5%80%99%EF%BC%8C%E4%BC%9A%E5%8F%91%E7%94%9F%E6%98%BE%E7%A4%BA%E9%94%99%E4%BD%8D%E7%9A%84%E6%83%85%E5%86%B5%EF%BC%8C%E6%8B%96%E6%8B%BD%E4%B8%80%E4%B8%8B%E5%8F%88%E6%81%A2%E5%A4%8D%E6%AD%A3%E5%B8%B8%E4%BA%86%EF%BC%8C%E8%BF%99%E6%98%AF%E8%A6%81%E9%80%BC%E6%AD%BB%E5%BC%BA%E8%BF%AB%E7%97%87%E3%80%90%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95%E3%80%911.%20%E7%BB%99%E8%A1%A8%E6%A0%BC%E6%B7%BB%E5%8A%A0ref%E6%A0%87%E5%BF%97%20%3Cel-table%20ref%3D%22tableRef%22%20%3Adata%3D%22tableData%22%3E%3C%2Fel-table%3E2.doLayout%E5%AF%B9%20Table,-%20The%20world%27s%20most%20popular%20Vue%20UI%20frameworkwa..)

封装select下拉框全选组件
[element select 添加全选_element select 全选_狗狗狗狗亮的博客-CSDN博客](https://blog.csdn.net/weixin_44046951/article/details/127259112?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169475590016800215062736%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=169475590016800215062736&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-2-127259112-null-null.142^v94^chatsearchT3_1&utm_term=element%20ui%20select%E5%86%85%E6%B7%BB%E5%8A%A0%E5%85%A8%E9%80%89%E5%8A%9F%E8%83%BD&spm=1018.2226.3001.4187)
[Element-ui中 选择器（select）多选下拉框实现全选功能 - 掘金 (juejin.cn)](https://juejin.cn/post/7091640007961608205?searchId=20230915151325E11DD15EC3D53CCF0BA3)

自定义序号
[element-UI——el-table添加序号 - cecelia - 博客园 (cnblogs.com)](https://www.cnblogs.com/ceceliahappycoding/p/10723702.html)

[min-width not working in table-掘金 (juejin.cn)](https://juejin.cn/s/min-width%20not%20working%20in%20table)

# MYTIPS
## Echart表格异常显示问题

### 缩成一团

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


## 多行文本溢出

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

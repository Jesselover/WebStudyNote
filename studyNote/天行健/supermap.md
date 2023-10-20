
# superMap 从入门到入土

[iClient for Leaflet 开发指南 (supermap.io)](https://iclient.supermap.io/web/introduction/leafletDevelop.html)

https://iclient.supermap.io/web/apis/leaflet.html

[Vue 项目接入使用超图 SuperMap_supermap vue-CSDN博客](https://wjw1014.blog.csdn.net/article/details/121719996?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-121719996-blog-123503149.235%5Ev38%5Epc_relevant_sort_base3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-121719996-blog-123503149.235%5Ev38%5Epc_relevant_sort_base3&utm_relevant_index=2)

[Leaflet 1.9.3 中文文档 - 帮助手册&教程 - 《leaflet 中文文档 - 帮助手册 - 教程》 - 极客文档 (geekdaxue.co)](https://geekdaxue.co/read/leaflet-cn/v1.9.3-doc)

[【精选】基于Vue快速搭建超图二维iClient开发环境-CSDN博客](https://blog.csdn.net/supermapsupport/article/details/109294147)

**WebGIS 系统开发三要素**  
1. 数据（SuperMap iDesktop）
2. iServer服务(SuperMap iServer)
3. WebGIS开发(SuperMap iClient)


## 初始化

根据开发指南，尝试在 vue 中引入失败。于是先在html中试用摸索。

[《Leaflet 进阶知识点》- Leaflet.draw 交互绘制_AvatarGiser的博客-CSDN博客](https://blog.csdn.net/sinat_31213021/article/details/119735922)


## leaflet

[Vue-CLI and Leaflet (1)：显示一个地图 - 掘金 (juejin.cn)](https://juejin.cn/post/6844903830639869966?searchId=20231019093416544904481A4DE42A1C1B)


[leaflet 加载高德地图_leaflet加载高德地图-CSDN博客](https://blog.csdn.net/qq_37550440/article/details/120328413)

### tileLayer

用来在地图上载入和显示切片图层，用ILayer接口实现。

[Documentation - Leaflet - a JavaScript library for interactive maps (leafletjs.com)](https://leafletjs.com/reference.html#tilelayer)
[Leaflet地图框架使用手册——L.TileLayer_leaflet tilelayer-CSDN博客](https://blog.csdn.net/black2Girl/article/details/85264597)

subdomains：服务的子域。可以传递一个字符串（其中每一个字母都是一个子域名称）或是一个字符串数组。

### marker

[Leaflet地图框架使用手册——L.Marker-CSDN博客](https://blog.csdn.net/black2Girl/article/details/85264410)
[【精选】Leaflet 自定义修改Marker点标记样式_l.marker-CSDN博客](https://blog.csdn.net/ddxshf/article/details/111036397)

### 图层 layer

[Documentation - Leaflet - a JavaScript library for interactive maps (leafletjs.com)](https://leafletjs.com/reference.html#geojson)

[leaflets学习——geoJSON图层_geojson leaflet-CSDN博客](https://blog.csdn.net/qq_40821274/article/details/107354367)

###  tooltip Popup
[Documentation - Leaflet - a JavaScript library for interactive maps (leafletjs.com)](https://leafletjs.com/reference.html#popup)
[Documentation - Leaflet - a JavaScript library for interactive maps (leafletjs.com)](https://leafletjs.com/reference.html#tooltip)


## SuperMap iClient

[【精选】基于Vue快速搭建超图二维iClient开发环境-CSDN博客](https://blog.csdn.net/supermapsupport/article/details/109294147)

## application

1. 图上展示车辆的实时位置，可以参考这个：
https://www.zhankr.net/137780.html

2. 地图上打点，画线，可以参考这个
https://blog.csdn.net/weixin_48203356/article/details/130559185
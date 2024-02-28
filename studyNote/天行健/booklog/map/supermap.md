**** 
# superMap 从入门到入土

[iClient for Leaflet 开发指南 (supermap.io)](https://iclient.supermap.io/web/introduction/leafletDevelop.html)

https://iclient.supermap.io/web/apis/leaflet.html

[Vue 项目接入使用超图 SuperMap_supermap vue-CSDN博客](https://wjw1014.blog.csdn.net/article/details/121719996?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-121719996-blog-123503149.235%5Ev38%5Epc_relevant_sort_base3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-121719996-blog-123503149.235%5Ev38%5Epc_relevant_sort_base3&utm_relevant_index=2)

[Leaflet 1.9.3 中文文档 - 帮助手册&教程 - 《leaflet 中文文档 - 帮助手册 - 教程》 - 极客文档 (geekdaxue.co)](https://geekdaxue.co/read/leaflet-cn/v1.9.3-doc)


**WebGIS 系统开发三要素**  
1. 数据（SuperMap iDesktop）
2. iServer服务(SuperMap iServer)
3. WebGIS开发(SuperMap iClient)


## 初始化

[《Leaflet 进阶知识点》- Leaflet.draw 交互绘制_AvatarGiser的博客-CSDN博客](https://blog.csdn.net/sinat_31213021/article/details/119735922)

[【精选】基于Vue快速搭建超图二维iClient开发环境-CSDN博客](https://blog.csdn.net/supermapsupport/article/details/109294147)

### 环境搭建注意事项

1. 引入失败
新建 `babel.config.js` 文件
```json
{ 
"plugins": [
    [ "@supermap/babel-plugin-import",
    { 
    "libraryName": "@supermap/iclient-leaflet"
    }
    ] 
   ] }

```
2. 使用api报错
>  由于11i版本更新，如需使用ES6语法，添加配置项后，只支持按需引入，否则会报　TypeError: Cannot read properties of undefined (reading ‘tiledMapLayer’)的错误。

## leaflet

```javascript
npm install leaflet --save
```

[leafLet入门教程兼leafLet API中文文档参考_leafletdraw中文文档-CSDN博客](https://blog.csdn.net/qq_36595013/article/details/83144874)

[Vue-CLI and Leaflet (1)：显示一个地图 - 掘金 (juejin.cn)](https://juejin.cn/post/6844903830639869966?searchId=20231019093416544904481A4DE42A1C1B)

[leaflet 加载高德地图_leaflet加载高德地图-CSDN博客](https://blog.csdn.net/qq_37550440/article/details/120328413)

[简介 | leafletjs-example](https://leafletjs-example.netlify.app/examples/)

###  map

[Leaflet中实现添加、隐藏、自定义缩放Zoom控件_leaflet 控件如何隐藏默认加载的_霸道流氓气质的博客-CSDN博客](https://blog.csdn.net/BADAO_LIUMANG_QIZHI/article/details/123923364)
### tileLayer

用来在地图上载入和显示切片图层，用ILayer接口实现。

[Documentation - Leaflet - a JavaScript library for interactive maps (leafletjs.com)](https://leafletjs.com/reference.html#tilelayer)
[Leaflet地图框架使用手册——L.TileLayer_leaflet tilelayer-CSDN博客](https://blog.csdn.net/black2Girl/article/details/85264597)

subdomains：服务的子域。可以传递一个字符串（其中每一个字母都是一个子域名称）或是一个字符串数组。

### marker

[Leaflet地图框架使用手册——L.Marker-CSDN博客](https://blog.csdn.net/black2Girl/article/details/85264410)
[【精选】Leaflet 自定义修改Marker点标记样式_l.marker-CSDN博客](https://blog.csdn.net/ddxshf/article/details/111036397)

自定义 marker 方向
[vue + leaflet实现图标指定方向随机旋转_leaflet图标旋转-CSDN博客](https://blog.csdn.net/qq_40920553/article/details/131375089?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-131375089-blog-121115675.235%5Ev38%5Epc_relevant_sort_base3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-131375089-blog-121115675.235%5Ev38%5Epc_relevant_sort_base3&utm_relevant_index=2)

### icon
[《Leaflet 基础知识点》- 添加图形标注_leaflet iconanchor-CSDN博客](https://blog.csdn.net/sinat_31213021/article/details/113245291)
![[Pasted image 20231116140631.png]]
### 图层 layer

[Documentation - Leaflet - a JavaScript library for interactive maps (leafletjs.com)](https://leafletjs.com/reference.html#geojson)

[leaflets学习——geoJSON图层_geojson leaflet-CSDN博客](https://blog.csdn.net/qq_40821274/article/details/107354367)

###  tooltip Popup
[Documentation - Leaflet - a JavaScript library for interactive maps (leafletjs.com)](https://leafletjs.com/reference.html#popup)
[Documentation - Leaflet - a JavaScript library for interactive maps (leafletjs.com)](https://leafletjs.com/reference.html#tooltip

### 聚合 Clustering/Decluttering

Leaflet.markercluster
`npm i leaflet.markercluster `

### 反向地理编码

[L.esri.Geocoding.Geocode |Esri 宣传册 |ArcGIS 开发人员](https://developers.arcgis.com/esri-leaflet/api-reference/tasks/geocode/)
[反向地理编码的基础知识—ArcGIS Pro | 文档](https://pro.arcgis.com/zh-cn/pro-app/latest/help/data/geocoding/fundamentals-of-reverse-geocoding.htm)
[Leaflet.js + ArcGIS Server开发入门_esri-leaflet_我只吃了一碗粉的博客-CSDN博客](https://blog.csdn.net/qq_42552251/article/details/125497339)
## SuperMap **iClient**

[【精选】基于Vue快速搭建超图二维iClient开发环境-CSDN博客](https://blog.csdn.net/supermapsupport/article/details/109294147)
[iClient for Leaflet 开发指南 (supermap.io)](https://iclient.supermap.io/web/introduction/leafletDevelop.html)


## application

1. 图上展示车辆的实时位置，可以参考这个：
https://www.zhankr.net/137780.html

2. 地图上打点，画线，可以参考这个
https://blog.csdn.net/weixin_48203356/article/details/130559185

[Leaflet 学习心路历程之 —— 自定义 Popup 基础教学（自定义Marker标记气泡）_leaflet popup-CSDN博客](https://blog.csdn.net/qq_43438095/article/details/107025110?spm=1001.2101.3001.6650.7&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-7-107025110-blog-121952723.235%5Ev38%5Epc_relevant_sort_base3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-7-107025110-blog-121952723.235%5Ev38%5Epc_relevant_sort_base3&utm_relevant_index=14)

### 自定义 Popup，实现数据动态显示

[Leaflet 1.9.3 中文文档 - 帮助手册&教程 - 《leaflet 中文文档 - 帮助手册 - 教程》 - 极客文档 (geekdaxue.co)](https://geekdaxue.co/read/leaflet-cn/v1.9.3-doc#popup)

```js
var popup = L.popup()
    .setLatLng(latlng)
    .setContent(‘<p>Hello world!<br />This is a nice popup.</p>’)
    .openOn(map);
```
leaflet 官方示例中，content 里面是html ，不能识别 vue 语法，这为动态数据的绑定造成了困难。

解决方法：在popup中显示vue组件
[Leaflet中实现在popup中展示Vue组件功能 - 掘金 (juejin.cn)](https://juejin.cn/post/7056010219624595487)

1.  新建info 组件，通过prop接收数据
```js
  props: {
    nowDetail: {
      default: () => {
        return {};
      },
      type: Object,
    },
  },
```
2. 在父组件中引入info 组件 ;(根据自己的需要选择是否隐藏显示，我的是直接被map覆盖掉了，看不见；使用visible属性，可能要在开启、关闭popup时进行一些操作)
```html
    <DetailWindow :nowDetail="nowDetail"
                  ref="detailWindow">
    </DetailWindow>
```
```js
import DetailWindow from './info.vue'
```
3. 将`this.$refs.detailWindow.$el`绑定在popup上
```js
 this.popup = L.popup().setContent(this.$refs.detailWindow.$el)
```



##### 1. 只改变位置，不改变方向
```js
    createMarker (item) {
      // 打点,item 为包含打点信息的对象
      let markerImg = require('/src/assets/images/offline.png')
      if (item.onLineBus == 1) {
        markerImg = require('/src/assets/images/online.png')
      }
      if (item.onLineBus == 2) {
        markerImg = require('/src/assets/images/offline.png')
      }
      const icon = L.divIcon({
        html: `
          <div  style=' width: 70px !important;'>
              <img src=${markerImg} 
           height=40
           style='transform: rotate(${item.direction}deg);margin:0 auto'/>
            <div  style=' width: 70px !important;'>${item.carNo}</div>
            </div>`,
        className: 'truck_icon'
      })
      const marker = L.marker(
        [item.lat, item.lng],
        {
          icon,
          title: item.carNo
        }).on('click', () => {
          this.markerClick(marker, item.vin, 0)
        })
      return marker
    },
    
	updateMarker (item) {
	// this.nowMarkers 是以item.vin 为key，对应的marker标记点为value的map
	  if (this.nowMarkers.has(item.vin)) {
		this.nowMarkers.get(item.vin).setLatLng([item.lat, item.lng])
		this.popup.setLatLng([item.lat, item.lng])
		this.map.setView([item.lat, item.lng])
	  }
	},
```
##### 2. 即改变位置，也改变方向
leaflet改变方向
1. 插件
[vue + leaflet实现图标指定方向随机旋转_leaflet图标旋转-CSDN博客](https://blog.csdn.net/qq_40920553/article/details/131375089?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-131375089-blog-121115675.235%5Ev38%5Epc_relevant_sort_base3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-131375089-blog-121115675.235%5Ev38%5Epc_relevant_sort_base3&utm_relevant_index=2)

2. 利用css translate属性
```js
    updateMarker (item) {
      if (this.nowMarkers.has(item.vin)) {
      // ! 由于要改变车辆方向，因此只能删除后重新打点
        this.truckLayer.removeLayer(this.nowMarkers.get(item.vin))
        this.createMarker(item).addTo(this.truckLayer)
        this.popup.setLatLng([item.lat, item.lng])
        this.map.setView([item.lat, item.lng])
      }
    },
```

## 历史轨迹

[Leaflet 带箭头轨迹以及沿轨迹带方向的动态marker_leaflet绘制带箭头的线-CSDN博客](https://blog.csdn.net/gisarmory/article/details/114290334)

插件准备
1. 下载 `MovingMarker.js` ,并引入
[ewoken/Leaflet.MovingMarker: A Leaflet plug-in to create moving marker (github.com)](https://github.com/ewoken/Leaflet.MovingMarker)

2. 下载`leaflet-polylinedecorator` ,并引入
` npm i leaflet-polylinedecorator `
```js
import * as L.Polylinedecorator from 'leaflet-polylinedecorator';
```
[npm包 leaflet-polylinedecorator 使用教程-JavaScript中文网-JavaScript教程资源分享门户 (javascriptcn.com)](https://www.javascriptcn.com/post/37739)



## bugs

#### 1. L.divIcon 去除自带得小方框

如图所示，使用divIcon 出现自带的小方框
`.leaflet-div-icon` 控制了这个样式
使用 `::v-deep .leaflet-div-icon` 并不起作用

![[Pasted image 20231026114021.png]]

解决方法：
```js
  const icon = L.divIcon({
          html: `
          <img src=${markerImg}
           height=40
           width=40/>`,
          className:'icon'
        })
```
```css
.icon {
  /* 不想要样式，设为空即可 */
}
```
![[Pasted image 20231026114748.png]]

车辆定位
1. supermap/iclient-leaflet、leaflet环境搭建；超图地图引入；地图缩放、边界等参数调整；
2. 接口请求与数据整理
3. 地图打点marker动态图标、车牌号标签制作； 车辆信息详情页制作；
5. 聚集功能
6. table表格

历史轨迹
1. 历史轨迹基础页面绘制
2. 接口请求与数据整理
4. 历史轨迹查询功能：地图上的轨迹绘制；起点、终点打点
5. 轨迹播放功能


#### 2. 超图地图引入工作

1. 根据地址进入地图页面
![[Pasted image 20231031173129.png]]

- 点击红圈查看地图具体信息，包括中心点等数据；引入插图地图时，使用此页面地址
![[5415145db39f0f097263727e71ac2ad.png]]

- 点击红色方块预览地图

1. 车辆监控页面地图、表格绘制；切换、隐藏表格功能完成
2. 消息提示框功能50%
3. 前往东庄水利出差沟通超图地图引入细节；构建超图引入环境（田总承诺下周一前给地图）
4. websocket连接成功
5. 基础打点、聚集功能完成，车辆数选择车辆打点 50%

#### 3. 缩放到指定大小，定位到指定位置后，打开 openPopup 失败

目标：点击 marker 或者 车牌号，定位到车辆位置，设置合适的zoom，并且openPopup打开信息弹窗

问题：点击车牌号后，有时无法正确打开openPopup

原因：
1. popup绑定在了marker上
3. 考虑marker点在openPopup时未打在地图上。
使用了 `L.markercluster` 聚集功能，zoom缩放到一定背书后才会显示marker点

解决方法：
1. 改变代码顺序，缩放后在openPopup ；如果仅改变顺序不行，可以使用setTimeOut
2. 直接创建popup弹窗，不要与marker绑定

方法2详细代码如下：
```js
// 地图初始化
this.popup = L.popup().setContent('html-content')
// 打开popup
 this.popup
	.setLatLng(marker.getLatLng())
	.openOn(this.map)
```


所需要图层：

1. 目前提供卫星地图： 实现车辆在东庄内定位，地图范围外呈灰色
2. 标注地图：在卫星地图上标记地点
3. 发布iserver地址匹配服务：显示车辆“最后定位位置” （最后定位位置 由经纬度+地址匹配服务 计算的出）；如不能提供，可以使用高德地图地址匹配功能，需要连接高德地图 ； 发布参考：https://blog.csdn.net/supermapsupport/article/details/77933357
1. 发布东庄地图rest服务：将东庄地图与高德地图结合，在东庄内显示东庄卫星地图，东庄外显示高德地图（需要连接高德）

#### 4. 使用L.setView 后，手动变换地图，地图闪回原视野
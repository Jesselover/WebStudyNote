# GIS 基础

webGis： 前端可视化技术与GIS技术相结合

二维：openLayer、MapBox、Leaflet
三维：cesium

## 开发术语（参考高德）

[地图组成与名词解释-入门-教程-地图 JS API 1.4|高德地图API (amap.com)](https://developer.amap.com/api/javascript-api/guide/abc/components)

1. 地图的组成 
![[Pasted image 20240219145327.png]]
- 底图 Map ：所有信息的载体
- 图层 Layer：将不同的地理信息分类形成一个集合
-  要素 Feature ：表示不同的地物
-  几何 Geometry ：信息的数据模型和抽象
# 环境搭建

使用 npm 下载好安装包后，引入leaflets及样式文件即可使用

`npm install leaflet --save`

```vue
<script setup>
import { onMounted } from 'vue'
import "leaflet/dist/leaflet.css"
import L from "leaflet";

var mapLayer, map
onMounted(() => {
mapLayer = L.tileLayer("http://webrd0{s}.is.autonavi.com/appmaptile?lang=zh_cn&size=1&scale=1&style=8&x={x}&y={y}&z={z}", {
    attribution: "", //用来进行属性控制的字符串，描述了图层数据
    maxZoom: 18, //map 对象中的 maxZoom 属性是必须的，否则在创建 **makerClusterGroup ** 会报错
    minZoom: 3,
    subdomains: "1234", // 子域名，替换{s}
  })
  map = L.map('map', {
    center: [34.96, 108.19],
    zoomControl: false,
    zoom: 10,
  });
  mapLayer.addTo(map)
})
</script>
<template>
  <div id="map">
  </div>
</template>
<style lang="scss">
#map {
  height:600px;
  margin-top: 5px;
}
</style>
```
加载出一个平平无奇的地图
![[Pasted image 20240219144059.png]]

由于使用的是 “瓦片地图” ：缩放平移时会进行请求

> **请求瓦片**
> 当用户在地图上进行缩放或平移操作时，系统会根据当前的缩放级别和可视区域计算需要显示的瓦片，并通过瓦片的 URL 地址发送请求获取相应的瓦片图像



# 打点

## 打单个点

使用` L.Marker ` 方法
[Leaflet 1.7.1 中文文档 - 帮助手册 - 教程 - 《leaflet 中文文档 - 帮助手册 - 教程》 - 极客文档 (geekdaxue.co)](https://geekdaxue.co/read/leaflet-cn/v1.7.1-doc#marker)

```js
 const marker = L.marker([lat, lng], {
        icon: L.icon({
          iconSize: [18, 18], // 大小，单位px
          iconUrl: new URL('@/assets/images/myImg/gui.png', import.meta.url).href, // vue3 使用setup语法糖后不能使用require
        }),
      })
 marker.addTo(map) // 将标记添加到地图上
```

现在你就可以看到上出现标记了。
![[Pasted image 20240220104953.png]]
当然你也可以不自己写icon，会默认使用leaflet自己的icon

```JS
 const marker = L.marker([lat, lng])
 marker.addTo(map) 
```

## 多个打点

我这里的思路是，吧多个点放在同一图层上，要有聚集功能能（点多的话建议大家使用）

这里需要用到一个插件：`leaflet.markercluster`

[Vue-CLI and Leaflet （11）: 点聚合 Leaflet.markercluster - 掘金 (juejin.cn)](https://juejin.cn/post/6844903866102710279#heading-1)
[leaflet.markercluster中文文档|leaflet.markercluster js中文教程|解析 | npm中文文档 (npmdoc.org)](http://www.npmdoc.org/leaflet-markerclusterzhongwenwendangleaflet-markercluster-jszhongwenjiaochengjiexi.html)
[Leaflet.markercluster | Marker Clustering plugin for Leaflet](http://leaflet.github.io/Leaflet.markercluster/)

以上是我觉得不错的参考，下面我简单嗦一下怎么用

1. 下载
`npm i leaflet.markercluster `
2. 引用
```js
// 引入 leaflet.markercluster
import "leaflet.markercluster/dist/MarkerCluster.css";
import "leaflet.markercluster/dist/MarkerCluster.Default.css";
import "leaflet.markercluster";
```

开用！
```js
var stationsLayer = createClusterLayer()

function createClusterLayer () {
  const layer = L.markerClusterGroup({
    spiderfyOnMaxZoom: false,
    disableClusteringAtZoom: 15, //缩放到此级别，将不再聚集
  })
  return layer
}

function getStations (checked) {
// checked true-打点 false-移除打点
  if (checked) {
  // Test.stations 自己写的数组数据，[{},{}...] ，内含经纬度
    Test.stations.forEach(item => {
      const marker = L.marker([item.lat, item.lng], {
        icon: L.icon({
          iconSize: [18, 18],
          iconUrl: new URL('@/assets/images/myImg/gui.png', import.meta.url).href,
        }),
      })
      marker.addTo(stationsLayer)
    })
    stationsLayer.addTo(map)
  } else {
    map.removeLayer(stationsLayer)
    stationsLayer = createClusterLayer()
  }
}
```
![[Pasted image 20240220105505.png]]

非常丝滑

## TIPS


> [!question] 打点时是否调用gis服务？
> 答：可以不调用。但是缩放、平移地图时会调用，获取地图瓦片


![[Pasted image 20240220114012.png]]
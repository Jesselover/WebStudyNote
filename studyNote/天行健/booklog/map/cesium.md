# study

[运行环境搭建 | Cesium 入门教程 (syzdev.cn)](https://syzdev.cn/cesium-docs/guide/using-cesium.html)
我的cesium-token
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJiMGJlNDZjNS00ZTA2LTQwYzYtOTE3Zi1jMGU5NTdhNTQzNjQiLCJpZCI6MTk3NDE3LCJpYXQiOjE3MDg2ODE5MTd9.h6V6u8gvDonhcVkb8yq9lAfdwZ9Hj1ASiqDjtzkLyyc

支持数据格式：
	三维模型：GLTF，GLB
	三维瓦片： 3D Tile （倾斜摄影，人工模型，三维建筑物，CAD，BIM，点云数据等）


![[31bc35eae2b48c62d0c92d719338c3c.jpg]]

## cesium 中的坐标及转换

[Cesium开发入门篇 | 06坐标系及坐标变换 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/334540571)

cesium中的常用坐标
1. 屏幕坐标（像素）：二维笛卡尔平面坐标，可通过 new Cesium.Cartesian2(x,y) 创建
2. 笛卡尔空间直角坐标（世界坐标）：可通过 new Cesium.Cartesian(x,y,z)创建，主要用来做空间位置的变化，如平移、旋转、缩放等，他的坐标原点在椭球的中心
3. 地理坐标（弧度）：new Cesium.Cartographic(longitude,latitude,height)

cesium 中常用的坐标转化
1. 经纬度坐标转世界坐标
```js
// 方法1：直接转换 
// var cartesian3 = Cesium.Cartesian3.fromDegrees(lng, lat, height); 
// 方法2：借助ellipsoid对象，先转换成弧度再转换 
var cartographic = Cesium.Cartographic.fromDegrees(lng, lat, height); //单位：度，度，米 
var cartesian3 = ellipsoid.cartographicToCartesian(cartographic);
```

2. 世界坐标转化为经纬度
```JS
// 第一步：笛卡尔空间直角坐标转为地理坐标（弧度制） 
// var cartographic = Cesium.Cartographic.fromCartesian(cartesian3); 
// 方法1 
// var cartographic = ellipsoid.cartesianToCartographic(cartesian3); 
// 方法2 
// 第二步：地理坐标（弧度制）转化为u经纬度坐标
var lat = Cesium.Math.toDegrees(cartographic.latitude);
var lng = Cesium.Math.toDegrees(cartographic.longitude); 
var height = cartographic.height;
```

3. 弧度经纬度互转
```js
// 角度转弧度
	var radians = Cesium.Math.toRadians(90)
// 弧度转角度
	var degree = Cesium.Math.toDegrees(2 * Math.PI)
```

4. 屏幕坐标和世界坐标互转 

```js
// 二维屏幕坐标转为三维笛卡尔空间直角坐标（世界坐标） 
var cartesian3 = scene.globe.pick(    viewer.camera.getPickRay(windowPostion),scene)  

// 三维笛卡尔空间直角坐标（世界坐标）转为二维屏幕坐标 
// 结果是Cartesian2对象，取出X,Y即为屏幕坐标。  
windowPostion = Cesium.SceneTransforms.wgs84ToWindowCoordinates(scene, cartesian3);

```

只有转换到笛卡尔坐标系后才能运用计算机图形学中的仿射变换知识进行空间位置变换如平移旋转缩放。Cesium为我们提供了如下几种很有用的变换工具类：

- Cesium.Cartesian3（相当于Point3D）
- Cesium.Matrix3（3x3矩阵，用于描述**旋转**变换）
- Cesium.Matrix4（4x4矩阵，用于描述**旋转加平移**变换）
- Cesium.Quaternion（四元数，用于描述围绕某个向量**旋转一定角度**的变换）
- Cesium.Transforms(包含将位置转换为各种**参考系**的功能)
- Cesium.SceneTransforms

## cesium 加载影像数据

[Cesium开发基础篇 | 01加载影像数据 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/340669216)

影像图层类
- ImageryLayerCollection类：ImageryLayer类对象的容器，可以装在、放置多个ImageryLayer或者ImageryProvider类对象。
- ImageryLayer类：Cesium中的影响图层，相当于皮毛、衣服，将数据源包裹在内，相关属性：透明度、亮度等
- ImageryProvider类：抽象类，子类封装了加载各种影像图层的方法

> 虽然cesium将此类图层叫做imagery，但是并不特指卫星影像数据，还有互联网地图、TMS、WMS、WMTS、单个图片等

Cesium提供了以下14种ImageryProvider:

![[Pasted image 20240306115826.png]]

各类影响服务的加载
- ArcGIs 影像服务：通过ArcGIS server 或者ArCGIS online发布的地图切片服务



## cesium加载地形数据

地形图层类：
Cesium提供了TerrainProvider基类，该Provider负责每一个Tile对应的地形数据的构建，定义了一套地形Provider需要实现的接口和规范，但本身并不会参与其中的操作。基于此类cesium为我们封装了5个现成的继承类操作地形数据。Cesium中的地形类是直接通过不同的terrainProvider控制的，然后把某一个实例化terrainProvider赋值给Viewer.terrainProvider来控制地形数据的显隐。所以，Cesium中的地形图层只能有一个。

各类地形数据的加载:
 - Cesium默认地形-EllipsoidTerrainProvider光滑椭球体，没有任何地形起伏效果即高度为0，不支持水面、法线，但不从服务器请求数据
 - ArcGlS地形-ArcGISTiledElevationTerrainProvider名副其实的高度图高度不为0，原理和ElipsoidTerrainProvider如出一辙，同样不支持法线，水面;
 - Cesium标准地形-CesiumTerrainProvider两种格式:-种是高度图(目前Cesium已经废弃)，另一种则是TIN网格的STK地形支持水面和法线，同时数据量比较小。

## cesium 加载矢量数据

- 矢量数据(矢量数据)是用X、Y、Z坐标表示地图图形或地理实体位置的数据，一般是通过记录坐标的方式来尽可能将地理实体的空间位置表现的准确无误，常见的矢量数据有:点、线、面等格式。
- 我们使用矢量数据的原因，就是因为矢量数据具有数据结构紧凑、冗余度低、有利于网络和检索分析、图形显示质量好、精度高等优点。
- 目前最常见的矢量数据格式就是shapfile(简称shp)，它是由Esri公司开发的一种空间数据开放格式。同时shapfile也成为了地理信息系统(Gis)行业的标准，几乎所有的商业和开源地理信息系统软件都支持形状文件，通常情况下，一个"形状文件”通常指带有shp、.shx、.dbf和其他扩展名且前缀名称一致的文件(.prj、.sbn等)集合


**shp 文件包含的数据**
必含文件:
- 主文件(`*.shp`):存储地理要素的几何信息
- 索引文件(`*.shx`):存储要素几何图形的索引信息
- dBasc表文件(`*.dbf`):存储地理要素的属性信息(非几何信息)
可选的文件:
- 空间参考文件(`.prj`):存储空间参考信息，即地理坐标系统信息和投影坐标系统信息，使用well-knonn文本格式(WKT)进行描述。
- 几何体的空间索引文件(`*.sbn` 和`*.sbx`)、只读的Shapefiles的几何体的空间索引文件(`*.fbn` 和`*.fbx`)等等

**Cesium支持的矢量数据格式**
- geojson：直接使用Polygon、Point之类的几何体来表示图形;
- topojson： GeoJS0N的扩展形式，TopoJSON中的每一个几何体都是通过将共享边(被称为arcs)整合后组成的，文件大小缩小了 80%;
- kml: KML(keyhole markup language)是一种基于XML语法格式的文件，用来描述和存储地理信息数据(点、线、面、多边形、多面体以及模型等)，通常应用于 Goog1e地球相关软件中(Google Earth，Google Map 等)，它跟XML文件最大的不同就是KML描述的是地理信息数据，同时KML已正式被0GC采用，成为0GC众多规范中的一个。KML文件有两个文件扩展名: `*.KML `和  `*.KMZ`(一个或几个 KML 文件的压缩集，采用 zip 格式压缩)。
- 具有时间特性的czml
![[Pasted image 20240306144155.png]]

**在线工具推荐**
- JSON在线解析及格式化:https://wmw.json.Ge0Js
- 在线生成 GeoJS0N:http://geojson.io/
- shp数据转GeoS0N和TopoJSON:http://mapshaper.org/Geodson和TopopJson在线转换:http://jeffpaine.github.io/geojson-topojson/
- GeoJson和TopopJson在线转换:http://jeffpaine.github.io/geojson-topojson/



## cesium 加载gltf数据

- 两种格式 `*.glb` ,`*.gltf`
##  cesium 加载3dtiles 数据

 - 3D Tiles 是在gITF的基础上，加入了分层LOD的概念(可以把3D Tiles简单地理解为带有 LOD的 gITF )，专门为流式传输和渲染海量 3D 地理空间数据而设计的，例如倾斜摄影、3D 建筑、BIMI/CAD、实例化要素集和点云。它定义了一种数据分层结构和一组切片格式，用于渲染数据内容。

![[Pasted image 20240306151040.png]]

![[Pasted image 20240306152225.png]]

![[Pasted image 20240306153500.png]]


# cesium结合three

## 整合

我这里采用了整合 1 的方式

整合 1 ：
- Cesium 1.95：[Release CesiumJS 1.95 · CesiumGS/cesium (github.com)](https://github.com/CesiumGS/cesium/releases/tag/1.95)
- Three 143：[Release r143 · mrdoob/three.js (github.com)](https://github.com/mrdoob/three.js/releases/tag/r143)

[最新的Cesium和Three的整合方法 | syzdev](https://syzdev.cn/2022/07/31/%E6%9C%80%E6%96%B0%E7%9A%84Cesium%E5%92%8CThree%E7%9A%84%E6%95%B4%E5%90%88%E6%96%B9%E6%B3%95/)

整合2：
>.three 使用版本 r87 (过高版本会导致模型无法显示)

[CesiumJs结合ThreeJs（实测） - 掘金 (juejin.cn)](https://juejin.cn/post/6872213268950155277)
[Migration Guide · mrdoob/three.js Wiki (github.com)](https://github.com/mrdoob/three.js/wiki/Migration-Guide#r86--r87)
[Integrating Cesium with Three.js – Cesium](https://cesium.com/blog/2017/10/23/integrating-cesium-with-threejs/)

在这里建议大家锁死版本, 尤其是three，版本不对它就不显示模型了

```js
npm i three@0.143.0
npm i cesium@1.95
```
在package.json中：
```js
{
  "dependencies": {
    "cesium": "1.99",
    "three": "0.143.0",
  },
}
```

> 1. `"^3.x.x"`：锁定大版本，以后安装包的时候，保证包是 3.x.x 版本，x默认取最新的。
> 2. `"~3.1.x"`：锁定小版本，以后安装包的时候，保证包是 3.1.x 版本，x默认取最新的。
> 3. `"3.1.1"`：锁定完整版本，以后安装包的时候，保证包必须是 3.1.1 版本。


## 导入自定义模型

其实跟在three中导入自定义模型一样。但是 “整合1”  导入模型后，模型是黑色的，原因是没有灯光。“整合2” 开灯了，大家可以参考一下，当然也可以直接参考我的。

```js
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader"

function addModel () {
  let model
  const glftLoader = new GLTFLoader()
  glftLoader.load("/public/models/monkeyAndCube.glb", function (gltf) {
    gltf.scene.traverse((child) => {
      console.log(child.name)
      if (child.name === "monkey") { model = child }
    })
    model.scale.set(15000, 15000, 15000) // scale object to be visible at planet scale
    model.position.z += 7000.0 // translate "up" in Three.js space so the "bottom" of the mesh is the handle
    model.rotation.x = Math.PI / 2 // rotate mesh for Cesium's Y-up system
    let modelYup = new THREE.Group()
    three.scene.add(modelYup)
    modelYup.add(model)
    let spotLight = new THREE.SpotLight(0xffffff);
    spotLight.position.set(0, 0, 50000);
    spotLight.castShadow = true; //设置光源投射阴影
    spotLight.intensity = 5;
    modelYup.add(spotLight)
    //添加环境光
    var hemiLight = new THREE.HemisphereLight(0xff0000, 0xff0000, 5);
    modelYup.add(hemiLight);
    // Assign Three.js object mesh to our object array
    let _3DOB = new _3DObject()
    _3DOB.threeMesh = modelYup
    _3DOB.minWGS84 = minWGS84
    _3DOB.maxWGS84 = maxWGS84
    _3Dobjects.push(_3DOB)
  })
}

function init3DObject () {
  // Cesium entity
  addPolygonEntity()
  // addBoxEntity()
  // Three.js Objects
  // addBoxGeometry()
  addModel()
}
```

效果如图：
![[Pasted image 20240308114627.png]]


## 使用自定义地图并将模型放在指定经纬度

公司的想法是，开发一个“智慧园区”，侧重点是园区内车辆的监控，顺便实现红绿灯等效果，当然重点还是在车辆上。

市面上流行的技术选型有：ue5+cesium，unity+cesium，three+cesium（好像小众一点）
%% 由于姐们根本不打游戏，又没人带，所以果断选择较为简单的three %%

大体思路如下
1. 宏观场景：cesium展示3dtiles数据，加载厂区模型；three展示车辆 、红绿灯等需要动态效果的模型。采用粗略、小的模型，只进行最简单的交互。
2. 微观场景：three加载精细化模型，进行复杂的交互

可能遇到的问题：
1. three和cesium的融合问题：three图层在cesium图层上，因此three一定会挡住cesium
2. 卡顿

# 踩坑日记

在vue3 报错 `Element with id "cesiumContainer" does not exist in the document.` 
解决方法： 相关代码写在 `onMounted(()=>{})` 函数里
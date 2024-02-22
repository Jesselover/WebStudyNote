
**key**
web服务：f3149af18efa39ec282876317d3fcddb
JS-API：d0543d6e1c9f40e8272aa30af54e8ded

# init 

## 展示地图

[展示地图-入门教程-地图 JS API 2.0|高德地图API (amap.com)](https://lbs.amap.com/api/javascript-api-v2/tutorails/display-a-map)
[在vue3项目中使用新版高德地图_vue3使用高德地图-CSDN博客](https://blog.csdn.net/m0_58293192/article/details/128190221)

踩坑：`<AMap JSAPI> KEY异常，错误信息：USERKEY_PLAT_NOMATCH`
原因：申请的key和使用的服务不匹配，展示地图使用JS-API的key，地理信息解析是web服务

1. npm包安装
```
npm i @amap/amap-jsapi-loader --save
```
2. 引入
```js
import AMapLoader from '@amap/amap-jsapi-loader';
```
3. 初始化
```js
var AMap, map
window._AMapSecurityConfig = {
  securityJsCode: "d0543d6e1c9f40e8272aa30af54e8ded",
};
AMapLoader.load({
  key: "d0543d6e1c9f40e8272aa30af54e8ded", //申请好的Web端开发者key，调用 load 时必填
  version: "2.0", //指定要加载的 JS API 的版本，缺省时默认为 1.4.15
})
  .then((res) => {
    //JS API 加载完成后获取AMap对象
    AMap = res
    initMap()
  })
  .catch((e) => {
    console.error(e); //加载错误提示
  });
function initMap () {
  map = new AMap.Map("map", {
    viewMode: '2D', //默认使用 2D 模式
    zoom: 11, //地图级别
    center: [116.397428, 39.90923], //地图中心点,背景天安门为例
  });
}
```
![[Pasted image 20240221095238.png]]

此时，一个平平无奇的高德地图跃然纸上


# 结合THREE

[自定义图层-GLCustomLayer 结合 THREE-自有数据图层-示例中心-JS API 2.0 示例 | 高德地图API (amap.com)](https://lbs.amap.com/demo/javascript-api-v2/example/selflayer/glcustom-layer)

环境搭建：
1. 高德地图环境搭建看上一章
2. three环境搭建 ：**一定要下载对应版本的three**，在官网示例中可以查看其引入的three版本
```
npm i three@0.142 # 24/2/1日数据
```
## 入门小案例-引入外部模型

照搬官网案例就行

我这里做了些许改动
1. 引入外部模型猴头
2. 创建mesh，参考官网案例
3. 添加移动功能，将猴头移动入mesh中

效果如图
![[Pasted image 20240222161509.png]]
![[Pasted image 20240222161502.png]]

代码如下：

1. 引入
```JS
import AMapLoader from '@amap/amap-jsapi-loader'
import * as THREE from 'three';
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
import {reactive } from 'vue'
```
2. 地图准备
```js
// step map 
var AMap, map

window._AMapSecurityConfig = {
  securityJsCode: "d0543d6e1c9f40e8272aa30af54e8ded",
};
AMapLoader.load({
  key: "d0543d6e1c9f40e8272aa30af54e8ded", //申请好的Web端开发者key，调用 load 时必填
  version: "2.0", //指定要加载的 JS API 的版本，缺省时默认为 1.4.15
})
  .then((res) => {
    //JS API 加载完成后获取AMap对象
    AMap = res
    createMap() // 创建地图
    createThree() // 创建three
  })
  .catch((e) => {
    console.error(e); //加载错误提示
  });
function createMap () {
  map = new AMap.Map("map", {
    center: [116.54, 39.79],
    zooms: [2, 20],
    zoom: 14,
    viewMode: '3D',
    pitch: 50,
  });
}

```
3. three 准备
核心内容是创建GL图层，里面的 `render` 基本没什么变化
```js
// step three init
var camera, renderer, scene
var model, monkey, mesh
// 数据转换工具
var customCoords
// 测试用数据
var data
function createThree () {
  customCoords = map.customCoords;
  data = customCoords.lngLatsToCoords([
    [116.52, 39.79],
    [116.54, 39.79],
    [116.56, 39.79],
  ])
  // 创建 GL 图层
  var gllayer = new AMap.GLCustomLayer({
    // 图层的层级
    zIndex: 10,
    // 初始化的操作，创建图层过程中执行一次。
    init: (gl) => {
      initThree(gl)
    },
    render: () => {
      // 这里必须执行！！重新设置 three 的 gl 上下文状态。
      renderer.resetState();
      // 重新设置图层的渲染中心点，将模型等物体的渲染中心点重置
      // 否则和 LOCA 可视化等多个图层能力使用的时候会出现物体位置偏移的问题
      customCoords.setCenter([116.52, 39.79]);
      var { near, far, fov, up, lookAt, position } =
        customCoords.getCameraParams();

      // 这里的顺序不能颠倒，否则可能会出现绘制卡顿的效果。
      camera.near = near;
      camera.far = far;
      camera.fov = fov;
      camera.position.set(...position);
      camera.up.set(...up);
      camera.lookAt(...lookAt);
      camera.updateProjectionMatrix();

      renderer.render(scene, camera);

      // 这里必须执行！！重新设置 three 的 gl 上下文状态。
      renderer.resetState();
    },
  });
  map.add(gllayer)
  window.addEventListener('resize', onWindowResize);
}
function onWindowResize () {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
}
```

然后我们来看initThree
```JS
function initThree (gl) {
  // 这里我们的地图模式是 3D，所以创建一个透视相机，相机的参数初始化可以随意设置，因为在 render 函数中，每一帧都需要同步相机参数，因此这里变得不那么重要。
  // 如果你需要 2D 地图（viewMode: '2D'），那么你需要创建一个正交相机
  camera = new THREE.PerspectiveCamera(
    60,
    window.innerWidth / window.innerHeight,
    100,
    1 << 30
  );

  renderer = new THREE.WebGLRenderer({
    context: gl, // 地图的 gl 上下文
    // alpha: true,
    // antialias: true,
    // canvas: gl.canvas,
  });

  // 自动清空画布这里必须设置为 false，否则地图底图将无法显示
  renderer.autoClear = false;
  scene = new THREE.Scene();

  // 环境光照和平行光
  var aLight = new THREE.AmbientLight(0xffffff, 3);
  var dLight = new THREE.DirectionalLight(0xffffff, 10);
  dLight.position.set(1000, -100, 900);
  scene.add(dLight);
  scene.add(aLight);
  // 加载模型、mesh
  addModel()
  addMesh()
}
```
以上内容也基本不变，但最后加载模型、mesh按照你需要加载的物体变化。

加载外部模型
```JS
function addModel () {
  const glftLoader = new GLTFLoader()
  glftLoader.load("/public/models/monkeyAndCube.glb", function (gltf) {
    model = gltf.scene
    model.traverse((child) => {
      child.scale.set(500, 500, 500); // 放大模型
      child.rotation.x = 0.5 * Math.PI;
      child.position.z = 0.8;
      console.log(child.name)
      if (child.name === "monkey") { monkey = child }
    })
    monkey.position.set(data[0][0], data[0][1], 500); // 设置位置
    scene.add(monkey)
  })
}
```
加载mesh
```js
function addMesh () {
  //  这里可以使用 three 的各种材质
  var mat = new THREE.MeshLambertMaterial({
    side: THREE.DoubleSide,
    color: 0x1e2f97,
    transparent: true,
    opacity: .4,
    depthWrite: false
  })
  var geo = new THREE.BoxBufferGeometry(1200, 1200, 1200);
  const d = data[2];
  mesh = new THREE.Mesh(geo, mat);
  mesh.position.set(d[0], d[1], 500);
  scene.add(mesh);
  animate()
}
// 动画
function animate () {
  mesh.rotateZ((1 / 180) * Math.PI);
  map.render();
  requestAnimationFrame(animate);
}
```
移动猴头！ 记得自己给这个函数加个按钮
```js
function moveMonkey (checked) {
  console.log(checked, 'moveMonkey')
  if (checked) {
    monkey.position.set(data[2][0], data[2][1], 500);
  } else {
    monkey.position.set(data[0][0], data[0][1], 500);
  }
}

```
>[!TIP] 这里面地图和three好像是一起渲染的。如果你只加载了猴头，没加载mesh，此时是没有动画效果的，所以移动猴头的话，这个效果有延迟，缩放平移一下地图就好了。但是如果添加了动画，由于一直调用 `map.render()` 函数，因此不会出现此问题
>

大功告成！





[在高德地图中进行THREE开发-贴地呼吸点图层 - 掘金 (juejin.cn)](https://juejin.cn/post/7116407380480360485?searchId=20240220140314973F2D373FBAE67FCEB4)
[@vuemap/vue-amap高德vue组件库常用技巧（九）- threejs - 掘金 (juejin.cn)](https://juejin.cn/post/7308219624138178598)
[高德地图API 使用Threejs加载GLTF模型并设置模型朝向角 - 掘金 (juejin.cn)](https://juejin.cn/post/7143083085968441380?from=search-suggest)

[three.js 汽车行驶动画效果 - 0611163 - 博客园 (cnblogs.com)](https://www.cnblogs.com/s0611163/p/17879849.html)
[F4map Demo - Interactive 3D map](https://demo.f4map.com/#lat=55.7952387&lon=36.7900533&zoom=19)





>[!question] leaflet 如何与three结合？
>1. 高德地图开发了结合three使用的api，leaflet我仍在研究。但是，应该时能给实现的



## 结合tween.js

是不是觉得猴头的移动还不够顺滑，加个tween的动画试试

1. 引入
`npm install @tweenjs/tween.js`
```js
import * as TWEEN from '@tweenjs/tween.js'
```
2. 更改 `moveMonkey`函数
```js
function moveMonkey (checked) {
  console.log(checked, 'moveMonkey')
  if (checked) {
    // monkey.position.set(data[2][0], data[2][1], 500);
    const tween = new TWEEN.Tween(monkey.position)
      .to({ x: data[2][0], y: data[2][1], z: 500 }, 2000)
      .start()
  } else {
    // monkey.position.set(data[0][0], data[0][1], 500);
    const tween = new TWEEN.Tween(monkey.position)
      .to({ x: data[0][0], y: data[0][1], z: 500 }, 2000)
      .start()
  }
}
```
3. 在  `render` 函数中加入 ( 这里指`Three` 函数中的 创建GL图层的 `render` 函数)
```js
 TWEEN.update()
```
大功告成！Tween的其他功能也可以使用
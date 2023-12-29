
[three.js全网最全最新入门课程（2023年5月更新）【搞定前端前沿技术】_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Gg411X7FY/?spm_id_from=333.337.search-card.all.click)

[创建一个场景 – three.js docs (threejs.org)](https://threejs.org/docs/index.html#manual/zh/introduction/Creating-a-scene)

## summary

[Three.js中文网 (webgl3d.cn)](http://www.webgl3d.cn/)

### 初始化

介绍： 主要总结threeJs在vue中的使用

安装引入
```SHELL
npm install three
``````js
import * as THREE from 'three';
```

> 1. 最好确定版本号，以免three更新造成版本不兼容问题
> 2. 使用轨道控制器、加载器等时，还需要另行引入

#### 数据初始化

1. 全局变量准备
```js
      scene: null,
      camera: null,
      renderer: null,
      objects: null,
```

2. 容器准备
```js
    // 数据初始化
    initData() {
      this.container = this.$refs.three; //容器
    },
```

#### 创建场景

[Scene – three.js docs (threejs.org)](https://threejs.org/docs/index.html?q=scene#api/zh/scenes/Scene)
```js
   // 创建场景
    createScene() {
      this.scene = new THREE.Scene();
      this.scene.environment =  环境贴图 ; //若该值不为null，则该纹理贴图将会被设为场景中所有物理材质的环境贴图。
    },
```

#### 创建相机
 
- 透视相机 PerspectiveCamera：进大远小，模拟人眼
- 正交相机：视锥体就是一个立方体，无近大远小，永远渲染2d场景或者ui元素

设置合适的相机参数
```js
    // 创建相机
    createCamera() {
      this.camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,  // 一般设置为canvas画布宽高比
        0.9,
        100
      );
      // 相机位置
      this.camera.position.set(0, 2, 10);     
      // 观察目标点的坐标
      this.camera.lookAt(this.scene.position);
      this.scene.add(this.camera);
    },
```


#### 创建轨道控制器

Orbit controls（轨道控制器）可以使得相机围绕目标进行轨道运动。

> 轨道控制器会影响lookAt的设置，注意手动修改`control`
```js
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
```

```js
    // 创建控制器

    createOrbitControls() {

      this.controls = new OrbitControls(this.camera, this.renderer.domElement);

      this.controls.enableDamping = true; // 启用阻尼

      this.controls.minPolarAngle = 0.5; // 最小绕y轴角度

      this.controls.maxPolarAngle = 1.35; // 最大绕y轴角度

      this.controls.zoomSpeed = 0.3; // 放慢缩放速度

    },
```

```js
    // 加载

    render() {
      this.renderer.render(this.scene, this.camera);
      this.controls && this.controls.update(); //使用控制器后，必须在加载的时候update
      requestAnimationFrame(this.render);

    },
```

#### 创建灯光

- 环境光ambient：环境光会均匀的照亮场景中所有物体，环境光不能用来投射阴影，因为它没有方向。
- 平行光directionalLight ：类似于太阳光
- 点光源pointLight：类似于灯泡
- 聚光灯spotlight：类似于手电筒 

```js
  

    // 添加燈光

    createLight() {

      let light1 = new THREE.DirectionalLight(0xffffff, 1); // 创建一个方向光，参数为光的颜色和强度

      light1.position.set(0, 0, 10);

      this.scene.add(light1);

    },
```


#### 创建渲染器

```js
    // 创建渲染器

    createRenderer() {

      this.renderer = new THREE.WebGL1Renderer();

      this.renderer.setSize(

        this.container.clientWidth,

        this.container.clientHeight

      );
      this.renderer.antialisa = true; // 抗锯齿

      //   this.renderer.setClearColor("pink"); // 设置画面颜色

      this.container.appendChild(this.renderer.domElement);

    },
````
#### 创建网格地面

```js
    // 创建网格地面

    createGridHelper() {

      const gridHelper = new THREE.GridHelper(10, 10); // size divisions

      this.scene.add(gridHelper);

      //   gridHelper.material.transparent = true;

      //   gridHelper.material.opacity = 0.5;

    },
```


#### 创建坐标轴辅助器

```JS
    // 创建坐标轴辅助器

    createAxesHelper() {

      const axesHelper = new THREE.AxesHelper(5);

      this.scene.add(axesHelper);

    },
```


### 灯光与阴影

#### 阴影

1. 材质对光照有反应
2. 设置渲染器开启阴影计算
3. 设置光照投影阴影
4. 设置物体投影阴影
5. 设置物理接收阴影

```js
// 可以产生阴影的光：平行光、点光源
// 支持产生阴影的材质：标准网格材质、物理网格材质
renderer.shadowMap.enable = true
directionaLight.castShadow = true
sphere.castShadow = true
plane.receiveShadow = true
```


```js
// 阴影贴图模糊度
light1.shadow.radius = 20; 
// 阴影贴图分辨率
light1.shadow.mapSize.set(2048, 2048);
```


[OrthographicCamera – three.js docs (threejs.org)](https://threejs.org/docs/index.html?q=ca#api/zh/cameras/OrthographicCamera)
```js
directionaLight.shadow.camera.top = 5
directionaLight.shadow.camera.buttom = -5
directionaLight.shadow.camera.left = -5
directionaLight.shadow.camera.top = 5
directionaLight.shadow.camera.far = 500
directionaLight.shadow.camera.near = 0.5

// 更改相机属性后，必须进行更新矩阵
directionaLight.shadow.camera.updateProjectionMatrix()
```

#### 聚光灯
[SpotLight – three.js docs (threejs.org)](https://threejs.org/docs/index.html?q=spo#api/zh/lights/SpotLight)

```js
      light1.angle = Math.PI / 10;

      light1.distance = 40;

      // light1.target = this.cube;
      
      light1.penumbra = 0.5;

      //  this.renderer.physicalCorrectLights = true;
      light1.decay = 0.5;
```
####  点光源
### TWEEN
[tween.js 用户指南 | tween.js (tweenjs.github.io)](https://tweenjs.github.io/tween.js/docs/user_guide_zh-CN.html)

### 光线投射技术

[Three.js - Group 组合对象_group和object3d有什么区别_「已注销」的博客-CSDN博客](https://blog.csdn.net/ithanmang/article/details/80965712?spm=1001.2014.3001.5501)

### 载入模型

[载入3D模型 – three.js docs (threejs.org)](https://threejs.org/docs/index.html#manual/zh/introduction/Loading-3D-models)
[GLTFLoader – three.js docs (threejs.org)](https://threejs.org/docs/index.html#examples/zh/loaders/GLTFLoader)

```js
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
```

```js
   const loader = new GLTFLoader();

      loader.load(

        "path/to/model.glb",

        function (gltf) {

          scene.add(gltf.scene);

        },

        undefined,

        function (error) {

          console.error(error);

        }

      );
```

>[!warning]  加载地址写打包后的相对地址

否则可能加载不出来

>[!tip] blender 导出的部分纹理，threejs可能无法加载出

如：颜色渐变等 

> 载入模型后，记得根据模型调节相机参数
### Object3D

[Object3D – three.js docs (threejs.org)](https://threejs.org/docs/index.html#api/zh/core/Object3D)

#### 世界坐标与局部坐标

#####   局部坐标/世界坐标

世界坐标：自己的局部坐标position与所有父对象的局部坐标position的叠加
局部坐标/本地坐标：模型的position属性
```js
const worldPosition = new THREE.Vector3()
      this.gearshift.getWorldPosition(worldPosition)
```

>[!QUESTION] 改变模型相对局部坐标的原点位置？
>改变集合体顶点位置，可以改变模型自身相对坐标原点的位置

#### 常用属性
##### rotation 与 position

rotation属性和旋转方法rotateX()差异类似position属性和平移方法translateX()的差异，一个是相对坐标系设置角度、位置，一个是相对当前的三维模型的状态设置角度、位置参数。 旋转与平移参考的都是坐标系，不过参考的坐标系稍有不同，平移参考的是世界坐标系或者说三维场景对象Scene的坐标系，和相机对象一样，在整个三维场景中的位置， 三维模型的旋转参考的是模型坐标系，也就是对三维模型本身建立的坐标系。


##### visible 

控制模型的可见性，布尔值
才智material也有这个属性


##### material

外部材质共享材质问题
	1. 不要共享材质，独享材质
	2. material.clone(),返回一个新的材质，重新赋值给 material

PDR材质


#### 常用方法

##### object3d.traverse(function)

`object3d.traverse((child)=>{})` 递归查找

##### object3d.getObjectByName(string)


##### objecct3d.add(obj3D)

```js
obj.add( axesHelper );
obj.add( obj1 );
```

##### objecct3d.remove(obj3D)

### 粒子系统

[Three.js 进阶之旅：神奇的粒子系统-迷失太空 👨‍🚀 - 掘金 (juejin.cn)](https://juejin.cn/post/7155278132806123557)

#### 精灵Sprites

[Sprite – three.js docs (threejs.org)](https://threejs.org/docs/index.html?q=spri#api/zh/objects/Sprite)
[SpriteMaterial – three.js docs (threejs.org)](https://threejs.org/docs/index.html?q=spri#api/zh/materials/SpriteMaterial)

##### 概述

- 精灵是一个总是面朝着摄像机的平面，通常含有使用一个半透明的纹理。
- 每个对象需要分别由 `Three.js` 进行管理。%% => 大量创建粒子时会产生性能问题，建议使用Points %%

`THREE.SpriteMatrial` 对象的一些可修改属性及其说明。
	- `color`：粒子的颜色。
	- `map`：粒子所用的纹理，可以是一组 `sprite sheet`。
	- `sizeAttenuation`：如果该属性设置为 `false`，那么距离摄像机的远近不影响粒子的大小，默认值为 `true`。
	- `opacity`：该属性设置粒子的不透明度。默认值为 `1`，不透明。
	- `blending`：该属性指定渲染粒子时所用的融合模式。
	- `fog`：该属性决定粒子是否受场景中雾化效果影响。默认值为 `true`

##### code
```js
    // 精灵材质
    createSprites() {
      for (let x = -30; x < 30; x++) {
        for (let y = -20; y < 20; y++) {
          for (let z = 0; z < 5; z++) {
            const material = new THREE.SpriteMaterial({
              transparent: true,
              opacity: 0.5,
              color: 0xffffff * Math.random()
            })
            // 精灵材质
            const sprite = new THREE.Sprite(material)
            sprite.position.set(x * 4, y * 4, z * 100)
            this.scene.add(sprite)
          }
        }
      }
    },
```

####  点云Points

[PointsMaterial – three.js docs (threejs.org)](https://threejs.org/docs/index.html?q=Points#api/zh/materials/PointsMaterial)
[Points – three.js docs (threejs.org)](https://threejs.org/docs/index.html?q=Points#api/zh/objects/Points)

##### 概述 

一个用于显示点的类。 由[WebGLRenderer](https://threejs.org/docs/index.html#api/zh/renderers/WebGLRenderer "WebGLRenderer")渲染的点使用 [gl.POINTS](https://developer.mozilla.org/en-US/docs/Web/API/WebGLRenderingContext/drawElements)。

1. 创建粒子的网格 `THREE.BufferGeometry`
2. 创建粒子的材质 `THREE.PointsMaterial`
3. 创建两个数组 `veticsFloat32Array` 和 `veticsColors`，用来管理粒子系统中每个粒子的位置和颜色
4. 通过 `THREE.Float32BufferAttribute` 将它们设置为网格属性
5. 使用 `THREE.Points` 将创建的网格和材质变为粒子系统添加到场景中。

`THREE.PointsMaterial` 中所有可设置属性及其说明
	- `color`: 粒子系统中所有粒子的颜色。将 `vertexColors` 属性设置为 `true`，并且通过颜色属性指定了几何体的颜色来覆盖该属性。默认值为 `0xFFFFFF`。
	- `map`: 通过这个属性可以在粒子材质，比如可以使用 `canvas`、贴图等。
	- `size`：该属性指定粒子的大小，默认值为 `1`。
	- `sizeAnnutation`: 如果该属性设置为 `false`，那么所有的粒子都将拥有相同的尺寸，无论它们距离相机有多远。如果设置为 `true`，粒子的大小取决于其距离摄像机的距离的远近，默认值为`true`。
	- `vertexColors`：通常 `THREE.Points` 中所有的粒子都拥有相同的颜色，如果该属性设置为 `THREE.VertexColors`，并且几何体的颜色数组也有值，那就会使用颜色数组中的值，默认值为 `THREE.NoColors`。
	- `opacity`：该属性与 `transparent` 属性一起使用，用来设置粒子的不透明度。默认值为 `1`（完全不透明）。
	- `transparent`：如果该属性设置为 `true`，那么粒子在渲染时会根据 `opacity` 属性的值来确定其透明度，默认值为 `false`。
	- `blending`：该属性指定渲染粒子时的融合模式。
	- `fog`：该属性决定粒子是否受场景中雾化效果影响，默认值为 `true`。

##### code
```js
    // 点云
    createPoints() {
      const geom = new THREE.BufferGeometry()
      const material = new THREE.PointsMaterial({
        size: 2,
        vertexColors: true, //是否使用顶点着色,此引擎支持RGB或者RGBA两种顶点颜色，取决于缓冲 attribute 使用的是三分量（RGB）还是四分量（RGBA）。
        color: 0xffffff,
        // map: // 添加纹理
      })
      const positions = []
      const colors = []
      for (let x = -30; x < 30; x++) {
        for (let y = -20; y < 20; y++) {
          for (let z = 0; z < 50; z++) {
            positions.push(x * 4, y * 4, z * 10)
            const clr = new THREE.Color(Math.random() * 0xffffff)
            colors.push(clr.r, clr.g, clr.b)
          }
        }
      }
      geom.setAttribute('position', new THREE.Float32BufferAttribute(positions, 3))
      geom.setAttribute('color', new THREE.Float32BufferAttribute(colors, 3))
      const cloud = new THREE.Points(geom, material)
      this.scene.add(cloud)
    },
```
##### example

###### 下雨效果

```JS
// 雨滴效果
    rain() {
      const geom = new THREE.BufferGeometry()
      const material = new THREE.PointsMaterial({
        size: 2,
        vertexColors: true,
        transparent: true,
        opacity: 0.6,
        blending: THREE.AdditiveBlending, // 混合模式：在画新像素时，背景像素的颜色会被添加到新像素上
        setAttenuation: true, // 每个雨滴粒子是否远小近大
        color: 0xffffff,
        //  index.html相对于目标资源的路径
        map: preThree.getTexture('./static/images/rain.png'), // 添加纹理
        depthWrite:false,
      })
      const positions = []
      const colors = []
      const range = 200
      const velocities = [] // 雨滴下落
      for (let x = 0; x < 10000; x++) {
        positions.push(Math.random() * range - range / 2,
          Math.random() * range - range / 2,
          Math.random() * range - range / 2)
          // vertexColors: true 时，显示此效果
        velocities.push((Math.random() - 0.5) / 3, Math.random() / 5 + 0.1)
        const color = new THREE.Color(0x00eeee)
        const asHSL = {}
        // 明暗度变换
        color.getHSL(asHSL)
        color.setHSL(asHSL.h, asHSL.s, asHSL.l * Math.random())
        colors.push(color.r, color.g, color.b)
      }
      geom.setAttribute('position', new THREE.Float32BufferAttribute(positions, 3))
      geom.setAttribute('color', new THREE.Float32BufferAttribute(colors, 3))
      // 自定义属性
      geom.setAttribute('velocity', new THREE.Float32BufferAttribute(velocities, 2))
      this.cloud = new THREE.Points(geom, material)
      this.cloud.name = 'cloud'
      this.scene.add(this.cloud)
      this.render()
    }
```

```JS
   render() {
      // 下雨动画
      const pos_BufferAttr = this.cloud.geometry.getAttribute('position')
      const vel_BufferAttr = this.cloud.geometry.getAttribute('velocity')
      for (let i = 0; i < pos_BufferAttr.count; i++) {
        let pos_x = pos_BufferAttr.getX(i)
        let pos_y = pos_BufferAttr.getY(i)
        let vel_x = vel_BufferAttr.getX(i)
        let vel_y = vel_BufferAttr.getY(i)
        pos_x = pos_x - vel_x
        pos_y = pos_y - vel_y
        // 边界矫正
        if (pos_x <= -20 || pos_x > 20) vel_x = vel_x * -1
        if (pos_y <= 0) pos_y = 60
        pos_BufferAttr.setX(i, pos_x)
        pos_BufferAttr.setY(i, pos_y)
        vel_BufferAttr.setX(i, vel_x
      }
      pos_BufferAttr.needsUpdate = true
      vel_BufferAttr.needsUpdate = true
      // other code ...
    },
```

### 后处理

### tools

[Three.js - dat.GUI库的使用详解 - 615 - 博客园 (cnblogs.com)](https://www.cnblogs.com/wuqun/p/14366057.html)

#### OutlinePass.js 高亮发光描边
![[Pasted image 20230816165030.png]]

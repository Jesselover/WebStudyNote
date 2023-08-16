
[three.js全网最全最新入门课程（2023年5月更新）【搞定前端前沿技术】_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Gg411X7FY/?spm_id_from=333.337.search-card.all.click)

[创建一个场景 – three.js docs (threejs.org)](https://threejs.org/docs/index.html#manual/zh/introduction/Creating-a-scene)


## study
###  本地搭建three.js 官网，用parcel搭建three.js开发环境

下载后
`npm install`
`npm run serve`

[🚀 快速开始 | Parcel中文网 (parceljs.cn)](https://www.parceljs.cn/getting_started.html)
[package.json 配置完全解读 - 掘金 (juejin.cn)](https://juejin.cn/post/7145759868010364959)
一个包全局安装后，另一个项目需要用时，还需要在安装一次

1. 创建文件夹，用VScode打开
	`npm init`
	`npm install -g parcel-bundler`

2. 创建src文件，里面添加index.html文件,引入相应文件 

>[!tip] 
> 引入three.js脚本文件
> `<script src="./main/main.js" type="module"></script>`


```html
<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Document</title>

    <!-- 引入样式 -->

    <link rel="stylesheet" href="./assets/css/style.css">

</head>

<body>

    <!-- 模块化开发，设置类型为module；type (en-US)

该属性表示所代表的脚本类型 -->

   <script src="./main/main.js" type="module"></script>

</body>

</html>
```

3. package.json 文件内, 配置入口文件
```json
  "scripts": {

    "dev": "parcel src/index.html",

    "build": "parcel build src/index.html"

  },
```

4. `npm install parcel-bundler --save-dev`
5. `npm install three`  
6. `src/main/main.js` 中 `import * as THREE from "three"`

###  使用three.js渲染场景和物体

```js
// 引入three.js

import * as THREE from "three"

// 1. 创建场景

const scene = new THREE.Scene()

// 2.创建相机(PerspectiveCamera透视相机)

const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.9, 100)

// 设置相机位置

camera.position.set(0,0,10) //x,y,z

scene.add(camera)

  

// 3.添加物体

// 创建几何体

const cubeGeometry = new THREE.BoxGeometry()

const cubeMaterial = new THREE.MeshBasicMaterial({color:0xffff00})

// 根据几何体、材质创建物体

const cube = new THREE.Mesh(cubeGeometry,cubeMaterial)

// 将几何体添加到场景

scene.add(cube)

  

// 4.初始化渲染器

const renderer = new THREE.WebGL1Renderer()

// 设置渲染尺寸

renderer.setSize(window.innerWidth,window.innerHeight)

 

// 5.将webGL渲染的canvas内容添加到body

document.body.appendChild(renderer.domElement)

  

// 6.使用渲染器，通过相机将场景进行渲染

renderer.render(scene,camera)
```

### 轨道控制器OrbitControls查看物体

[OrbitControls – three.js docs (threejs.org)](https://threejs.org/docs/index.html?q=OrbitControls#examples/zh/controls/OrbitControls)

```js
// !轨道控制器查看物体

import * as THREE from "three"

// 导入轨道控制器

import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'

  

const scene = new THREE.Scene()

  

const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 100)

camera.position.set(0,0,10) //x,y,z

scene.add(camera)

  

const cubeGeometry = new THREE.BoxGeometry()

const cubeMaterial = new THREE.MeshBasicMaterial({color:0xffff00})

const cube = new THREE.Mesh(cubeGeometry,cubeMaterial)

scene.add(cube)

  

const renderer = new THREE.WebGL1Renderer()

renderer.setSize(window.innerWidth,window.innerHeight)

  

document.body.appendChild(renderer.domElement)

  

// renderer.render(scene,camera)

  

// 创建轨道控制器

const controls = new OrbitControls(camera,renderer.domElement)

  

function render(){

    renderer.render(scene,camera)

    // 渲染下一帧的时候就会调用render函数

    requestAnimationFrame(render)

}

  

// 初始化渲染

render()
```


### 使用坐标轴

```js
// !添加坐标辅助器

import * as THREE from "three"

import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'

  

const scene = new THREE.Scene()

  

const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 100)

camera.position.set(0,0,10) //x,y,z

scene.add(camera)

  

const cubeGeometry = new THREE.BoxGeometry()

const cubeMaterial = new THREE.MeshBasicMaterial({color:0xffff00})

const cube = new THREE.Mesh(cubeGeometry,cubeMaterial)

scene.add(cube)

  

const renderer = new THREE.WebGL1Renderer()

renderer.setSize(window.innerWidth,window.innerHeight)

  

document.body.appendChild(renderer.domElement)

  

const controls = new OrbitControls(camera,renderer.domElement)

  

// 添加坐标轴辅助器

const axesHelper = new THREE.AxesHelper(5)

scene.add(axesHelper)

  

function render(){

    renderer.render(scene,camera)

    requestAnimationFrame(render)

}

  

render()
```


### 设置物体移动、縮放、旋转

[Object3D – three.js docs (threejs.org)](https://threejs.org/docs/index.html?q=Mesh#api/zh/core/Object3D)

> postion是相对于父元素的位置

 postion https://threejs.org/docs/index.html?q=vector3#api/zh/math/Vector3

```js
// !设置物体移动、縮放、旋转

import * as THREE from "three"

import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'

  

const scene = new THREE.Scene()

  

const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 100)

camera.position.set(0, 0, 10) //x,y,z

scene.add(camera)

  

const cubeGeometry = new THREE.BoxGeometry()

const cubeMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 })

const cube = new THREE.Mesh(cubeGeometry, cubeMaterial)

scene.add(cube)

  

const renderer = new THREE.WebGL1Renderer()

renderer.setSize(window.innerWidth, window.innerHeight)

  

document.body.appendChild(renderer.domElement)

  

const controls = new OrbitControls(camera, renderer.domElement)

  

const axesHelper = new THREE.AxesHelper(5)

scene.add(axesHelper)

  

function render() {

    // 移动物体

    cube.position.x += 0.01

    cube.rotation.x += 0.01

    if (cube.position.x > 5) {

        cube.position.x = 0

    }

    // 缩放物体

    cube.scale.set(3, 1, 5)

    // 旋转物体

    // cube.rotation.set(Math.PI / 4, 0, Math.PI / 4)

    renderer.render(scene, camera)

    requestAnimationFrame(render)

}

  

render()
```

### 应用requestAnimationFrame、clock跟踪时间处理动画

[AnimationAction – three.js docs (threejs.org)](https://threejs.org/docs/index.html?q=Animation#api/zh/animation/AnimationAction)
[Clock – three.js docs (threejs.org)](https://threejs.org/docs/index.html?q=clock#api/zh/core/Clock)

```js
// !应用requestAnimationFrame、clock跟踪时间处理动画

import * as THREE from "three"

import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'

  

const scene = new THREE.Scene()

  

const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 100)

camera.position.set(0, 0, 10) //x,y,z

scene.add(camera)

  

const cubeGeometry = new THREE.BoxGeometry()

const cubeMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 })

const cube = new THREE.Mesh(cubeGeometry, cubeMaterial)

scene.add(cube)

  

const renderer = new THREE.WebGL1Renderer()

renderer.setSize(window.innerWidth, window.innerHeight)

  

document.body.appendChild(renderer.domElement)

  

const controls = new OrbitControls(camera, renderer.domElement)

  

const axesHelper = new THREE.AxesHelper(5)

scene.add(axesHelper)

  

// 设置时钟

const clock = new THREE.Clock()

  

function render(time) {

    // 使用requestAnimationFrame时间参数控制

    // let t = time / 1000 % 5

  

    // 获取始终运行总时长

    let time = clock.getElapsedTime()

    let daltaTime = clock.getDelta()

    console.log(time);

    // cube.position.x = t

    cube.rotation.x += 0.01

    if (cube.position.x > 5) {

        cube.position.x = 0

    }

    cube.scale.set(3, 1, 5)

    renderer.render(scene, camera)

    requestAnimationFrame(render)

}

  

render()
```

### Gsap动画库

[gsap - npm (npmjs.com)](https://www.npmjs.com/package/gsap)
[GSAP3入门 - 掘金 (juejin.cn)](https://juejin.cn/post/7041862990622605349)

`npm install gsap`
`import gsap from "gsap"`

```js
// ! Gsap动画

import * as THREE from "three"

import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'

// 导入Gsap动画库

import gsap from "gsap"

const scene = new THREE.Scene()

  

const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 100)

camera.position.set(0, 0, 10) //x,y,z

scene.add(camera)

  

const cubeGeometry = new THREE.BoxGeometry()

const cubeMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 })

const cube = new THREE.Mesh(cubeGeometry, cubeMaterial)

scene.add(cube)

  

const renderer = new THREE.WebGL1Renderer()

renderer.setSize(window.innerWidth, window.innerHeight)

  

document.body.appendChild(renderer.domElement)

  

const controls = new OrbitControls(camera, renderer.domElement)

  

const axesHelper = new THREE.AxesHelper(5)

scene.add(axesHelper)

// 设置动画

var animate1 = gsap.to(cube.position, {

    x: 5,

    duration: 5,

    ease: 'power1.in',

    onComplete: () => {

        console.log("结束");

    },

    onStart: () => {

        console.log("开始");

    },

    //! 无限次循环-1

    repeat: -1,

    // 往返运动

    yoyo: true,

    delay: 2,

})

gsap.to(cube.rotation, { x: 2 * Math.PI, duration: 5, repeat: -1, })

  

// 鼠标双击，不再移动

window.addEventListener("dblclick", () => {

    if (animate1.isActive())

        // 暂停

        animate1.pause()

    else

        // 恢复

        animate1.resume()

})

  

function render(time) {

    cube.scale.set(1, 1, 1)

    renderer.render(scene, camera)

    requestAnimationFrame(render)

}

  

render()
```

### 根据尺寸变化实现自适应画面

```js
// ! 根据尺寸变化实现自适应画面

import * as THREE from "three"

import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'

import gsap from "gsap"

const scene = new THREE.Scene()

  

const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 100)

camera.position.set(0, 0, 10) //x,y,z

scene.add(camera)

  

const cubeGeometry = new THREE.BoxGeometry()

const cubeMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 })

const cube = new THREE.Mesh(cubeGeometry, cubeMaterial)

scene.add(cube)

cube.scale.set(1, 1, 1)

  

const renderer = new THREE.WebGL1Renderer()

renderer.setSize(window.innerWidth, window.innerHeight)

  

document.body.appendChild(renderer.domElement)

  

const controls = new OrbitControls(camera, renderer.domElement)

// 设置控制器阻尼，让控制器更有真实感觉

// 将其设置为true以启用阻尼（惯性），这将给控制器带来重量感。默认值为false。

// !请注意，如果该值被启用，你将必须在你的动画循环里调用.update()。

controls.enableDamping = true

  

const axesHelper = new THREE.AxesHelper(5)

scene.add(axesHelper)

  

var animate1 = gsap.to(cube.position, {

    x: 5,

    duration: 5,

    ease: 'power1.in',

    onComplete: () => {

        console.log("结束");

    },

    onStart: () => {

        console.log("开始");

    },

    repeat: -1,

    yoyo: true,

    delay: 2,

})

gsap.to(cube.rotation, { x: 2 * Math.PI, duration: 5, repeat: -1, })

  

window.addEventListener("dblclick", () => {

    if (animate1.isActive())

        animate1.pause()

    else

        animate1.resume()

})

  

// 动画循环

function render(time) {

    // !请注意，如果controls.enableDamping被启用，你将必须在你的动画循环里调用.update()。

    controls.update()

    renderer.render(scene, camera)

    requestAnimationFrame(render)

}

  

render()

  

// !监听画面变化，更新渲染画面（根据尺寸变化实现自适应画面）

window.addEventListener('resize', () => {

    // 更新摄像头

    camera.aspect = window.innerWidth / innerHeight

    // 更新摄像头投影矩阵

    camera.updateProjectionMatrix();

    // 更新渲染器

    renderer.setSize(window.innerWidth,window.innerHeight)

    // 设置渲染器像素比,为设备像素比

    renderer.setPixelRatio(window.devicePixelRatio)

})
```



### 材质

> 1. 有的材质需要灯光才能看见
> 2. 图片资源默认在public静态资源下。如果要放在src中，要require
> 3. texture.colorSpace = THREE.SRGBColorSpace 更符合人眼感知。默认是线性。
> 4. blender中的部分材质，在threejs中无法渲染，建议选择简单材质



#### 贴图

ao贴图：环境遮挡贴图
alph map：透明度贴图
[Threejs02 - 贴图、阴影和灯光 - 掘金 (juejin.cn)](https://juejin.cn/post/7240268161538719805?searchId=2023081014283437D458565F8479F38D83)

####  雾 
scene.fog

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



## summary

[Three.js中文网 (webgl3d.cn)](http://www.webgl3d.cn/)

### 初始化

介绍： 主要总结threeJs在vue中的使用

安装引入
```SHELL
npm install three
```

```js
import * as THREE from 'three';
```

> 使用轨道控制器、加载器等时，还需要重新引入

#### 数据初始化

1. 全局变量准备
```js
      scene: null,

      camera: null,

      renderer: null,

      controls: null,

      container: null,
```

2. 容器准备
```js
    // 数据初始化

    initData() {

      this.container = this.$refs.three; //容器

    },
```

#### 创建场景

```js
   // 创建场景

    createScene() {

      this.scene = new THREE.Scene();

      this.scene.background = new THREE.Color("#ccc"); // 设置场景背景颜色

      //   ? 环境光

      this.scene.environment = new THREE.Color("green"); // 设置环境光颜色

    },
```

> 1. 场景背景颜色不设置，默认为黑色，在没开灯的情况下，某些材质也是黑色，可能会导致模型加载出来了但是看不见

>[!QUESTION] 环境光颜色？

#### 创建相机？？

```js
    // 创建相机

    createCamera() {

      this.camera = new THREE.PerspectiveCamera(

        75,

        window.innerWidth / window.innerHeight,

        0.9,

        100

      );

      this.camera.position.set(0, 2, 10);

      //   this.camera.lookAt(this.scene.position);

      //   this.scene.add(this.camera);

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

      //   this.renderer.setClearColor("pink"); // 设置画面颜色

      this.container.appendChild(this.renderer.domElement);

    },
```

>[!question] 设置画面颜色？

#### 加载

```js
    // 加载

    render() {

      this.renderer.render(this.scene, this.camera);

      this.controls && this.controls.update(); //使用控制器后，必须在加载的时候update

      requestAnimationFrame(this.render);

    },
```
#### 创建网格地面

```js
    // 创建网格地面

    createGridHelper() {

      const gridHelper = new THREE.GridHelper(10, 10);

      this.scene.add(gridHelper);

      //   gridHelper.material.transparent = true;

      //   gridHelper.material.opacity = 0.5;

    },
```

#### 创建控制器

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

#### 创建坐标轴辅助器

```JS
    // 创建坐标轴辅助器

    createAxesHelper() {

      const axesHelper = new THREE.AxesHelper(5);

      this.scene.add(axesHelper);

    },
```

#### 创建灯光

```js
  

    // 添加燈光

    createLight() {

      let light1 = new THREE.DirectionalLight(0xffffff, 1); // 创建一个方向光，参数为光的颜色和强度

      light1.position.set(0, 0, 10);

      this.scene.add(light1);

    },
```


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





### Object3D

[Object3D – three.js docs (threejs.org)](https://threejs.org/docs/index.html#api/zh/core/Object3D)

rotation属性和旋转方法rotateX()差异类似position属性和平移方法translateX()的差异，一个是相对坐标系设置角度、位置，一个是相对当前的三维模型的状态设置角度、位置参数。 旋转与平移参考的都是坐标系，不过参考的坐标系稍有不同，平移参考的是世界坐标系或者说三维场景对象Scene的坐标系，和相机对象一样，在整个三维场景中的位置， 三维模型的旋转参考的是模型坐标系，也就是对三维模型本身建立的坐标系。

## application

[threejs+vue/01 glb模型导入，改变汽车车身颜色 - 掘金 (juejin.cn)](https://juejin.cn/post/7122000200851259429)

[Three.js实现汽车3D展示/开关门/变色/运动/视角切换/波动热点/汽车模型_threejs 车轮模型车灯打开的效果_左本Web3D的博客-CSDN博客](https://blog.csdn.net/baidu_29701003/article/details/125334202)


[Three.js三维模型几何体旋转、缩放和平移_three 旋转_Naive》的博客-CSDN博客](https://blog.csdn.net/qq_34568700/article/details/117703695)

[threejs+tweenjs实现3d场景动画 - 掘金 (juejin.cn)](https://juejin.cn/post/7028780379649605646#heading-3)

[three.js聚光灯SpotLight使用，调整聚光灯颜色、位置、角度、强度、距离、衰减指数、方向、可见性、是否产生阴影属性(vue中使用three.js09)_threejs spotlight_点燃火柴的博客-CSDN博客](https://blog.csdn.net/qw8704149/article/details/108541970)
###### target
1. 轮子的转动
2. 发动机转速
3. 车是否启动 
4. 灯光、转向灯 
5. 车门
	开关门交互：3D建模的时候，保证每个车门是一个Group，在3D软件中，让本地坐标轴和车门旋转轴重合，然后代码根据车门节点名称，获取车group，然后最后tweenjs生成开关门动画
6. 尾气
7. 档位
8. 车窗


1，2，5 group+rotation
3 material+color
4 盒子+点光源
8 postion + tween

注意：需要操作的对象，单个成为一个完整的组或者mesh。其位置的

![[Pasted image 20230811163807.png]]
设置发光材质
![[Pasted image 20230811163947.png]]

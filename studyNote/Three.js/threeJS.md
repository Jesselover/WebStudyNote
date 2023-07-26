
[three.js全网最全最新入门课程（2023年5月更新）【搞定前端前沿技术】_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Gg411X7FY/?spm_id_from=333.337.search-card.all.click)

[创建一个场景 – three.js docs (threejs.org)](https://threejs.org/docs/index.html#manual/zh/introduction/Creating-a-scene)



### 1. 本地搭建three.js 官网

下载后
`npm install`
`npm run serve`

### 2. 用parcel搭建three.js开发环境

[🚀 快速开始 | Parcel中文网 (parceljs.cn)](https://www.parceljs.cn/getting_started.html)

1. 创建文件夹，用VScode打开
	`npm init`
	`npm install -g parcel-bundler`

2. 创建src文件，里面添加index.html文件,引入相应文
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

3. package.json 文件内
```json
  "scripts": {

    "dev": "parcel src/index.html",

    "build": "parcel build src/index.html"

  },
```

4. `npm install parcel-bundler --save-dev`
5. `npm install three`  `src/main/main.js` 中 `import * as THREE from "three"`

### 3. 使用three.js渲染场景和物体

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

### 4. 轨道控制器查看物体
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


### 5.使用坐标轴

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

### 6.设置物体移动、縮放、旋转

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
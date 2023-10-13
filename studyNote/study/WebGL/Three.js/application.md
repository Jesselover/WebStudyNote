## application 1

[threejs+vue/01 glb模型导入，改变汽车车身颜色 - 掘金 (juejin.cn)](https://juejin.cn/post/7122000200851259429)

[Three.js实现汽车3D展示/开关门/变色/运动/视角切换/波动热点/汽车模型_threejs 车轮模型车灯打开的效果_左本Web3D的博客-CSDN博客](https://blog.csdn.net/baidu_29701003/article/details/125334202)


[Three.js三维模型几何体旋转、缩放和平移_three 旋转_Naive》的博客-CSDN博客](https://blog.csdn.net/qq_34568700/article/details/117703695)

[threejs+tweenjs实现3d场景动画 - 掘金 (juejin.cn)](https://juejin.cn/post/7028780379649605646#heading-3)

[three.js聚光灯SpotLight使用，调整聚光灯颜色、位置、角度、强度、距离、衰减指数、方向、可见性、是否产生阴影属性(vue中使用three.js09)_threejs spotlight_点燃火柴的博客-CSDN博客](https://blog.csdn.net/qw8704149/article/details/108541970)

###### target

- [x] 轮子的转动 number
- [x] 踏板
- [x] 车是否启动  Boolean
- [ ]  灯光、转向灯 
- [x] 车门
- [x] 锁车门
- [x] 挡位
- [x] 车窗

###### DATA

```JS
    start: {
        type: "start",
        status: Boolean //true 开启 false 关闭
    },
    velocity: {
        type: "velocity",
        velocity: Number,
    },
    window: {
        type: "window",
        windiow: "left" || "right" || "all", //string
        status: Boolean//true 开启 false 关闭
    },
    door: {
        type: "door",
        door: "left" || "right" || "all", //string
        status: Boolean//true 开启 false 关闭
    },
    lock: {
        type: 'lock',
        status: Boolean
    },
    gear: {
        type: "gear",
        gear: "P" || "N" || "R" || "D",
    },
    pedal: {
        type: "pedal",
        pedal: "accelerator" || "brake"
    light: {
        type: "light",
        signal: "far" || "close" || "leftTurn" || "rightTurn", //远光，近光，转向string
        status: Boolean//true 开启 false 关闭
        }
```


### 车门开启、关闭


使用光线投射技术，点击车门开启、关闭车门
```JS
    onMouseClick(event) {

      this.raycaster = new THREE.Raycaster();

      this.mouse = new THREE.Vector2();

      //通过鼠标点击的位置计算出raycaster所需要的点的位置，以屏幕中心为原点，值的范围为-1到1.

      this.mouse.x = (event.clientX / window.innerWidth) * 2 - 1;

      this.mouse.y = - (event.clientY / window.innerHeight) * 2 + 1;

      this.raycaster.setFromCamera(this.mouse, this.camera);

      const intersectsRight = this.raycaster.intersectObjects(this.rightDoor.children);

      const intersectsLeft = this.raycaster.intersectObjects(this.leftDoor.children);

      if (intersectsRight.length) {

        this.door("right")

      }

      if(intersectsLeft.length){

        this.door("left")

      }

      this.renderer.render( this.scene, this.camera );

    },
```

在mounted中绑定点击事件

```JS
  mounted() {

    window.addEventListener('click', this.onMouseClick, false)

  },
```

### 挡位

1、P档：驻车档位。在挂入该档位时，停车锁止机构将变速器输出轴锁止，P表示“Parking”停车，在车子停放或完全静止时采用。

2、R档：倒车档位。挂入该档位时，接通液压系统倒档油路，使驱动轮反转，进行倒档行驶。当车辆未完全停稳时，不得强行转至“R”挡，否则将导致变速器损坏。

3、N档：空档。在挂入空档时，行星齿轮系统空转，不能输出动力。

4、D档：前进档位。当换档操纵手柄处于该档位时，液压系统控制装置就会根据节气门开度信号与车速信号自动接通相应的前进档油路，可随着行驶速度的变化自动升降挡，实现自动变速功能。
### 启动时发动机闪烁

>[!question] 改变一个物体材料，其他的也跟着变？
>
>threejs中的网格物体对材质的是引用传递，不是值传递，如果material1 被 mesh1和mesh2用到了，改变 mesh1.material.color，则mesh2的材质颜色也改了。解决方法：赋予一个新的材质




### detail

#### 添加音效

```js
// 导入音频
import openMusic from '../../../public/static/music/Open.mp3'
```

```js
      const audio = new Audio()

      audio.src = openMusic

      audio.play()
```

[vue播放音频的两种方法（audio标签和audiocontext方法）_偏振万花筒的博客-CSDN博客](https://blog.csdn.net/weixin_44325637/article/details/89248110)

[Uncaught (in promise) DOMException: Failed to load because no supported source was found._不完美女孩-的博客-CSDN博客](https://blog.csdn.net/LJJONESEED/article/details/123838313)


### updata
[threejs单击选中模型高亮显示/选中模型发光_threejs模型轮廓发光_冉冉胜起的博客-CSDN博客](https://blog.csdn.net/qq_15023917/article/details/114366480)



[一次Three.js加载obj模型引出的，点击改变模型颜色的问题_人中伊布的博客-CSDN博客](https://blog.csdn.net/darkproc/article/details/80015901)

模型需求：
1. 车窗颜色加深（看到开关车窗效果）
2. 发动机名字（要更改发动机材料属性）
3. 挡位相关问题
4. 车外壳整理，可以一件改变车颜色（先不弄这个，等做完其他功能有时间再说）、

## application 2

![[Pasted image 20231009103105.png]]

1. 线上车辆销售展示
[全景看车-思域 (autohome.com.cn)](https://pano.autohome.com.cn/car/ext/25893?appversion=)
[Oreo Li - Homepage](https://oreo.ink/)
[BMW i8 - PLAYCANVAS](https://playcanv.as/p/RqJJ9oU9)
[全景VR看车是如何实现的？虚拟建模还是实景拍摄 - 腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/news/667750)
建模 + 实景拍摄全景照片 + WebGL

对于车辆外观，采用3d建模技术，实现变色、开关车门车窗、启动等交互效果。并提供实景图片和视频辅佐我们验证我们所看到的真实性。
对于车辆内饰，采用实景拍摄，增加真实感，减少资金、时间成本。

2. 故障诊断定位、真实车辆模拟展示
预测车辆可能出问题的部件看，在3D模型中高亮展示，方便定位。
使用3D模型，模拟真实车辆行为，如：开关车门车窗、启动加速、减速刹车等行为。使用全景照片，模拟车辆真实驾驶场景，如高速公路、城市等


流程改进：
1. 结合实景拍摄、全景拍摄技术，增加车辆细节、真实感，减少资金、时间成本。
2. 拆分、压缩车辆模型，使用webGL进行前端材质渲染,减少卡顿
3. 3d场景

产品预研：
首先，基于3D技术的动态车辆展示能够提供更加真实、直观的展示效果，有助于提升用户体验和吸引力。其次，在云端进行车辆展示能够更好地支持多终端访问和跨地域展示，具有更高的灵活性和可扩展性。最后，针对该产品的预研需要囊括用户需求分析、技术可行性研究、市场竞争情况分析等方面，以明确产品定位、功能特性和商业模式。

1. 基于3D技术的动态车辆展示能够提供更加真实、直观的展示效果，有助于提升用户体验和吸引力；满足异地用户线上看车、远程选车的需求。
2. 车辆结构复杂，难以描述故障发生地。使用3D技术

技术储备：
1. 基础的HTML、CSS和JavaScript知识
2. Three.js库、加载器、Tween.js等
3. webGL基础知识、计算机图形学基础知识

需要的资源支持：
1. 书籍：《Three.js开发指南：基于WebGL和HTML5在网页上渲染3D图形和动画（原书第3版）》
2. 陕汽卡车数据，建模用
3. 
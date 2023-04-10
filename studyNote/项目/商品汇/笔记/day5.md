# Note
## 1.mock 模拟数据
### 1.过程
1. `npm install mockjs --legacy-peer-deps`
2. src文件夹下新建mock文件夹
3. 在src/mock下创建json文件，准备json数据（一定要格式化，如果留有空格可能跑不起来）
4. 把mock数据需要的图片放在public文件夹中（public文件夹在打包的时候原封不动的放在dist中）
5. 通过mock模块模拟出数据
	1. 配置banner.json 、floor.json 文件// * webpack 默认对外暴露：图片资源、JSON资源.===>直接再mockSever.js中使用，不用引入
	3. 在mock文件夹中创建一个mockServer.js文件，通过mockjs插件模拟数据
6. 把mockServer.js在入口文件中引入（至少需要执行一次）
7. 向服务器发请求
### 2.Swiper的基本使用---轮播图
[Swiper使用方法 - Swiper中文网](https://swiper.com.cn/usage/index.html)
1.  `npm install --save swiper@5  --legacy-peer-deps`
2. 在main.js中引入相应的依赖包（全局都能用，也可以在当前组件中引用）
```css
// *引入swiper样式
import 'swiper/css/swiper.css'
```
3. 页面中的结构必须要有%%静态页面中结构必须完整【container、wrap、slider】，类名不能瞎写%%
```html
      <div class="center">
        <!--banner轮播-->
        <div class="swiper-container" ref="mySwiper">
          <!-- swiper-wrapper里面每一个slider即为一张图片 -->
          <div class="swiper-wrapper">
            <div class="swiper-slide" v-for="(item, index) in bannerList" :key="item.id">
              <img :src="item.imgUrl" />
            </div>
          </div>
          <!-- 如果需要分页器 -->
          <div class="swiper-pagination"></div>
          <!-- 如果需要导航按钮 -->
          <div class="swiper-button-prev"></div>
          <div class="swiper-button-next"></div>

        </div>

      </div>>
```
5. 初始化swiper实例，给轮播图添加动态效果(watch+$nextTick)
	$nextTick：在下次dom更新，循环结束后，执行回调。%%在修改数据之后，使用此方法，获得更新后的dom。%%
```js
  watch: {
    bannerList() {
      //能在这里直接初始化Swiper类的实例吗?
      //不能在当前状态直接初始化Swiper类的实例,因为这里只能保证数据发生变化了[服务器数据回来了],
      //但是你不能保证v-for遍历的结构完事了.
      this.$nextTick(() => {
        //初始化Swiper类的实例
        var mySwiper = new Swiper(document.querySelector(".swiper-container"), {
          //设置轮播图防线
          direction: "horizontal",
          //开启循环模式
          loop: true,
          // 如果需要分页器
          pagination: {
            el: ".swiper-pagination",
            //分页器类型
            type: "bullets",
            //点击分页器，切换轮播
            clickable: true,//点小球也可以切换
          },
          //自动轮播
          autoplay: {
            delay: 1000,
            //新版本的写法：目前是5版本
            // pauseOnMouseEnter: true,
            //如果设置为true，当切换到最后一个slide时停止自动切换
            stopOnLastSlide: true,
            //用户操作swiper之后，是否禁止autoplay
            disableOnInteraction: false,
          },
          // 如果需要前进后退按钮
          navigation: {
            nextEl: ".swiper-button-next",
            prevEl: ".swiper-button-prev",
          },
          //切换效果
          // effect: "cube",
        });

        //1:swiper插件,对外暴露一个Swiper构造函数
        //2:Swiper构造函数需要传递参数 1、结构总根节点CSS选择器|根节点真实DOM节点  2、轮播图配置项
        //鼠标进入停止轮播
        mySwiper.el.onmouseover = function () {
          mySwiper.autoplay.stop();
        };
        //鼠标离开开始轮播
        mySwiper.el.onmouseout = function () {

          mySwiper.autoplay.start();
        };
      });
    },
  },
```
注意
1. 在 new Swiper实例之前，页面上的dom节点必须有==>不能再mounted上new %% v-for 里面是动态数据，需要服务器返回数据，axios是异步的，返回的会晚一点。这就导致组建的数据不完整,因为没有获取到节点.Swiper需要获取到轮播图的节点DOM，才能给swiper轮播添加动态效果.因此不能在mounted里写%%
2. 初始化swiper实例在哪里书写? watch+$nextTick
3. nextTick官网解释:
	在下次DOM更新, 循环结束之后,执行延迟回调。在 修改数据之后 立即使用这个方法，获取更新后的DOM。
	注意：组件实例的$nextTick方法，在工作当中经常使用，经常结合第三方插件使用，获取更新后的DOM节点
1. 移动端,pc端都可以使用


### 3.实例

#### 1. ListContainer
##### 1.api数据请求
1. 在api中用axios发送请求（mockRequest.js）
2. 在api/index.js中封装请求函数
```js
import mockRequests from "./mockRequests"
// 获取Banner （Home首页轮播图）的接口
// 发请求，返回promise对象
export const reqBannerList = () => mockRequests({ url: '/banner', method: 'get' });

```
3. 在组件ListContainer中：派发action：通过Vuex发起ajax请求，奖数据存储在仓库中
```js
  mounted() {
    // 派发action：通过Vuex发起ajax请求，奖数据存储在仓库中
    this.$store.dispatch("getBannerList");
  },
```
4. 在store中找对应的仓库,写vuex三连环 actions 、mutations、state
```js
import {reqBannerList } from "@/API";
const state = {
    //* state中的默认初始值不要瞎写，与服务器返回值类型一致
    //首页轮播图的数据
    bannerList: [],
};

const mutations = {
    GETBANNERLIST(state, bannerList) {
        state.bannerList = bannerList;
    },
};

const actions = {
    async getBannerList({ commit, state, dispatch }) {
        //获取首页数据
        let result = await reqBannerList();
        if (result.code == 200) {
            commit("GETBANNERLIST", result.data);
        }
    },
};
```
5. 在组件ListContainer中使用vuex中的数据
```js
  computed: {
    ...mapState({
      bannerList: state => state.home.bannerList
    })
  },
```
%%
---对于将来实际工作的时候，后台没有准备好接口（服务器没有开发出来），前端工程师可以利用mock技术，
实现模拟数据，将来项目上线（后台真实接口）写好了，替换为真实接口即可。
---对于咱们而言，后台老师确实没有给首页中轮播这部分的接口，mock数据，你可以当中一个真实接口就行了。
上线的时候，对于mock数据对于项目而言没有任何影响。

对于项目而言:真实的接口 /api/xxxx    模拟的数据/mock/xxxx
模拟数据JSON：没有空格，最好使用格式化插件进行格式化。空格，最好使用格式化插件进行格式化。%%



##### 2. Swiper轮播图
[Swiper使用方法 - Swiper中文网](https://swiper.com.cn/usage/index.html)
1.  `npm install --save swiper@5  --legacy-peer-deps`
2. 在main.js中引入相应的依赖包（全局都能用，也可以在当前组件中引用）
```css
// *引入swiper样式
import 'swiper/css/swiper.css'
```
3. 页面中的结构必须要有
```html
      <div class="center">
        <!--banner轮播-->
        <div class="swiper-container" ref="mySwiper">
          <!-- swiper-wrapper里面每一个slider即为一张图片 -->
          <div class="swiper-wrapper">
            <div class="swiper-slide" v-for="(item, index) in bannerList" :key="item.id">
              <img :src="item.imgUrl" />
            </div>
          </div>
          <!-- 如果需要分页器 -->
          <div class="swiper-pagination"></div>
          <!-- 如果需要导航按钮 -->
          <div class="swiper-button-prev"></div>
          <div class="swiper-button-next"></div>

        </div>

      </div>>
```
5. 初始化swiper实例，给轮播图添加动态效果(watch+$nextTick)
	$nextTick：在下次dom更新，循环结束后，执行回调。%%在修改数据之后，使用此方法，获得更新后的dom。%%
```js
  watch: {
    bannerList() {
      //能在这里直接初始化Swiper类的实例吗?
      //不能在当前状态直接初始化Swiper类的实例,因为这里只能保证数据发生变化了[服务器数据回来了],
      //但是你不能保证v-for遍历的结构完事了.
      this.$nextTick(() => {
        //初始化Swiper类的实例，这里也可以用ref
       
        var mySwiper = new Swiper(document.querySelector(".swiper-container"), {
          //设置轮播图防线
          direction: "horizontal",
          //开启循环模式
          loop: true,
          // 如果需要分页器
          pagination: {
            el: ".swiper-pagination",
            //分页器类型
            type: "bullets",
            //点击分页器，切换轮播
            clickable: true,//点小球也可以切换
          },
          //自动轮播
          autoplay: {
            delay: 1000,
            //新版本的写法：目前是5版本
            // pauseOnMouseEnter: true,
            //如果设置为true，当切换到最后一个slide时停止自动切换
            stopOnLastSlide: true,
            //用户操作swiper之后，是否禁止autoplay
            disableOnInteraction: false,
          },
          // 如果需要前进后退按钮
          navigation: {
            nextEl: ".swiper-button-next",
            prevEl: ".swiper-button-prev",
          },
          //切换效果
          // effect: "cube",
        });

        //1:swiper插件,对外暴露一个Swiper构造函数
        //2:Swiper构造函数需要传递参数 1、结构总根节点CSS选择器|根节点真实DOM节点  2、轮播图配置项
        //鼠标进入停止轮播
        mySwiper.el.onmouseover = function () {
          mySwiper.autoplay.stop();
        };
        //鼠标离开开始轮播
        mySwiper.el.onmouseout = function () {

          mySwiper.autoplay.start();
        };
      });
    },
  },
```
注意
1. 在 new Swiper实例之前，页面上的dom节点必须有==>不能再mounted上new %% v-for 里面是动态数据，需要服务器返回数据，axios是异步的，返回的会晚一点。这就导致组建的数据不完整%%
2. 移动端,pc端都可以使用

#### 2. Floor组件
##### 1.api数据请求
1. api/index.js
```js
export const reqFloorList = () => mockRequests({ url: '/floor', method: 'get' });
```

2. store/home %%vuex三连环%%
```js
const state = {
    floorList: [],
};

const mutations = {
    GETFLOORLIST(state, floorList) {
        state.floorList = floorList;
    },
};
const actions = {
    async getFloorList({ commit }) {
        let result = await reqFloorList();
        if (result.code == 200) {
            commit("GETFLOORLIST", result.data);
        }
    },

};

```

3. 触发action

> getFloorList这个action要在home路由组件中触发。

%%因为getFloorList返回的数据FloorList是个数组，需要遍历。但是在floor内部无法v-for遍历，因此要在home中进行对FloorList的v-for遍历%%
%%派发action应该是在父组件的组件挂载完毕生命周期函数中书写，因为父组件需要通知Vuex发请求，父组件获取到mock数据，通过v-for遍历 生成多个floor组件，因此达到复用作用。%%
pages/home/index.vue
```js
  mounted() {
    this.$store.dispatch("getFloorList")
  },
  computed: {
    ...mapState({ floorList: state => state.home.floorList })
```

4. v-for|v-show|v-if|这些指令可以在自定义标签（组件）的身上使用
5. 父子组件通信
```html
    <Floor v-for="floor in floorList " :key="floor.id"  :floorList="floorList"/>
```

##### 2.Swiper轮播图
1. 引入
2. 结构
```html
<div class="swiper-wrapper" >
	<div class="swiper-slide" v-for="(carouse,index) in floorList.carouselList " :key="carouse.id">
		<img src="carouse.imgUrl">
	</div>
</div>
```
3. 初始化
```js
```
>  为什么在Floor组件的mounted中初始化SWiper实例轮播图可以使用?

1. 因为floor内部没有异步请求。数据请求是父组件做的，数据是父组件给的。
2. mounted执行的时候，结构都已经有了。


# question
 1. 在store中使用API里的方法，一定要引入此方法
 2. mock无法使用？ 没有在main.js中引用
### 1.组件间通信方式
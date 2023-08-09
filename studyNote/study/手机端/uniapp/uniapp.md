
## 概述

[uni-app官网 (dcloud.net.cn)](https://uniapp.dcloud.net.cn/resource.html)

[[官方网课]]https://ke.qq.com/course/3169971#term_id=103296764

mpvue [mpvue.com](http://mpvue.com/)

H5 Plus

nvue: vue原生，作为局部使用更合适

![[Pasted image 20230802141851.png]]

[uni-app组成和跨端原理 | uni-app官网 (dcloud.net.cn)](https://uniapp.dcloud.net.cn/tutorial/)

[uni-app —— 使用Vue.js注意事项 - 掘金 (juejin.cn)](https://juejin.cn/post/6881459593868918792?searchId=202308021516166209C4BF362494A4BC06)


## 使用

### tips

 > [!TIP] 建议使用绝对路径

> [!TIP]  tarBar导航菜单与opentype跳转差异
> tarBar里面有的，不能谁用navigate跳转
> open-type 为 relaunch （可携带参数） 则可以跳转


### easycom

`easycom` ：只要组件安装在项目根目录或uni_modules的components目录下，并符合`components/组件名称/组件名称.vue`或`uni_modules/插件ID/components/组件名称/组件名称.vue`目录结构。就可以不用引用、注册，直接在页面中使用。

在components下面创建新组件，勾选新建同名目录

[pages.json 页面路由 | uni-app官网 (dcloud.net.cn)](https://uniapp.dcloud.net.cn/collocation/pages.html#easycom)

### onload页面传参与vueroute路由差异

> [!tip] 小程序不支持 this.$router

当获取参数的时候，建议使用 `onLoad(e)`

在 onLoad 里得到，onLoad 的参数是其他页面打开当前页面所传递的数据。

### 交互反馈

[uni.showToast(OBJECT) | uni-app官网 (dcloud.net.cn)](https://uniapp.dcloud.net.cn/api/ui/prompt.html#showtoast )


` uni.hideToast() `建议 `mask` 设置为 `true` ，加载的时候不能进行其他操作


### 在uniapp中使用Echart

[uni-app引用echarts_uniapp引入echarts_花归去的博客-CSDN博客](https://blog.csdn.net/weixin_42120669/article/details/106123645)

## practice01


[表单验证、数据验证 - graceUI js 模块 - DCloud 插件市场](https://ext.dcloud.net.cn/plugin?id=383)


##### 去掉scrollview的滚动条 
[uniapp去除滚动条的方法_uniapp去掉滚动条_前端若水的博客-CSDN博客](https://blog.csdn.net/qq_37547964/article/details/109530142)

```scss
.navscroll {
		// 去掉自带的滑动条
		// /deep/::-webkit-scrollbar {
		// 	display: none;
		// }

		/deep/::-webkit-scrollbar {
			display: none !important;
			width: 0 !important;
			height: 0 !important;
			-webkit-appearance: none;
			background: transparent;
		}
	}
```

1. `/deep/` 由于 `<style>` 的 `scoped` 属性，无法更改子组件的样式，需要使用深度选择器
[vue中的css深度选择器 :deep(<inner-selector>)、/deep/、>>>、::v-deep 到底是什么？ - 掘金 (juejin.cn)](https://juejin.cn/post/6978781674070884366)
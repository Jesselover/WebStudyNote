## Echart 表格学习

[快速上手 - Handbook - Apache ECharts](https://echarts.apache.org/handbook/zh/get-started/)

[15.15-ECharts体验使用五部曲(Av582842505,P15)_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1ap4y1q7JV?p=15&spm_id_from=pageDriver&vd_source=9ff9288661d168a184d858583892913c)

### 1. 步骤

1. 下载并引入Echart.js文件 %%图表依赖这个js库%%
2. 准备一个具备大小的DOM容器  %%生成的图表会放入这个容器内  `var myChart = echart.init(dom)`%%
3. 初始化Echart实例对象 %%实例化echart对象%%
4. 指定配置项和数据（option） %%根据具体需求修改配置选项%%
5. 将配置项设置给echart实例对象  %% `myCahrt.setOption（option）`%%

为什么没有 `echart.init(dom)` 初始化数据
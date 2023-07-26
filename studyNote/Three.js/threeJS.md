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
5. `npm install three`
6. 
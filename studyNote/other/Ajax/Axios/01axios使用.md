## 1.基本操作
1. axios函数
	- 返回值:Promise对象
2. 基本操作
	1. 获取文章GET/posts/id
	2. 添加一篇新的文章POST/post
	3. 更新数据PUT/posts/id
	4. 删除数据delete/posts/3
```js
 const btns = document.querySelectorAll('button');
        //获取文章GET/posts/id
        btns[0].onclick = function() {
            //发送 AJAX 请求
            // axios()返回一个promise对象
            axios({
                //请求类型
                method: 'GET',
                //URL
                url: 'http://localhost:3000/posts/2',
            }).then(response => {
                console.log(response);
            });
        }
        //添加一篇新的文章POST/post
        btns[1].onclick = function() {
            //发送 AJAX 请求
            axios({
                //请求类型
                method: 'POST',
                //URL
                url: 'http://localhost:3000/posts',
                //设置请求体
                data: {
                    title: "今天天气不错, 还挺风和日丽的",
                    author: "张三"
                }
            }).then(response => {
                console.log(response);
            });
        }
        //更新数据PUT/posts/id
        btns[2].onclick = function() {
            //发送 AJAX 请求
            axios({
                //请求类型
                method: 'PUT',
                //URL
                url: 'http://localhost:3000/posts/3',
                //设置请求体
                data: {
                    title: "今天天气不错, 还挺风和日丽的",

                    author: "李四"
                }
            }).then(response => {
                console.log(response);
            });
        }
        //删除数据delete/posts/3
        btns[3].onclick = function() {
            //发送 AJAX 请求
            axios({
                //请求类型
                method: 'delete',
                //URL
                url: 'http://localhost:3000/posts/3',
            }).then(response => {
                console.log(response);
            });
        }
```
3. Axios 实例
	`axios.create([config])`
	实例方法:
	```
		axios#request(config)
		axios#get(url[, config])
		axios#delete(url[, config])
		axios#head(url[, config])
		axios#options(url[, config])
		axios#post(url[, data[, config]])
		axios#put(url[, data[, config]])
		axios#patch(url[, data[, config]])
		axios#getUri([config])
	```
5. Axios API
	1. `axios.request(config)`发送 GET 请求
	2. `axios.post(url[,data][,config])`发送 POST 请求
	3. (https://github.com/axios/axios#request-method-aliases)
	4. 中文 ([Axios API | Axios 中文文档 | Axios 中文网 (axios-http.cn)](https://www.axios-http.cn/docs/api_intro))

	```js
	        //发送 GET 请求
	        btns[0].onclick = function() {
	            // axios()
	            // axios.request(config)
	            axios.request({
	                method: 'GET',
	                url: 'http://localhost:3000/comments'
	            }).then(response => {
	                console.log(response);
	            })
	        }
	        //发送 POST 请求
	        btns[1].onclick = function() {
	            // axios()
	            // axios.post(url[,data][,config])
	            axios.post(
	                'http://localhost:3000/comments', {
	                    "body": "喜大普奔",
	                    "postId": 2
	                }).then(response => {
	                console.log(response);
	            })
	        }
	```
	5. 请求方式别名:在使用别名方法时， `url`、`method`、`data` 这些属性都不必在配置中指定。
	```
		axios.request(config)
		axios.get(url[, config])
		axios.delete(url[, config])
		axios.head(url[, config])
		axios.options(url[, config])
		axios.post(url[, data[, config]])
		axios.put(url[, data[, config]])
		axios.patch(url[, data[, config]])
	```
## 2.axios响应
([响应结构 | Axios 中文文档 | Axios 中文网 (axios-http.cn)](https://www.axios-http.cn/docs/res_schema))
- axios发送请求响应结果的介绍
config
data:对象/响应体的结果
headers:响应头信息
request:原生AJAX请求对象 XMLHttpRquest
status:响应码
statusText:响应字符串

## 3.axios请求对象 Request Config
- 请求配置/axios在调用时,所接受的参数对象 
- (https://github.com/axios/axios#request-config)
- ([请求配置 | Axios 中文文档 | Axios 中文网 (axios-http.cn)](https://www.axios-http.cn/docs/req_config))
	1. baseURL 基础url
	2. transformRequest 对请求的数据做处理,处理后再发送
	3. transformResponse 对相应的数据做改变,改变后再处理
	4. headers 对象,配置请求头信息
	5. params 对象,设置url参数/url参数用这个
	6. paramsSerializer 对请求的参数序列化,转化为字符串
	7. data 请求体设置/JSON形式数据用这个
		- 字符串形式:直接传递
		- 对象形式:转化成JSON传递
	8. timeout
	9. withCredentials// default false 跨域请求时是否可以携带cookie
	10.  adapter: function (config) { },设置请求识别器
		1. AJAX
		2. node.js发送http请求
	11. auth:{ username: 'janedoe',password:'s00pers3cret '},
	12.  responseType: 'json', // default

## 4.默认配置
```js
        //默认配置
        axios.defaults.method = 'GET';//设置默认的请求类型为 GET
        axios.defaults.baseURL = 'http://localhost:3000';//设置基础 URL
        axios.defaults.params = {id:100};
        axios.defaults.timeout = 3000;//
```

## 5.axios创建实例对象
([默认配置 | Axios 中文文档 | Axios 中文网 (axios-http.cn)](https://www.axios-http.cn/docs/config_defaults))
- 应用场景:给不同服务器发请求
```js
  //创建实例对象  /getJoke
   //这里  duanzi 与 axios 对象的功能几近是一样的
        const duanzi = axios.create({
            baseURL: 'https://api.apiopen.top',
            timeout: 2000
        });
        const onather = axios.create({
            baseURL: 'https://b.com',
            timeout: 2000
        });
        duanzi.get('/getJoke').then(response => {
            console.log(response.data)
        })
```

## 6.拦截器 
[拦截器 | Axios 中文文档 | Axios 中文网 (axios-http.cn)](https://www.axios-http.cn/docs/interceptors)
- 本质:函数
- 分类:
	  请求拦截器
	  响应拦截器
1. 请求拦截器:
	在真正发送请求前执行的回调函数
	可以对请求进行检查或配置进行特定处理
	成功的回调函数, 传递的默认是 config(也必须是)
	失败的回调函数, 传递的默认是 error
2. 响应拦截器
	在请求得到响应后执行的回调函数
	可以对响应数据进行特定处理
	成功的回调函数, 传递的默认是 response
	失败的回调函数, 传递的默认是 error
## 7.取消请求
([取消请求 | Axios 中文文档 | Axios 中文网 (axios-http.cn)](https://www.axios-http.cn/docs/cancellation))
- ancelToken: new axios.CancelToken(function)     
```js
    const btns = document.querySelectorAll('button');
        //2.声明全局变量
        let cancel = null;
        //发送请求
        btns[0].onclick = function(){
            //检测上一次的请求是否已经完成
            if(cancel !== null){
                //取消上一次的请求
                cancel();
            }
            // 发送请求
            axios({
                method: 'GET',
                url: 'http://localhost:3000/posts',
                //1. 添加配置对象的属性
                cancelToken: new axios.CancelToken(function(c){
                    //3. 将 c 的值赋值给 cancel
                    cancel = c;
                })
            }).then(response => {
                console.log(response);
                //将 cancel 的值初始化
                cancel = null;
            })
        }
        //绑定第二个事件取消请求
        btns[1].onclick = function(){
            cancel();
        }
```
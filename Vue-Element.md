## 一、Ajax

### 1.Ajax介绍

#### 1.1 Ajax概述

Ajax: 全称Asynchronous JavaScript And XML，异步的JavaScript和XML。其作用有如下2点：

- 与服务器进行数据交换：通过Ajax可以给服务器发送请求，并获取服务器响应的数据。
- 异步交互：可以在**不重新加载整个页面**的情况下，与服务器交换数据并**更新部分网页**的技术，如：搜索联想、用户名是否可用的校验等等。



https://raw.githubusercontent.com/



#### 1.2 Ajax作用

Ajax技术的2个作用

- ①与服务器进行数据交互

  ​        如下图所示前端资源被浏览器解析，但是**前端页面上缺少数据**，前端可以通过Ajax技术，向后台服务器发起请求，后台服务器接受到前端的请求，**从数据库中获取前端需要的资源**，然后响应给前端，前端在通过我们学习的vue技术，可以将数据展示到页面上，这样用户就能看到完整的页面了。此处可以对比JavaSE中的网络编程技术来理解。

  ![image-20240806152524507](assets/image-20240806152524507.png)

  

- ②异步交互：可以在**不重新加载整个页面**的情况下，与服务器交换数据并**更新部分网页**的技术。

  ​        如下图所示，当我们再百度搜索java时，下面的联想数据是通过Ajax请求从后台服务器得到的，在整个过程中，我们的Ajax请求不会导致整个百度页面的重新加载，并且只针对搜索栏这局部模块的数据进行了数据的更新，不会对整个页面的其他地方进行数据的更新，这样就大大提升了页面的加载速度，用户体验高。

  ![image-20240806152543146](assets/image-20240806152543146.png)







#### 1.3 同步与异步

针对于上述Ajax的局部刷新功能是因为Ajax请求是异步的，与之对应的有同步请求。

接下来我们介绍一下异步请求和同步请求的区别。

- ①同步请求发送过程如下图所示：

  ![image-20240806152557282](assets/image-20240806152557282.png)

  浏览器页面在发送请求给服务器，在服务器处理请求的过程中，浏览器页面不能做其他的操作。只能等到服务器响应结束后才能，浏览器页面才能继续做其他的操作。 

  

- ②异步请求发送过程如下图所示：

  ![image-20240806152611892](assets/image-20240806152611892.png) 

  浏览器页面发送请求给服务器，在服务器处理请求的过程中，浏览器页面还可以做其他的操作。











### 2. 原生Ajax

​         对于Ajax技术有了充分的认知了，接下来先采用原生的Ajax代码来演示。因为Ajax请求是基于客户端发送请求，服务器响应数据的技术。所以为了完成快速入门案例，我们需要提供服服务器端和编写客户端。

* ①服务器端：

因为我们暂时还没学过服务器端的代码，所以此处已经直接提供好了服务器端的请求地址，我们前端直接通过Ajax请求访问该地址即可。

后台服务器地址：https://jsonplaceholder.typicode.com/posts

上述地址我们也可以直接通过浏览器来访问，访问结果如图所示：![image-20240806152347941](assets/image-20240806152347941.png)



* 客户端：

客户端的Ajax请求代码如下有如下4步，接下来我们跟着步骤一起操作一下。

①首先我们再VS Code中创建ajxa的文件夹，并且创建名为01. Ajax-原生方式.html的文件，提供如下代码，主要是按钮绑定单击事件，我们希望点击按钮，来发送ajax请求

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>原生Ajax</title>
</head>
<body>
    <input type="button" value="获取数据" onclick="getData()">
    <div id="div1"></div>
    
</body>
<script>
    function getData(){
     
    }
</script>
</html>
```

②创建XMLHttpRequest对象，用于和服务器交换数据，也是原生Ajax请求的核心对象，提供了各种方法。代码如下：

```js
//1. 创建XMLHttpRequest 
var xmlHttpRequest  = new XMLHttpRequest();
```

③调用对象的open()方法设置请求的参数信息，例如请求地址，请求方式。然后调用send()方法向服务器发送请求，代码如下：

```js
//2. 发送异步请求
xmlHttpRequest.open('GET','https://jsonplaceholder.typicode.com/posts');
xmlHttpRequest.send();//发送请求
```

④我们通过绑定事件的方式，来获取服务器响应的数据。

```js
//3. 获取服务响应数据
xmlHttpRequest.onreadystatechange = function(){
    //此处判断 4表示浏览器已经完全接受到Ajax请求得到的响应， 200表示这是一个正确的Http请求，没有错误
    if(xmlHttpRequest.readyState == 4 && xmlHttpRequest.status == 200){
        document.getElementById('div1').innerHTML = xmlHttpRequest.responseText;
    }
}
```



通过浏览器打开页面，请求点击按钮，发送Ajax请求，最终显示结果如下图所示：

![image-20240806154037161](assets/image-20240806154037161.png)

点击网络中的XHR请求，发现可以抓包到我们发送的Ajax请求，XHR代表的就是异步请求。



完整代码如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>原生Ajax</title>
</head>
    
<body>
    <input type="button" value="获取数据" onclick="getData()">
    <div id="div1">
    </div> 
</body>
    
<script>
    function getData(){
        // 1.创建XMLHttpRequest
        var xmlHttpRequest = new XMLHttpRequest();
        // 2.发送异步请求
        xmlHttpRequest.open('GET','https://jsonplaceholder.typicode.com/posts');
        xmlHttpRequest.send(); // 发送请求
        // 3.获取服务器响应数据
        xmlHttpRequest.onreadystatechange = function(){
            if(xmlHttpRequest.readyState == 4 && xmlHttpRequest.status == 200){
                document.getElementById('div1').innerHTML = xmlHttpRequest.responseText;
            }
        }
    }
</script>
</html>
```











### 3.Axios

上述原生的Ajax请求的代码编写起来还是比较繁琐的，所以接下来我们学习一门更加简单的发送Ajax请求的技术Axios 。

Axios是对原生的AJAX进行封装，简化书写，Axios官网：`https://www.axios-http.cn`





#### 3.1 Axios的基本使用

axios()的语法格式：

```js
// 方法1
axios({
    method:"",
    url:"",
    data:"",
}).then(function (resp){
})
```

axios()是用来发送异步请求的，小括号中使用 js的JSON对象传递请求相关的参数：

- method属性：用来设置请求方式的。取值为 get 或者 post。
- url属性：用来书写请求的资源路径。如果是 get 请求，需要将请求参数拼接到路径的后面，格式为： url?参数名=参数值&参数名2=参数值2。
- data属性：作为请求体被发送的数据。也就是说如果是 post 请求的话，数据需要作为 data 属性的值。

* then() 需要传递一个匿名函数。我们将 then()中传递的匿名函数称为 **回调函数**，意思是该匿名函数在发送请求时不会被调用，而是在成功响应后调用的函数。而该回调函数中的 resp 参数是对响应的数据进行封装的对象，通过 resp.data 可以获取到响应的数据。



Axios的使用比较简单，主要分为2步：

* ①引入Axios文件：

```html
<script src="js/axios-0.18.0.js"></script>
```

* ②使用Axios发送请求，并获取响应结果，官方提供的api很多，此处给出2种如下：

  * 发送get请求：

  ```js
  axios({
      method:"get",
      url:"https://jsonplaceholder.typicode.com/posts"
  }).then(function (resp){
      alert(resp.data);
  })
  ```

  * 发送post请求：

  ```js
  axios({
      method:"post",
      url:"https://jsonplaceholder.typicode.com/posts",
      data:"id=1"
  }).then(function (resp){
      alert(resp.data);
  });
  ```



效果如图所示：

![image-20240806164108142](assets/image-20240806164108142.png)



完整代码如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ajax-Axios</title>
    <script src="js/axios-0.18.0.js"></script>
</head>
<body>
    
    <input type="button" value="获取数据GET" onclick="get()">

    <input type="button" value="删除数据POST" onclick="post()">

</body>
<script>
    function get(){
        //通过axios发送异步请求-get
        axios({
             method:"get",
             url:"https://jsonplaceholder.typicode.com/posts"
         }).then(result=>{
             console.log(result.data);
         })
    }

    function post(){
        //通过axios发送异步请求-post
         axios({
             method:"post",
             url:"https://jsonplaceholder.typicode.com/posts",
             data:"id=1"
         }).then(result=>{
             console.log(result.data);
        })
    }
</script>
</html>
```







#### 3.2 Axios快速入门

Axios还针对不同的请求，提供了别名方式的api,具体如下：

| 方法                               | 描述           |
| ---------------------------------- | -------------- |
| axios.get(url [, config])          | 发送get请求    |
| axios.delete(url [, config])       | 发送delete请求 |
| axios.post(url [, data[, config]]) | 发送post请求   |
| axios.put(url [, data[, config]])  | 发送put请求    |

我们目前只关注get和post请求，所以在上述的入门案例中，我们可以将get请求代码改写成如下：

~~~js
axios.get("https://jsonplaceholder.typicode.com/posts").then(result => {
    console.log(result.data);
})
~~~

post请求改写成如下：

~~~js
axios.post("https://jsonplaceholder.typicode.com/posts","id=1").then(result => {
    console.log(result.data);
})
~~~



代码如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ajax-Axios</title>
    <script src="js/axios-0.18.0.js"></script>
</head>
<body>
  
    <input type="button" value="获取数据GET" onclick="get()">
    <input type="button" value="删除数据POST" onclick="post()">

</body>
<script>
    function get(){
        //通过axios发送异步请求-get
        axios.get("https://jsonplaceholder.typicode.com/posts").then(result =>{
            console.log(result.data);
        })
    }

    function post(){
        //通过axios发送异步请求-post
        axios.post("https://jsonplaceholder.typicode.com/posts","id=1").then(result=>{
            console.log(result.data);
        })
    }
</script>
</html>
```







#### 3.3 案例

需求：基于Vue及Axios完成数据的动态加载展示，如下图所示

![image-20240806210304743](assets/image-20240806210304743.png) 

其中数据是来自于后台程序的，地址是： https://apifoxmock.com/m1/4950168-4607885-default/emp/list



- 分析：

  前端首先是一张表格，我们缺少数据，而提供数据的地址已经有了，所以意味这我们需要使用Ajax请求获取后台的数据。但是Ajax请求什么时候发送呢？页面的数据应该是页面加载完成，自动发送请求，展示数据，所以我们需要借助vue的mounted钩子函数。那么拿到数据了，我们该怎么将数据显示表格中呢？这里就得借助v-for指令来遍历数据，展示数据。

- 步骤：

  1. 首先创建文件，提前准备基础代码，包括表格以及vue.js和axios.js文件的引入
  2. 我们需要在vue的mounted钩子函数中发送ajax请求，获取数据
  3. 拿到数据，数据需要绑定给vue的data属性
  4. 在&lt;tr&gt;标签上通过v-for指令遍历数据，展示数据

- 代码实现：

  1. 首先创建文件，提前准备基础代码，包括表格以及vue.js和axios.js文件的引入

     ![image-20240806210413753](assets/image-20240806210413753.png)

     初始代码如下：

     ~~~html
     <!DOCTYPE html>
     <html lang="en">
     <head>
         <meta charset="UTF-8">
         <meta http-equiv="X-UA-Compatible" content="IE=edge">
         <meta name="viewport" content="width=device-width, initial-scale=1.0">
         <title>Ajax-Axios-案例</title>
         <script src="js/axios-0.18.0.js"></script>
         <script src="js/vue.js"></script>
     </head>
     <body>
         <div id="app">
             <table border="1" cellspacing="0" width="60%">
                 <tr>
                     <th>编号</th>
                     <th>姓名</th>
                     <th>图像</th>
                     <th>性别</th>
                     <th>职位</th>
                     <th>入职日期</th>
                     <th>最后操作时间</th>
                 </tr>
     
                 <tr align="center" >
                     <td>1</td>
                     <td>Tom</td>
                     <td>
                         <img src="" width="70px" height="50px">
                     </td>
                     <td>
                         <span>男</span>
                        <!-- <span>女</span>-->
                     </td>
                     <td>班主任</td>
                     <td>2009-08-09</td>
                     <td>2009-08-09 12:00:00</td>
                 </tr>
             </table>
         </div>
     </body>
     <script>
         new Vue({
            el: "#app",
            data: {
             
            }
         });
     </script>
     </html>
     ~~~

  2. 在vue的mounted钩子函数，编写Ajax请求，请求数据，代码如下：

     ~~~js
     mounted () {
         //发送异步请求,加载数据
         axios.get("https://apifoxmock.com/m1/4950168-4607885-default/emp/list").then(result => {
             
         })
     }
     ~~~

  3. ajax请求的数据我们应该绑定给vue的data属性，之后才能进行数据绑定到视图；并且浏览器打开后台地址，数据返回格式如下图所示：

     ![image-20240806210540222](assets/image-20240806210540222.png) 

     因为服务器响应的json中的data属性才是我们需要展示的信息，所以我们应该将员工列表信息赋值给vue的data属性，代码如下：

     ~~~js
      //发送异步请求,加载数据
     axios.get("http://yapi.smart-xwork.cn/mock/169327/emp/list").then(result => {
         this.emps = result.data.data;
     })
     ~~~

     其中，data中生命emps变量，代码如下：

     ~~~js
     data: {
         emps:[]
     },
     ~~~

  4. 在&lt;tr&gt;标签上通过v-for指令遍历数据，展示数据，其中需要注意的是图片的值，需要使用vue的属性绑定，男女的展示需要使用条件判断，其代码如下：

     ~~~html
     <tr align="center" v-for="(emp,index) in emps">
         <td>{{index + 1}}</td>
         <td>{{emp.name}}</td>
         <td>
             <img :src="emp.image" width="70px" height="50px">
         </td>
         <td>
             <span v-if="emp.gender == 1">男</span>
             <span v-if="emp.gender == 2">女</span>
         </td>
         <td>{{emp.job}}</td>
         <td>{{emp.entrydate}}</td>
         <td>{{emp.updatetime}}</td>
     </tr>
     ~~~

完整代码如下：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ajax-Axios-案例</title>
    <script src="js/axios-0.18.0.js"></script>
    <script src="js/vue.js"></script>
</head>
<body>
    <div id="app">
        <table border="1" cellspacing="0" width="60%">
            <tr>
                <th>编号</th>
                <th>姓名</th>
                <th>图像</th>
                <th>性别</th>
                <th>职位</th>
                <th>入职日期</th>
                <th>最后操作时间</th>
            </tr>

            <tr align="center" v-for="(emp,index) in emps">
                <td>{{index+1}}</td>
                <td>{{emp.name}}</td>
                <td>
                    <img :src="emp.image" width="70px" height="50px">
                </td>
                <td>
                    <span v-if="emp.gender == 1">男</span>
                    <span v-if="emp.gender == 2">女</span>
                </td>
                <td>{{emp.job}}</td>
                <td>{{emp.entrydate}}</td>
                <td>{{emp.updatetime}}</td>
            </tr>
        </table>
    </div>
</body>
<script>
    new Vue({
       el: "#app",
       data: {
            emps:[]
       },
       mounted(){
        // 发送异步请求，加载数据
        axios.get("https://apifoxmock.com/m1/4950168-4607885-default/emp/list").then(result =>{
            console.log(result.data);
            this.emps = result.data.data;
        })
       }
    });
</script>
</html>
~~~













## 二、前后台分离开发

### 1.前后台分离开发介绍

前端开发有2种方式：**前后台混合开发**和**前后台分离开发**。

前后台混合开发，顾名思义就是前台后台代码混在一起开发，如下图所示：

![image-20240806170123530](assets/image-20240806170123530.png)

这种开发模式有如下缺点：

- 沟通成本高：后台人员发现前端有问题，需要找前端人员修改，前端修改成功，再交给后台人员使用
- 分工不明确：后台开发人员需要开发后台代码，也需要开发部分前端代码。很难培养专业人才
- 不便管理：所有的代码都在一个工程中
- 不便维护和扩展：前端代码更新，和后台无关，但是需要整个工程包括后台一起重新打包部署。



所以我们目前基本都是采用的前后台分离开发方式，如下图所示：

![image-20240806170157916](assets/image-20240806170157916.png)

> ​        将原先的工程分为前端工程和后端工程这2个工程，然后前端工程交给专业的前端人员开发，后端工程交给专业的后端人员开发。前端页面需要数据，可以通过发送异步请求，从后台工程获取。但是，我们前后台是分开来开发的，那么前端人员怎么知道后台返回数据的格式呢？后端人员开发，怎么知道前端人员需要的数据格式呢？所以针对这个问题，我们前后台统一指定一套规范！我们前后台开发人员都需要遵循这套规范开发，这就是我们的**接口文档**。接口文档有离线版和在线版本，接口文档示可以查询今天提供**资料/接口文档示例**里面的资料。那么接口文档的内容怎么来的呢？是我们后台开发者根据产品经理提供的产品原型和需求文档所撰写出来的，产品原型示例可以参考今天提供**资料/页面原型**里面的资料。

那么基于前后台分离开发的模式下，我们后台开发者开发一个功能的具体流程如何呢？如下图所示：

![image-20240806170340724](assets/image-20240806170340724.png)

1. 需求分析：首先我们需要阅读需求文档，分析需求，理解需求。
2. 接口定义：查询接口文档中关于需求的接口的定义，包括地址，参数，响应数据类型等等
3. 前后台并行开发：各自按照接口文档进行开发，实现需求
4. 测试：前后台开发完了，各自按照接口文档进行测试
5. 前后段联调测试：前段工程请求后端工程，测试功能







### 2.API

#### 2.1 APIfox介绍

前后台分离开发中，我们前后台开发人员都需要遵循接口文档，所以接下来我们介绍一款撰写接口文档的平台。

APIfox是高效、易用、功能强大的 api 管理平台，旨在为开发、产品、测试人员提供更优雅的接口管理服务。

其官网地址：https://apifox.com/

APIfox主要提供了2个功能：

- API接口管理：根据需求撰写接口，包括接口的地址，参数，响应等等信息。
- Mock服务：模拟真实接口，生成接口的模拟测试数据，用于前端的测试。







#### 2.2 接口文档管理

接下来我们演示一下APIfox是如何管理接口文档的。

①首先我们登录APIfox的官网，可下载app或使用Web版，如下图所示

![image-20240806170747587](assets/image-20240806170747587.png)



②登录进去后，在个人空间中，添加项目，效果如图所示：

<img src="assets/image-20240806171718920.png" alt="image-20240806171718920" style="zoom:50%;" />

新建项目名称：前端项目1

<img src="assets/image-20240806171639838.png" alt="image-20240806171639838" style="zoom: 50%;" />



③在项目接口下新建一个目录：用户管理

<img src="assets/image-20240806172000106.png" alt="image-20240806172000106" style="zoom: 50%;" />



④到接口的编辑界面 ，对接口做生层次的定制，例如：接口的请求参数参数，接口的返回值等等，效果图下图所示：

<img src="assets/image-20240806173658784.png" alt="image-20240806173658784" style="zoom:80%;" />



添加接口的返回值，如下图所示：

![image-20240806174027915](assets/image-20240806174027915.png)





⑤可以设置接口的mock信息的期望值，如图所示：

![image-20240806174620400](assets/image-20240806174620400.png)

![image-20240806174339326](assets/image-20240806174339326.png)





⑥在接口的预览界面，直接点击Mock地址，如下图所示：

![image-20240806174604583](assets/image-20240806174604583.png)

快捷请求后返回如下数据：

![image-20240806174857681](assets/image-20240806174857681.png)

如上步骤就是APIfox接口平台中对于接口的配置步骤。























## 三、前端工程化

### 1.环境准备

​		前端工程化是通过vue官方提供的脚手架Vue-cli来完成的，用于快速的生成一个Vue的项目模板。Vue-cli主要提供了如下功能：

- 统一的目录结构
- 本地调试
- 热部署
- 单元测试
- 集成打包上线

我们需要运行Vue-cli，需要依赖NodeJS，NodeJS是前端工程化依赖的环境。所以我们需要先安装NodeJS，然后才能安装Vue-cli



- NodeJS安装和Vue-cli安装

**NodeJS的安装：**

①在官网选择下载长期支持的版本

![image-20240807145541025](assets/image-20240807145541025.png)



②安装过程默认点下一步即可

![image-20240807145852013](assets/image-20240807145852013.png)



③NodeJS安装完后会自动配置好环境变量，可以自行验证是否安装成功，在终端输入命令：node -v

![image-20240807145810966](assets/image-20240807145810966.png)



④配置npm的全局安装路径

![image-20240807145946577](assets/image-20240807145946577.png)

使用管理员身份运行命令行，在命令行中，执行如下指令：

```
npm config set prefix "E:\node"
```

注意：E:\node 这个目录是NodeJS的安装目录

![image-20240807150105781](assets/image-20240807150105781.png)



⑤切换npm的淘宝镜像

使用管理员身份运行命令行，在命令行中，执行如下指令：

```
npm config set registry http://registry.npm.taobao.org/
```







**Vue-cli的安装：**

①使用管理员身份运行命令行，在命令行中，执行如下指令：

```
npm install -g @vue/cli
```

这个过程中，会联网下载，可能会耗时几分钟，耐心等待。

![image-20240807150246041](assets/image-20240807150246041.png)



②验证是否已安装，在终端输入命令：vue -versio

![image-20240807150357722](assets/image-20240807150357722.png)







### 2.Vue项目简介

​		环境准备好后，接下来我们需要通过Vue-cli创建一个vue项目，然后再学习一下vue项目的目录结构。



Vue-cli提供了如下2种方式创建vue项目:

①命令行：直接通过命令行方式创建vue项目

```
vue create vue-project01
```

![image-20240807154041205](assets/image-20240807154041205.png)



②图形化界面：通过命令先进入到图形化界面，然后再进行vue工程的创建

图形化界面如下：

![image-20240807153806258](assets/image-20240807153806258.png)







#### 2.1  创建Vue项目

①这里通过图形化界面创建项目，在Vue文件夹下进入cmd窗口界面，输入命令：vue ui

![image-20240807154359769](assets/image-20240807154359769.png)

②进入Vue图形化界面后，选择创建按钮，在vue文件夹下创建项目，如下图所示：

![image-20240807154532092](assets/image-20240807154532092.png)



③进行Vue项目的创建

<img src="assets/image-20240807154810765.png" alt="image-20240807154810765" style="zoom:50%;" />

④预设模板选择手动

<img src="assets/image-20240807154914738.png" alt="image-20240807154914738" style="zoom: 50%;" />



⑤功能页面开启路由功能

![image-20240807154958116](assets/image-20240807154958116.png)

⑥配置页面选择语言版本和语法检查规范

<img src="assets/image-20240807155036631.png" alt="image-20240807155036631" style="zoom:50%;" />

⑦配置不作为预设模板保存，点击创建项目

<img src="assets/image-20240807155058430.png" alt="image-20240807155058430" style="zoom:50%;" />

⑧只需要等待片刻，即可进入到创建创建成功的界面

<img src="assets/image-20240807155220978.png" alt="image-20240807155220978" style="zoom:50%;" />







#### 2.2 Vue项目结构

通过VSCode打开之前创建的vue文件夹，打开之后，呈现如下图所示页面：

<img src="assets/image-20240807155757680.png" alt="image-20240807155757680" style="zoom:50%;" />



Vue项目的标准目录结构以及目录对应的解释如下图所示:

(开发代码存放src目录下)

<img src="assets/image-20240807155831488.png" alt="image-20240807155831488" style="zoom:50%;" />







#### 2.3 运行Vue项目

Vue项目创建完后，提供了2种方式运行方式：

![image-20240807152736586](assets/image-20240807152736586.png)



①通过VSCode提供的图形化界面 （注意：NPM脚本窗口默认不显示，需手动设置）

<img src="assets/image-20240807160531618.png" alt="image-20240807160531618" style="zoom: 80%;" />

运行服务器后，可以直接通过浏览器打开地址

![image-20240807160704941](assets/image-20240807160704941.png)





此时默认访问的是index.html文件，默认引入了入口函数main.js文件，而该文件引用了 **src/App.vue**这个根组件

![image-20240807160812220](assets/image-20240807160812220.png)





> 补充：NPM脚本窗口调试出来
>
> ①通过**设置/用户设置/扩展/MPM**更改NPM默认配置
>
> ![image-20240807160310693](assets/image-20240807160310693.png)
>
> 
>
> ②双击打开项目的package.json文件，然后点击资源管理器处的3个小点，勾选npm脚本选项
>
> ![image-20240807160418572](assets/image-20240807160418572.png)



②命令行方式

直接基于cmd命令窗口，在vue目录下，执行输入命令`npm run serve`即可

![image-20240807161230123](assets/image-20240807161230123.png)







### 3.Vue项目运行流程

①**入口 HTML**：`public/index.html` 提供基础 HTML 结构。



②**入口 JS**：`src/main.js` 创建 Vue 实例，并将 `App.vue` 挂载到 `#app` 元素。

在vue项目中，index.html文件默认是引入了入口函数src/main.js文件，**src/main.js**文件其代码如下：

~~~js
import Vue from 'vue'
import App from './App.vue'
import router from './router'

Vue.config.productionTip = false

new Vue({
  router,
  render: h => h(App)
}).$mount('#app')
~~~

上述代码中，包括如下几个关键点：

- import: 导入指定文件，并且重新起名。例如上述代码`import App from './App.vue'`导入当前目录下得App.vue并且起名为App
- new Vue(): 创建vue对象
- $mount('#app');将vue对象创建的dom对象挂在到id=app的这个标签区域中，作用和之前学习的vue对象的el属性一致。
- router:  路由
- render: 主要使用视图的渲染的。



③**根组件**：`src/App.vue` 作为根组件，包含基本布局和 `<router-view>`。

vue创建的dom对象挂在到id=app的标签区域，接着就是首页内容呈现问题，这就涉及到render中的App

![image-20240807170054706](assets/image-20240807170054706.png)

对于App.vue的组件文件包含3个部分：

- template: 模板部分，主要是HTML代码，用来展示页面主体结构的
- script: js代码区域，主要是通过js代码来控制模板的数据来源和行为的
- style: css样式部分，主要通过css样式控制模板的页面效果得

如下图所示就是一个vue组件的小案例：

![image-20240807170043187](assets/image-20240807170043187.png)



④**路由配置**：`src/router/index.js` 配置路由，定义路径与组件的映射。

⑤**状态管理**：`src/store/index.js` 配置 Vuex 状态管理（如果使用）。

⑥**组件和视图**：`src/components/` 和 `src/views/` 存放各自的组件，构成应用的具体功能和界面。



我们可以简化模板部分内容，添加script部分的数据模型，删除css样式，完整代码如下：

~~~html
<template>
  <div id="app">
    {{message}}
  </div>
</template>

<script>
export default {
  data(){
    return {
      "message":"hello world"
    }
  }
}
</script>

<style>
</style>
~~~

保存后打开浏览器，可以发现首页展示效果发生了变化

![image-20240807164813401](assets/image-20240807164813401.png)





















## 四、Vue组件库Element

### 1.Element介绍

ElementUI就是一款侧重于V开发的前端框架，主要用于开发美观的页面的。
Element：是饿了么公司前端开发团队提供的一套基于 Vue 的网站组件库，用于快速构建网页。

Element 提供了很多组件（组成网页的部件）供我们使用。例如 超链接、按钮、图片、表格等等。

如下图所示就是我们开发的页面和ElementUI提供的效果对比，可以发现ElementUI提供的各式各样好看的按钮：

![image-20240807174512747](assets/image-20240807174512747.png)

官网地址：https://element.eleme.cn/#/zh-CN







### 2.快速入门

①首先，先安装ElementUI的组件库，打开VSCode，在命令行输入如下命令：

```
npm install element-ui@2.15.3 
```

具体操作如下图所示：

![image-20240807175500083](assets/image-20240807175500083.png)

<img src="assets/image-20240807175636372.png" alt="image-20240807175636372" style="zoom:50%;" />



②在main.js这个入口js文件中引入ElementUI的组件库，其代码如下：

```js
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';

Vue.use(ElementUI);
```

![image-20240807175805137](assets/image-20240807175805137.png)



③按照vue项目的开发规范，在**src/views**目录下创建一个vue组件文件，注意组件名称后缀是.vue，并且在组件文件中编写之前介绍过的基本组件语法，代码如下：

~~~html
<template>
</template>

<script>
export default {

}
</script>

<style>
</style>
~~~

具体操作如图所示：

![image-20240807180238021](assets/image-20240807180238021.png)



④到ElementUI的官网，找到组件库，然后找到按钮组件，抄写代码即可，具体操作如下图所示：

![image-20240807180400966](assets/image-20240807180400966.png)



⑤找到组件，紧接着将组件代码复制到我们的vue组件文件中，操作如下图所示：

![image-20240807180502935](assets/image-20240807180502935.png)



![image-20240807180517907](assets/image-20240807180517907.png)

```html
<template>
    <el-row>
        <el-button>默认按钮</el-button>
        <el-button type="primary">主要按钮</el-button>
        <el-button type="success">成功按钮</el-button>
        <el-button type="info">信息按钮</el-button>
        <el-button type="warning">警告按钮</el-button>
        <el-button type="danger">危险按钮</el-button>
      </el-row>
</template>

<script>
export default {

}
</script>

<style>
</style>
```



⑥在默认访问的根组件**src/App.vue**中引入我们自定义的组件，具体操作步骤如下：

![image-20240807181103886](assets/image-20240807181103886.png)

```html
<template>
  <div id="app">
    <!-- {{message}} -->
    <element-view></element-view>
  </div>
</template>

<script>
import ElementView from './views/Element/ElementView.vue'
export default {
  components: { ElementView },
  data(){
    return {
      "message":"hello world"
    }
  }
}
</script>
<style>

</style>
```



⑦运行Vue项目，打开浏览器展示效果如下图所示：

![image-20240807183016797](assets/image-20240807183016797.png)







### 3.Element组件

对于ElementUI的常用组件的使用，我们只需要参考官方提供的代码，然后复制粘贴即可。

#### 3.1 Table表格

Table 表格：用于展示多条结构类似的数据，可对数据进行排序、筛选、对比或其他自定义操作。



①在ElementUI的组件库中，找到表格组件，如下图所示：

![image-20240807204627179](assets/image-20240807204627179.png)



②复制代码到ElementVue.vue组件中，需要注意的是，我们组件包括了3个部分，如果官方有除了template部分之外的style和script都需要复制。具体操作如下图所示：

template模板部分：

![image-20240807205304499](assets/image-20240807205304499.png)



script脚本部分

![image-20240807205447679](assets/image-20240807205447679.png)



ElemetView.vue组件代码如下：

```html
<template>
  <div>
    <!-- Button -->
    <el-row>
      <el-button>默认按钮</el-button>
      <el-button type="primary">主要按钮</el-button>
      <el-button type="success">成功按钮</el-button>
      <el-button type="info">信息按钮</el-button>
      <el-button type="warning">警告按钮</el-button>
      <el-button type="danger">危险按钮</el-button>
    </el-row>
      
    <!-- Table -->
    <el-table
      :data="tableData"
      style="width: 100%">
      <el-table-column
        prop="date"
        label="日期"
        width="180">
      </el-table-column>
      <el-table-column
        prop="name"
        label="姓名"
        width="180">
      </el-table-column>
      <el-table-column
        prop="address"
        label="地址">
      </el-table-column>
    </el-table>

  </div>
</template>

<script>
export default {
  data(){
    return {
      tableData: [{
            date: '2016-05-02',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1518 弄'
          }, {
            date: '2016-05-04',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1517 弄'
          }, {
            date: '2016-05-01',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1519 弄'
          }, {
            date: '2016-05-03',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1516 弄'
          }]
    }
  }

}
</script>

<style>
</style>
```



③打开浏览器，页面呈现如下效果：

![image-20240807205548668](assets/image-20240807205548668.png)



> **组件属性详解**
>
> ElementUI是如何将数据模型绑定到视图的呢？主要通过如下几个属性：
>
> - data: 主要定义table组件的数据模型
> - prop: 定义列的数据应该绑定data中定义的具体的数据模型
> - label: 定义列的标题
> - width: 定义列的宽度
>
> 其具体示例含义如下图所示：
>
> ![image-20240807205828115](assets/image-20240807205828115.png)
>
> **PS:Element组件的所有属性都可以在组件页面的最下方找到**，如下图所示：
>
> ![image-20240807205853372](assets/image-20240807205853372.png)







#### 3.2 Pagination分页

Pagination: 分页组件，主要提供分页工具条相关功能。其展示效果图下图所示：

![image-20240807205947920](assets/image-20240807205947920.png)



①在ElementUI的组件库中，找到分页组件，选择带背景色分页组件，如下图所示：

![image-20240807210136355](assets/image-20240807210136355.png)



②复制代码到ElementView.vue组件文件的template中	

```html
<el-pagination
    background
    layout="prev, pager, next"
    :total="1000">
</el-pagination>
```



③打开浏览器，页面呈现如下效果：

<img src="assets/image-20240807211224965.png" alt="image-20240807211224965" style="zoom:80%;" />



>  组件属性详解
>
> 对于分页组件需要关注的是如下几个重要属性（可以通过查阅官网组件中最下面的组件属性详细说明得到）：
>
> - background: 添加北京颜色，也就是上图蓝色背景色效果。
> - layout: 分页工具条的布局，其具体值包含`sizes`, `prev`, `pager`, `next`, `jumper`, `->`, `total`, `slot` 这些值
> - total: 数据的总数量
>
> 根据官方分页组件提供的layout属性说明，如下图所示：
>
> ![image-20240807211424635](assets/image-20240807211424635.png)
>
> 修改layout属性如下：
>
> ```js
> layout="sizes,prev, pager, next,jumper,total"
> ```
>
> 添加了一些额外的功能，其具体对应关系如下图所示：
>
> ![image-20240807211508938](assets/image-20240807211508938.png)



> **组件事件详解**
>
> 对于分页组件，除了上述几个属性，还有2个非常重要的事件我们需要去学习：
>
> - size-change ： pageSize 改变时会触发 
> - current-change ：currentPage 改变时会触发 
>
> 其官方详细解释含义如下图所示：
>
> ![image-20240807211736265](assets/image-20240807211736265.png)
>
> 对于这2个事件的参考代码，同样可以通过官方提供的完整案例中找到，如下图所示：
>
> ![image-20240807211807950](assets/image-20240807211807950.png)
>
> 找到对应的代码，首先复制事件，复制代码如下：
>
> ~~~js
> @size-change="handleSizeChange"
> @current-change="handleCurrentChange"
> ~~~
>
> 此时Panigation组件的template完整代码如下：
>
> ~~~html
> <el-pagination
>   @size-change="handleSizeChange"
>   @current-change="handleCurrentChange"
>   background
>   layout="sizes,prev, pager, next,jumper,total"
>   :total="1000">
> </el-pagination>
> ~~~
>
> 接着需要复制事件需要的2个函数，需要注意methods属性和data同级，其代码如下：
>
> ~~~json
> methods: {
>       handleSizeChange(val) {
>         console.log(`每页 ${val} 条`);
>       },
>       handleCurrentChange(val) {
>         console.log(`当前页: ${val}`);
>       }
>     }
> ~~~
>
> Panigation组件的script完整代码如下：
>
> ```js
> <script>
> export default {
>     methods: {
>       handleSizeChange(val) {
>         console.log(`每页 ${val} 条`);
>       },
>       handleCurrentChange(val) {
>         console.log(`当前页: ${val}`);
>       }
>     },
>      data() {
>         return {
>           tableData: [{
>             date: '2016-05-02',
>             name: '王小虎',
>             address: '上海市普陀区金沙江路 1518 弄'
>           }, {
>             date: '2016-05-04',
>             name: '王小虎',
>             address: '上海市普陀区金沙江路 1517 弄'
>           }, {
>             date: '2016-05-01',
>             name: '王小虎',
>             address: '上海市普陀区金沙江路 1519 弄'
>           }, {
>             date: '2016-05-03',
>             name: '王小虎',
>             address: '上海市普陀区金沙江路 1516 弄'
>           }]
>         }
>       }
> }
> </script>
> ```
>
> 打开浏览器中，f12打开开发者控制台，然后切换当前页码和切换每页显示的数量，呈现如下效果：
>
> ![image-20240807212220374](assets/image-20240807212220374.png)





#### 3.3 Dialog对话框

Dialog: 在保留当前页面状态的情况下，告知用户并承载相关操作。



①在ElementUI的组件库中，找到Dialog组件，如下图所示：

![image-20240807212742214](assets/image-20240807212742214.png)



②template代码：

~~~html
<!--Dialog 对话框 -->
<!-- Table -->
<el-button type="text" @click="dialogTableVisible = true">打开嵌套表格的 Dialog</el-button>

<el-dialog title="收货地址" :visible.sync="dialogTableVisible">
    <el-table :data="gridData">
        <el-table-column property="date" label="日期" width="150"></el-table-column>
        <el-table-column property="name" label="姓名" width="200"></el-table-column>
        <el-table-column property="address" label="地址"></el-table-column>
    </el-table>
</el-dialog>
~~~

script代码：

~~~JavaScript
 gridData: [{
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }],
        dialogTableVisible: false,
~~~

完整代码如下：

~~~html
<script>
export default {
    methods: {
      handleSizeChange(val) {
        console.log(`每页 ${val} 条`);
      },
      handleCurrentChange(val) {
        console.log(`当前页: ${val}`);
      }
    },
     data() {
        return {
        gridData: [{
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }],
        dialogTableVisible: false,
        tableData: [{
            date: '2016-05-02',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1518 弄'
          }, {
            date: '2016-05-04',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1517 弄'
          }, {
            date: '2016-05-01',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1519 弄'
          }, {
            date: '2016-05-03',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1516 弄'
          }]
        }
      }
}
</script>
~~~



③打开浏览器，页面呈现如下效果：

![image-20240808114503285](assets/image-20240808114503285.png) 



> **组件属性详解**
>
> ElementUI是如何做到对话框的显示与隐藏的呢？是通过如下的属性：
>
> - visible.sync ：是否显示 Dialog 
>
> 具体释意如下图所示：
>
> ![image-20240808114657958](assets/image-20240808114657958.png) 
>
> visible属性绑定的dialogTableVisble属性一开始默认是false，所以对话框隐藏；然后我们点击按钮，触发事件，修改属性值为true，然后对话框visible属性值为true，所以对话框呈现出来。
>







#### 3.4 Form表单

Form 表单：由输入框、选择器、单选框、多选框等控件组成，用以收集、校验、提交数据。 



①在ElementUI的组件库中，找到Form组件，如下图所示：

![1669369751014](assets/1669369751014.png) 



 ②template代码：

~~~html
<el-dialog title="Form表单" :visible.sync="dialogFormVisible">
            
            <el-form ref="form" :model="form" label-width="80px">
                <el-form-item label="活动名称">
                    <el-input v-model="form.name"></el-input>
                </el-form-item>
                <el-form-item label="活动区域">
                    <el-select v-model="form.region" placeholder="请选择活动区域">
                    <el-option label="区域一" value="shanghai"></el-option>
                    <el-option label="区域二" value="beijing"></el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="活动时间">
                    <el-col :span="11">
                    <el-date-picker type="date" placeholder="选择日期" v-model="form.date1" style="width: 100%;"></el-date-picker>
                    </el-col>
                    <el-col class="line" :span="2">-</el-col>
                    <el-col :span="11">
                    <el-time-picker placeholder="选择时间" v-model="form.date2" style="width: 100%;"></el-time-picker>
                    </el-col>
                </el-form-item>
            
                <el-form-item>
                    <el-button type="primary" @click="onSubmit">立即创建</el-button>
                    <el-button>取消</el-button>
                </el-form-item>
            </el-form>
        </el-dialog>
~~~

观察上述代码，我们发现其中表单项标签使用了v-model双向绑定，所以我们需要在vue的数据模型中声明变量，同样可以从官方提供的代码中复制粘贴，但是我们需要去掉我们不需要的属性。

通过观察上述代码，我们发现双向绑定的属性有4个，分别是form.name,form.region,form.date1,form.date2,所以最终数据模型如下：

![image-20240808120736026](assets/image-20240808120736026.png)

script-data函数：

~~~javascript
form: {
         name: '',
         region: '',
         date1: '',
         date2:''
},
~~~

同时在template可以发现创建按钮绑定了一个 onSubmit 事件，同样，在官方的script代码中，也提供了onSubmit函数，表单的立即创建按钮绑定了此函数，我们可以输入表单的内容，而表单的内容是双向绑定到form对象的，所以我们修改官方的onSubmit函数如下即可，而且我们还需要关闭对话框，最终函数代码如下：

![image-20240808121102637](assets/image-20240808121102637.png) 

script-onSubmit事件函数：

~~~javascript
onSubmit() {
       console.log(this.form); //输出表单内容到控制台
       this.dialogFormVisible=false; //关闭表案例的对话框
}
~~~



③打开浏览器，页面呈现如下效果：

![image-20240808121446644](assets/image-20240808121446644.png) 



完整代码如下：

~~~html
<template>
  <div>
  <!-- Button按钮 -->
      <el-row>
          <el-button>默认按钮</el-button>
          <el-button type="primary">主要按钮</el-button>
          <el-button type="success">成功按钮</el-button>
          <el-button type="info">信息按钮</el-button>
          <el-button type="warning">警告按钮</el-button>
          <el-button type="danger">危险按钮</el-button>
      </el-row>

      <!-- Table表格 -->
      <el-table
      :data="tableData"
      style="width: 100%">
          <el-table-column
              prop="date"
              label="日期"
              width="180">
          </el-table-column>
          <el-table-column
              prop="name"
              label="姓名"
              width="180">
          </el-table-column>
          <el-table-column
              prop="address"
              label="地址">
          </el-table-column>
      </el-table>

      <br>
      <!-- Pagination分页 -->
      <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          background
          layout="sizes,prev, pager, next,jumper,total"
          :total="1000">
      </el-pagination>

      <br><br>
      <!--Dialog 对话框 -->
      <!-- Table -->
      <el-button type="text" @click="dialogTableVisible = true">打开嵌套表格的 Dialog</el-button>

      <el-dialog title="收货地址" :visible.sync="dialogTableVisible">
      <el-table :data="gridData">
          <el-table-column property="date" label="日期" width="150"></el-table-column>
          <el-table-column property="name" label="姓名" width="200"></el-table-column>
          <el-table-column property="address" label="地址"></el-table-column>
      </el-table>
      </el-dialog>

      <br><br>
      <!-- Dialog对话框-Form表单 -->
      <el-button type="text" @click="dialogFormVisible = true">打开嵌套Form的 Dialog</el-button>

      <el-dialog title="Form表单" :visible.sync="dialogFormVisible">
          
          <el-form ref="form" :model="form" label-width="80px">
              <el-form-item label="活动名称">
                  <el-input v-model="form.name"></el-input>
              </el-form-item>
              <el-form-item label="活动区域">
                  <el-select v-model="form.region" placeholder="请选择活动区域">
                  <el-option label="区域一" value="shanghai"></el-option>
                  <el-option label="区域二" value="beijing"></el-option>
                  </el-select>
              </el-form-item>
              <el-form-item label="活动时间">
                  <el-col :span="11">
                  <el-date-picker type="date" placeholder="选择日期" v-model="form.date1" style="width: 100%;"></el-date-picker>
                  </el-col>
                  <el-col class="line" :span="2">-</el-col>
                  <el-col :span="11">
                  <el-time-picker placeholder="选择时间" v-model="form.date2" style="width: 100%;"></el-time-picker>
                  </el-col>
              </el-form-item>
          
              <el-form-item>
                  <el-button type="primary" @click="onSubmit">立即创建</el-button>
                  <el-button>取消</el-button>
              </el-form-item>
          </el-form>
      </el-dialog>
  </div>
</template>

<script>
export default {
  methods: {
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
    },
    //表单案例的提交事件
    onSubmit() {
      console.log(JSON.stringify(this.form)); //输出表单内容到控制台
      this.dialogFormVisible=false; //关闭表案例的对话框
    }
  },
   data() {
      return {
      //表单案例的数据双向绑定
      form: {
        name: '',
        region: '',
        date1: '',
        date2:''
      },
      gridData: [{
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-04',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-01',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }],
      dialogTableVisible: false,
      dialogFormVisible: false, //控制form表单案例的对话框
      tableData: [{
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1517 弄'
        }, {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1519 弄'
        }, {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1516 弄'
        }]
      }
    }
}
</script>

<style>
</style>
~~~




















































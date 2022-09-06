# VUE

#app：id选择器

.app：class选择器



## 本地应用

### v-text

设置标签的内容

```html
<!DOCTYPE html>
<html>
<head>
  <title>My first Vue app</title>
  <script src="https://unpkg.com/vue@2"></script>
</head>
<body>
  <div id="app">
    <h2 v-text="message+'!'"></h2>
    <h2 v-text="info">啊啊啊</h2>
    <h2>{{message}}啊啊啊</h2>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    var app = new Vue({
      el:"#app",
      data:{
        message:"12345622",
        info:"前端"
      }
    })
  </script>

</body>
</html>
```

![image-20220712101500883](C:\Users\coshi\AppData\Roaming\Typora\typora-user-images\image-20220712101500883.png)

### v-html

可以解析html语言格式

```html
<!DOCTYPE html>
<html>
<head>
  <title>My first Vue app</title>
  <script src="https://unpkg.com/vue@2"></script>
</head>
<body>
  <div id="app">
    <p v-html="content"></p>
    <p v-text="content"></p>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    var app = new Vue({
    el:"#app",
    data:{
      content:"<a href='www.baidu.com'>史浚申</a>"
    }
  })
  </script>

</body>
</html>
```

![image-20220712102210634](C:\Users\coshi\AppData\Roaming\Typora\typora-user-images\image-20220712102210634.png)



### v-on

为元素绑定事件，方法写在Vue对象中的method中

```html
<!DOCTYPE html>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<html>
<head>
  <title>My first Vue app</title>
  <script src="https://unpkg.com/vue@2"></script>
</head>
<body>
  <div id="app">
    <input type="button" value="v-on" v-on:click="doIt">
    <input type="button" value="v-on简写" @click="doIt">
    <input type="button" value="双击事件" @dblclick="doIt">
    <h2 @click="changeFood">{{food}}</h2>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    var app = new Vue({
      el:"#app",
      data:{
        food:"西兰花炒蛋"
      },
      methods:{
        doIt:function(){
          alert("做It");
        },
        changeFood:function(){
          console.log(this.food);
          this.food += "好吃";
        }
      }
    })
  </script>
  
</body>
</html>
```



**补充：**

传递自定义参数，时间修饰符

```html
<!DOCTYPE html>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<html>
<head>
  <title>My first Vue app</title>
  <script src="https://unpkg.com/vue@2"></script>
</head>
<body>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  
  <div id="app">
    <input type="button" value="点击" @click="doIt(666)">
    <input type="text" name="吃了没" @keyup.enter="sayHi">
  </div>

  <script>
    var app = new Vue({
      el:"#app",
      methods:{
        doIt:function(p1){
          console.log("做it");
          console.log(p1);
        },
        sayHi:function(){
          alert("Hi")
        }
      }
    })
  </script>
</body>
</html>
```





### 案例：计数器

```html
<!DOCTYPE html>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<html>
<head>
  <title>My first Vue app</title>
  <script src="https://unpkg.com/vue@2"></script>
</head>
<body>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  
  <div id="app">
    <button @click="add">+</button>
    <span>{{ message }}</span>
    <button @click="sub">-</button>
  </div>

  <script>
    var app = new Vue({
      el:"#app",
      data:{
        message:0
      },
      methods:{
        add:function(){
          if(this.message<10)
            this.message += 1;
          else
            alert("到顶了");
        },
        sub:function(){
          if(this.message>0){
            this.message -= 1;
          }
          else{
            alert("到底了");
          }
            
        }
      }
    })
  </script>

</body>
</html>
```



### v-show

根据表达式的真假，切换元素的显示和隐藏

```html
<!DOCTYPE html>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<html>
<head>
  <title>My first Vue app</title>
  <script src="https://unpkg.com/vue@2"></script>
</head>
<body>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  
  <div id="app">
    <input type="button" value="切换显示状态" @click="changeIsShow">
    <img v-show="isShow" src="C:\Users\coshi\OneDrive - Calix, Inc\Desktop\1.jpg" alt="">
    <img v-show="age>=18" src="C:\Users\coshi\OneDrive - Calix, Inc\Desktop\1.jpg" alt="">
  </div>
  <script>
    var app = new Vue({
      el:"#app",
      data:{
        isShow:true,
        age:19
      },
      methods:{
        changeIsShow:function(){
          this.isShow = !this.isShow;
        }
      },
    })
  </script>

</body>
</html>
```



### v-if

根据表达式的真假，切换元素的显示和隐藏（操纵dom元素）

**与v-show不同的是：**if操作的是dom树（直接移除变成<!---->），show操作的是样式style，所以show的效率更高一些



### v-bind

设置元素的属性（比如：src，title，class）

```html
<!DOCTYPE html>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<html>
<head>
  <title>My first Vue app</title>
  <script src="https://unpkg.com/vue@2"></script>
  <style>
    .active{
      border:1px solid red;
    }
  </style>
</head>
<body>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  
  <div id="app">
    <img v-bind:src="imgSrc" alt="">
    <br>
    <img :src="imgSrc" alt="" :title="imgTitle+'!!!'" 
    :class="isActive?'active':''" @click="toggleActive">
    <!-- :class="{active:isActive}" :使用对象的方式来写-->
  </div>

  <script>
    var app = new Vue({
      el:"#app",
      data:{
        imgSrc:"C:/Users/coshi/OneDrive - Calix, Inc/Desktop/1.jpg",
        imgTitle:"图片",
        isActive:false
      },
      methods:{
        toggleActive:function(){
          this.isActive = !this.isActive;
        }
      }
    })
  </script>
</body>

</html>
```





### v-for

```html
<!DOCTYPE html>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<html>
<head>
  <title>My first Vue app</title>
  <script src="https://unpkg.com/vue@2"></script>
</head>
<body>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  
  <div id="app">
    <input type="button" value="add" @click="add">
    <input type="button" value="remove" @click="remove">
    <ul>
      <li v-for="item,index in arr">
        序号:{{item}} {{index}}
      </li>
    </ul>
    <h2 v-for="item in vegetables" v-bind:title="item.name">
      {{item.name}}
    </h2>
  </div>
  <script>
    var app = new Vue({
      el:"#app",
      data:{
        arr:[1,2,3,4],
        vegetables:[
          {name:"西兰花炒蛋"},
          {name:"蛋炒西兰花"}
        ]
      },
      methods:{
        add:function(){
          this.vegetables.push({name:"花菜炒蛋"})
        },
        remove:function(){
          this.vegetables.shift();
        }
      }
    })
  </script>

</body>
</html>
```



### v-model

获取和设置表单元素的值（双向数据绑定）

```html
<!DOCTYPE html>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<html>
<head>
  <title>My first Vue app</title>
  <script src="https://unpkg.com/vue@2"></script>
</head>
<body>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  
  <div id="app">
    <input type="text" v-model="message" @keyup.enter="getM">
    <input type="button" value="小名" @click="setM">
    <h2>{{message}}</h2>
  </div>

  <script>
    var app = new Vue({
      el:"#app",
      data:{
        message:"史浚申"
      },
      methods:{
        getM:function(){
          alert(this.message);
        },
        setM:function(){
          this.message = "石头";
        }
      }
    })
  </script>
</body>
</html>
```



## 网络应用

### Axios

功能强大的网络请求库

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

格式：第二个回调函数是请求出错的时候触发
axios.get(地址?key=value&key2=value2).then(function(response){},function(err){})
axios.post(地址{key:value,key2:value2}).then(function(response){},function(err){})
```







## Vue-element-admin

- 拉取基础项目模板

会在目录下生成shop文件夹存放模板

```shell
git clone https://github.com/PanJiaChen/vue-admin-template.git /home/corey/vue/vue-element-template
git clone https://github.com/PanJiaChen/vue-element-admin.git /home/corey/vue/vue-element-admin
```



- 安装项目依赖

进入vue-element-template/vue-element-admin文件夹里执行安装命令

```shell
# 建议不要用 cnpm 安装 会有各种诡异的bug 可以通过如下操作解决 npm 下载速度慢的问题
npm install --registry=https://registry.npm.taobao.org
```



- 启动项目

```shell
npm run dev
```





# 问题

- invalid host header

[(3条消息) 解决vue-cli脚手架访问出现“Invalid Host header”_new-lijiabin的博客-CSDN博客](https://blog.csdn.net/lijiabinbbg/article/details/89452449?ops_request_misc=%7B%22request%5Fid%22%3A%22166078811916782391826728%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=166078811916782391826728&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-89452449-null-null.142^v41^pc_rank_34_1,185^v2^control&utm_term=invalid host header vue cli 2&spm=1018.2226.3001.4449)


























































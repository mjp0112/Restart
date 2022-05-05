# VUE day3 

引入vue

```html
<script src="https://cdn.bootcdn.net/ajax/libs/vue/2.6.11/vue.min.js"></script>
```



vue.js 页面模板

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue的入门</title>
    <script src="../js/vue.js"></script>
</head>
<body>
    <div id="app">
        <div>{{message}}</div>
    </div>
    <!--
        使用vue获取div中的内容，以及设置div中的内容
        使用vue的具体步骤:
            1. 指定一个区域，在该区域中使用vue
            2. 在该区域下方编写一个script标签，在script标签中创建Vue对象
            3. 以json格式给Vue对象传入需要传入的参数
                1. "el" 表示哪块区域可以使用vue
                2. "data" 表示你需要定义的数据模型
            4. 将message这个数据模型绑定给div的标签体: 使用插值表达式进行绑定
    -->
    <script>
        var vue = new Vue({
            "el":"#app",//作用域
            
            //数据域
            "data":{
                "message":"hello vue" //定义了一个数据模型，该数据模型的名字叫"message"
            },
            // 方法域
            "methods":{
            
            }
        });
    </script>
</body>
</html>

```



### 绑定

v-bind

```html
    <div id="app">
        <!--
            目标使用vue绑定输入框的内容(其实就是input的value属性)
            绑定属性的方式:    v-bind:属性名 = "要绑定的数据模型"
            它还可以省略v-bind简写成 :属性名 = "要绑定的数据模型"
        -->
        <input type="text" name="username" :value = "username"/>
        
        
        <!--
            双向绑定
				v-model:
			去掉前后空格
				v-model.trim
        -->
        <input type="text" name="username" v-model:value = "username"/>
        
    </div>
    <script>
        var vue = new Vue({
            "el":"#app",
            "data":{
                "username":"张三疯"
            }
        });
    </script>
```



### 条件渲染

```html
v-if="数据模型名"  //true显示
v-else="数据模型名"   //必须搭配v-if使用，若果数据为false则显示


v-show="数据模型名"  //标签仍然存在，底层使用css实现
```

#### 列表渲染

```html
   <div id="app">
        <ul>
            <!--
                使用v-for进行绑定:
                    1. v-for写在哪里? 你遍历出来一个数据就要添加一个什么标签，那么v-for就写在哪里
                    2. v-for的值写什么? (遍历出来的内容,下标) in 要遍历的数据模型
            -->
            <li v-for="(fruit,i) in fruitList" :value="i">{{fruit}}</li>
        </ul>
    </div>
    <script>
        var vue = new Vue({
           "el":"#app",
           "data":{
               "fruitList": [
                   "apple",
                   "banana",
                   "orange",
                   "grape",
                   "dragonfruit"
               ]
           }
        });
    </script>


```



### 事件驱动

```html
<div id="app">
        <!--目标1:点击按钮，就将上方的字符串反转-->
        <!--
            v-on:事件名="函数" 这就是vue绑定事件
            它还可以简写成 @事件名="函数"

            vue中声明函数是写在"methods"的键值对的值里面
            语法  "函数名":function(参数列表){函数体},
            它还可以简写成 函数名(参数列表){函数体}
        -->
        <!--
            目标2: 鼠标在div中移动的时候，获取鼠标所在位置的坐标
                1. 要给div绑定鼠标移动事件 @mousemove="函数"
                2. 编写一个函数记录鼠标移动时候的X轴和Y轴坐标

                我们需要注意的是:
                    1. 绑定事件的时候如果直接写 @mousemove="函数名"默认就会将当前事件传给函数
                    2. 绑定事件的时候如果写成 @mousemove="函数名($event)"才能将当前事件传给函数
        -->
        <div style="width: 400px;height: 400px;border: 1px solid black" @mousemove="recordPosition($event)">{{message}}</div>
        <button @click="reverseMessage()">反转字符串</button>
    </div>
    <script>
        var vue = new Vue({
           "el":"#app",
           "data":{
               "message":"Hello Vue!"
           },
           "methods":{
               "reverseMessage":function () {
                    //实现message的反转
                   //将字符串切割成一个数组
                   var arr = this.message.split("");
                    //然后将数组中的元素反转
                   arr = arr.reverse()
                   //再将反转过后的数组拼接成一个新的字符串,赋值给message，那么就能反转div的内容了
                   this.message = arr.join("")
               },
               recordPosition(event){
                    //event表示当前触发的事件
                   //获取当前事件触发时候的x轴的坐标
                   var clientX = event.clientX;

                   //获取当前事件触发时候的Y轴的坐标
                   var clientY = event.clientY;

                   console.log(clientX+","+clientY)
               }
           }
        });
    </script>
```






# VUE day1 html和css

![image-20211017210533410](E:\md\毕设\前端\image-20211017210533410.png)



## html

Hyper Text Markup Language，超文本标记语言；



### 目录结构

![image-20210919111251077](C:\Users\54060\AppData\Roaming\Typora\typora-user-images\image-20210919111251077.png)





### html布局:

​	![image-20210919125946347](C:\Users\54060\AppData\Roaming\Typora\typora-user-images\image-20210919125946347.png)





### 标题

​	<h1>......<h6> 标题大小

### 段落

​	<p> </p>



### 换行标签

<br/>



### 无序列表标签

```html
<ul>
    <li>Apple</li>
    <li>Banana</li>
    <li>Grape</li>
</ul>
```

面效果

<img src="VUE day1 html和css.assets/img035.png" style="zoom:50%;" />









### 图像：

#### 	1.图像标签和属性：

```html
<img src="url" alt="some_text">  //没有闭合标签，alt为预备可替换文本

//超链接
<a href="../01_html的入门/start.html">跳转到start.html页面</a><br/>
```



### 块标签

span style="color:blue;font-weight:bold;">『块』</span>并不是为了显示文章内容的，而是为了方便结合CSS对页面进行布局。块有两种，div是前后有换行的块，span是前后没有换行的块。

把下面代码粘贴到HTML文件中查看他们的区别：

```html
<div style="border: 1px solid black;width: 100px;height: 100px;">This is a div block</div>
<div style="border: 1px solid black;width: 100px;height: 100px;">This is a div block</div>

<span style="border: 1px solid black;width: 100px;height: 100px;">This is a span block</span>
<span style="border: 1px solid black;width: 100px;height: 100px;">This is a span block</span>
```

<img src="VUE day1 html和css.assets/img038.png" style="zoom:50%;" />



## CSS



CSS有三种方式标记：

​	1.id；  ==单独标注==  用#标识

​	2.class； ==表示同一类==	用.标识

​	3.直接使用标签

​	

​	在<head>里面<style>里用相应的css样式来对html进行编辑。

```html
<style type="text/css">
		#title1 {
			font-size: 2em;
			text-align: center;
		}
		
		.comment{
			font-size: 0.8em;
			text-align: center;
		}
		
		img {
			width:500px;
			display: block;
			margin: auto;
		}
		
		#lianxi{
			color: burlywood;
			width: 500px;
			height: 500px;
			background-color: antiquewhite;
			margin: 50px auto 50px auto;  //居中
			/* 上 右 下 左 */
		}
	</style>
```





## JSON

JS中一切结尾对象；所以支持通过JSON来表示，例如字符串、数字、对象等：

- 对象表示为键值对
- 数据由逗号分割
- 花括号保存对象
- 方括号保存数组

![image-20220505175545993](VUE day1 html和css.assets/image-20220505175545993.png)


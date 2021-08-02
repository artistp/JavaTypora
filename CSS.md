HTML+CSS+JavaScript

结构+表现+交互

# 1. 什么是CSS

如何学习

1. CSS是什么
2. CSS怎么用
3. **CSS选择器（重要+难点）**
4. 美化网页（文字，阴影，超链接，列表，渐变）
5. 盒模型
6. 浮动
7. 定位
8. 网页动画（特效效果）

## 1.1 什么是CSS

Cascading Style Sheet 层叠级联样式表

CSS：表现（美化网页）

## 1.2 CSS发展史

CSS1.0

CSS2.0 DIV（块）+CSS, HTML与CSS结构分离的思想，网页变得简单，SEO

CSS2.1 浮动  定位

CSS3.0 圆角 阴影 动画   浏览器兼容性

## 1.3 快速入门

style

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--规范，<style>可以编写CSS代码，每一个声明，最好用分号结尾
    语法：
        选择器{
            声明1;
            声明2;
            声明3;
        }
    -->
    <style>
        h1{
            color: red;
        }
    </style>
</head>
<body>
<h1>kuaile</h1>
</body>
</html>


<!--引用外部的CSS文件-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
<h1>kuaile</h1>
</body>
</html>
```

**CSS优势：**

- 内容和表现分离
- 网页结构表现统一，可以实现复用
- 样式十分的丰富
- 建议使用独立于HTML的CSS文件
- 利用SEO，容易被搜索引擎收录

## 1.4 CSS的3种导入方式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <!--2.内部样式-->
    <style>
        h1{
            color: red;
        }
    </style>
  
    <!--3.外部样式-->
    <link rel="stylesheet" href="style.css">
</head>
<body>
<!--优先级：就近原则 谁离代码最近谁生效-->
<!--1. 行内样式-->
<h1 style="color: red"></h1>

</body>
</html>
```

扩展：外部样式两种写法

- 链接式：HTML

```html
<!--3.外部样式-->
<link rel="stylesheet" href="style.css">
```

- 导入式：

```html
<!--3.外部样式：导入式 CSS2.0-->
<style>
    @import url("style.css");
</style>
```

# 2.选择器

**作用：选择页面上的某一个或者某一类原始**

## 2.1基本选择器

1. 标签选择器
2. 类选择器 class
3. ID选择器

```html
1.标签选择器
h1{
    color: red;
}

2.类选择器
.kuaile1{
    color: green;
}
.kuaile2{
    color: #00f7ff;
}
<!--class可以复用-->
<h1 class="kuaile1">快乐1</h1>
<h1 class="kuaile2">快乐2</h1>
<h1 class="kuaile1">快乐3</h1>

3.ID选择器
#id1{
    color: bisque;
}
<!--ID必须全局唯一-->
<h1 id="id1" class="kuaile1">标题1</h1>
<h1 >标题2</h1>
<!--优先级：ID选择器>class选择器>标签选择器-->
```

## 2.2层次选择器

1. 后代选择器

	```html
	<style>
	  /*后代选择器
	  对body标签下的所有标签生效
	  */
	    body p{
	        background: red;
	    }
	</style>
	```

	

2. 子选择器 一代 儿子

	```html
	<style>
	        /*子选择器*/
	        body>p{
	            background: #00f7ff;
	        }
	</style>
	```

	

3. 相邻兄弟选择器

	```html
	<style>
	        /*相邻兄弟选择器 只有一个，相邻（向下）*/
	        .active+p{
	            background: deepskyblue;
	        }
	</style>
	```

	

4. 通用选择器

	```html
	    <style>
	        /*通用选择器，当先元素向下的所有兄弟元素*/
	        .active~p{
	            background: deepskyblue;
	        }
	    </style>
	```

## 2.3结构 伪类选择器

```html
<style>

  p:hover{
    
  }

  ul li:last-child{
       background: red;
  }
</style>
```



## 2.4属性选择器

```html
<style>
  /*包含有 id属性的a标签*/
  a[id]{
    
  }
  /*id属性为first的a标签*/
  a[id=first]{
    
  }
  /*class中含有links的元素*/
  a[class*="links"]{
    
  }
  /*href属性以http开头的元素*/
  a[href^=http]{
    
  }
  /*href属性以pdf结尾的元素*/
  a[href$=pdf]{
    
  }
</style>

= 相等
*= 包含
^= 以...开头的
$= 以...结尾的
```

# 3.美化网页






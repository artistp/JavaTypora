## W3C标准

- **结构**化标准语言（HTML XML）
- **表现**标准语言（CSS）
- **行为**标准（DOM, ECMAScript）

```html
<!--标题标签-->
<h1>一级标签</h1>
<h2>二级标签</h2>
<h3>三级标签</h3>
<h4>四级标签</h4>
<h5>五级标签</h5>

<!--段落标签-->
<p></p>

<!--换行标签-->
<br/>

<!--水平线标签-->
<hr/>

<!--字体标签-->
<strong></strong>  <!--粗体标签-->
<em></em> <!--斜体标签-->

<!--特殊符号-->
空格：&nbsp;
大于号：&gt;
小于号：&lt;
版权符号：&copy;
```



## **图片标签**

![image-20210520161806371](E:\TyporaImg\image-20210520161806371.png)

****



##**链接标签**

![image-20210520162310744](E:\TyporaImg\image-20210520162310744.png)

```html
<!--锚链接
1.需要一个锚标记
2.跳转到标记
-->
<a name="top">顶部</a>
<a href="#top">回到顶部</a>

<!--功能性链接
邮件链接：maito:
QQ链接 在QQ的官网差链接

-->
<a href="maito:artistp@163.com">点击联系我</a>
```



****

## **块元素**

- 无论内容多少，该元素独占一行
- （p, h1~h6标签）

## **行内元素**

- 内容撑开宽度，左右都是行内元素的可以排在一行
- （a strong em标签）

---------

## **列表**

```html
<!--有序列表-->
<ol>
  <li></li>
  <li></li>
  <li></li>
</ol>

<!--无序列表:导航栏，侧边栏等-->
<ul>
  <li></li>
  <li></li>
  <li></li>
</ul>

<!--自定义列表
dl: 标签
dt: 列表名称
dd: 列表内容
公司网站底部等
-->
<dl>
  <dt>学科</dt>
  <dd></dd>
  <dd></dd>
  <dd></dd>
  
  <dt>学科</dt>
  <dd></dd>
  <dd></dd>
  <dd></dd>
</dl>
```

--------

## **表格**

```HTML
<!--表格 table
行 tr
列 td
-->
<table>
  <tr>
    <!--colspan 跨列-->
  	<td colspan='4'></td>
  </tr>
  
  <tr>
  	<td rowspan='2'></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  
  <tr>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>
```

------------------------

## **音频视频**

```HTML
<!--视频
src: 资源路径
controls: 控制条
autoplay: 自动播放
-->
<video src="" controls autoplay></video>

<audio src="" controls autoplay></audio>
```

------

## **网站的结构**

![image-20210520164714881](E:\TyporaImg\image-20210520164714881.png)

------

## **iframe**

```HTML
<!--iframe内联框架
src: 地址

-->
<iframe src="www.baidu.com" name="hello" frameborder="0" width"1000px" heigth="800px"></iframe>


<iframe src="" name="hello" frameborder="0" width"1000px" heigth="800px"></iframe>
<a href="www.baidu.com" target="hello">点击跳转</a>
```

![image-20210520165523667](E:\TyporaImg\image-20210520165523667.png)

-------

## **表单语法**

![image-20210520165658795](E:\TyporaImg\image-20210520165658795.png)



------

## 文本框和单选框

![image-20210520170117706](E:\TyporaImg\image-20210520170117706.png)

```HTML
<p>
  性别：
  <input type="radio" value="boy" name="gender" checked/>男
  <input type="radio" value="girl" name="gender"/>女
</p>
```

-------

## 多选框

```HTML
<p>
  性别：
  <input type="checkbox" value="sleep" name="hobby"/>睡觉
  <input type="checkbox" value="code" name="hobby" checked/>代码
  <input type="checkbox" value="game" name="hobby"/>游戏
  <input type="checkbox" value="chat" name="hobby"/>聊天
</p>
```

-------

## 按钮

```html
<!--按钮
input type="button" 普通按钮
input type="image" 图像按钮
input type="submit" 提交按钮
input type="reset" 重置
-->
<p>
  按钮：
  <input type="button" name="btn1" value="  "/>睡觉
  <input type="image" src=" "/>代码
</p>
```

--------

## 下拉框

```HTML
<!--下拉框

-->
<p>
  下拉框：
  <select name="列表名称">
    <option value="" selected></option>
    <option value=""></option>
    <option value=""></option>
    <option value=""></option>
    <option value=""></option>
  </select>
</p>
```

------

## 文本域、文件域

```html
<!--文本域
cols="50" rows="10"
-->
<p>
  反馈：
  <textarea name="textarea" cols="50" rows="10">文本内容</textarea>
</p>

<!--文件域
-->
<input type="file" name="files"/>
```

----------------

## 邮件验证

```html
<p>
  邮箱：
  <input type="email" name="email"/>
</p>

<p>
  URL：
  <input type="url" name="url"/>
</p>

<p>
  数字：
  <input type="number" name="num" max="100" min="0" step="1"/>
</p>

<p>
  滑块：
  <input type="range" name="voice" max="100" min="0" step="2"/>
</p>

<p>
  滑块：
  <input type="search" name="search"/>
</p>
```

-------------------

## 表单的应用

```html
隐藏域 hidden
只读 readonly
禁用 disable
placeholder 提示信息
required 非空判断
pattern 正则表达式
<p>
  邮箱：
  <input type="text" name="diymail" pat/>
</p>
```










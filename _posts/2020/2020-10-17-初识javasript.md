---
layout:     post   				    # 使用的布局（不需要改）
title:      '初识javascript'			# 标题 
date:       '2020-10-17'			# 时间
author:     meguriri						# 作者
categories: javascript
tags:		
- javascript						#标签
excerpt: 'JavaScript是一种具有函数优先的轻量级，解释型或即时编译型的高级编程语言。它是作为开发Web页面的脚本语言而出名的，与HTML，css并称为前端三件套，JavaScript通常被嵌入到HTML文件中，不过也可以通过node.js在本地终端运行。'
---
JavaScript是一种具有函数优先的轻量级，解释型或即时编译型的高级编程语言。它是作为开发Web页面的脚本语言而出名的，与HTML，css并称为前端三件套，JavaScript通常被嵌入到HTML文件中，不过也可以通过node.js在本地终端运行。
## 环境配置
### node.js的下载：
  首先需要下载node.js
  >网址：[node.js](https://nodejs.org/zh-cn/)

  ![node.js](https://ftp.bmp.ovh/imgs/2020/10/cf76f87b52d199b0.png)

按照步骤安装后，可以在命令提示符中输入：node -v来查看是否安装成功。如果见到版本号则说明安装成功，如下图。
![node-v](https://i.bmp.ovh/imgs/2020/10/c4a2b2d12a58414f.png)

之后就可以在vs code上愉快的写javasccript了！！
### 推荐插件：
在vs code上有一个十分好用的插件**Quokka.js**，他可以通过快捷键
<kbd>ctrl</kbd>+<kbd>k</kbd>+<kbd>j</kbd>来快速创建一个js文件，并且实时输出结果。

![quokka](https://i.bmp.ovh/imgs/2020/10/94cfaa05dcd1ae49.png)

## 语句与输出
javascript是一种动态语言。每一句javascrpit可以值、运算符、表达式、关键词和注释组成。注释与c++一样可以用//或/**/来表示。大括号{}中表示的是一个代码块。你可以用关键字<font color=orange>var</font>来声明一个变量，
并通过<font color=orange>console.log()</font>在终端输出。例如：

``` javascript
//这是一条注释
var a=3,b=4;
console.log(a+b);
/*代码块*/
{
  console.log("hello wolrd");
}
```

注意变量的命名不要与JavaScript的关键字相同。
下面列出一部分javascript的关键词：

|关键词|描述|
|:---|:---|
|break|	终止 switch 或循环。|
|continue	|跳出循环并在顶端开始。|
|debugger	|停止执行 JavaScript，并调用调试函数（如果可用）。|
|do ... while	|执行语句块，并在条件为真时重复代码块。|
|for	|标记需被执行的语句块，只要条件为真。|
|function	|声明函数。|
|if ... else	|标记需被执行的语句块，根据某个条件。|
|return	|退出函数。|
|switch	|标记需被执行的语句块，根据不同的情况。|
|try ... catch	|对语句块实现错误处理。|
|var	|声明变量。|

## 变量类型
JavaScript一共有五种类型，字符串值(string)，数值(number)，布尔值(bool)，数组(array)，对象(object)。想要知道一个变量的类型可以通过函数<font color=orange>typeof()</font>查看，该函数将返回函数类型的字符串。
### 1.字符串 string
字符串是一串字符，被一对''或""包围，当其他变量与字符串相加时将返回字符串，注：（js表达式为从左向右）。同时字符串也有很多方法。

``` javascript
var p1="wangruikang",p2='yangzheng';
console.log(typeof(p1));//    string
console.log(p1+20);//  wangruikang20
console.log(20+p1);//  20wangruikang
console.log(20+5+p2);//  25yangzheng
console.log(p2+25+0);// yangzheng250
```

#### 特殊字符 
由于字符串必须由引号包围，当字符串中有引号的时候JavaScript会误解这段字符串。为避免此问题，可以使用**转义字符\\**,下面给出转义字符的常见用法：

|代码|结果|描述|
|:--|:--|:--|
|\'	|'	|单引号|
|\"	|"	|双引号|
|\\\\	|\	|反斜杠|
|\b|	|退格键|
\f	||换页|
\n	||新行|
\r	||回车|
\t	||水平制表符|
\v	||垂直制表符|

### 2.数值(number)
javascript的数值没有整形与浮点型之分，既可以带小数点，也可以不带，过大的数也可以用科学计数法来写，小数的最大位为17位，但是不总是100%精确。




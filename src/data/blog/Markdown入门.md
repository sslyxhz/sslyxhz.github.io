---
author: sslyxhz
pubDatetime: 2016-05-23T12:13:24Z
modDatetime: 2016-05-24T09:09:06Z
title: Markdown入门
slug: markdown-course
featured: false
draft: false
tags:
  - color-schemes
description:
  Markdown的目标是易读易写。在此简单记录markdown的使用语法。
---


# 标题
使用井号+空格标注标题级别。
```
# 1级标题
## 2级标题
```

# 字体
```
# 粗体字，主要用作强调
**粗体字**

# 斜体字，主要作为书名等
*斜体字*
```
效果如下：

**粗体字**

*斜体字*

# 换行
要在文本插入换行符号，需要在行末插入两个以上的空格再回车。

# 列表
## 无序列表
对无序列表来说+、-、*的效果相同。
```
- Item1
+ Item2
* Item3
```
效果如下：
- Item1
+ Item2
* Item3

## 有序列表
前缀加上1.，需要有空格。
```
1. Item1
2. Item2
```
1. Item1
2. Item2

# 引用
## 一般引用
引用，前缀符号为 >  ，可以嵌套使用。

> 引用内容1
>> 引用内容1-1  

> 引用内容2

## 代码
代码框，符号为 `

```
`  some code `
```
效果如下：
`  some code `

## 链接
```
[链接文字](链接地址)  
[图片链接](https://i2.hdslb.com/bfs/face/a6f243d8374c623271cad03f0c105ae76e47a004.jpg)  
![图片](https://i2.hdslb.com/bfs/face/a6f243d8374c623271cad03f0c105ae76e47a004.jpg)  
```
[图片链接](https://i2.hdslb.com/bfs/face/a6f243d8374c623271cad03f0c105ae76e47a004.jpg)  
![图片](https://i2.hdslb.com/bfs/face/a6f243d8374c623271cad03f0c105ae76e47a004.jpg)  

# 表格
```
| Tables        | Are           | Cool  |
|      ----     |  :----:       |  ----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |
```
效果如下：  

| Tables        | Are           | Cool  |
|      ----     |  :----:       |  ----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

# 分割线
下述符号都能被解释为分割线。  

```
* * *
***
*****
- - -
---------------------------------------
```

# 作图
绘制顺序图和流程图需要有插件支持。
## 顺序图
```
Alice->Bob: Hello Bob, how are you?
Note right of Bob: Bob thinks
Bob-->Alice: I am good thanks!
```

```sequence
Alice->Bob: Hello Bob, how are you?
Note right of Bob: Bob thinks
Bob-->Alice: I am good thanks!
```
## 流程图
```
st=>start: Start|past:>http://www.google.com[blank]
e=>end: End:>http://www.google.com
op1=>operation: My Operation|past
op2=>operation: Stuff|current
sub1=>subroutine: My Subroutine|invalid
cond=>condition: Yes
or No?|approved:>http://www.google.com
c2=>condition: Good idea|rejected
io=>inputoutput: catch something...|request

st->op1(right)->cond
cond(yes, right)->c2
cond(no)->sub1(left)->op1
c2(yes)->io->e
c2(no)->op2->e
```

```flow
st=>start: Start|past:>http://www.google.com[blank]
e=>end: End:>http://www.google.com
op1=>operation: My Operation|past
op2=>operation: Stuff|current
sub1=>subroutine: My Subroutine|invalid
cond=>condition: Yes
or No?|approved:>http://www.google.com
c2=>condition: Good idea|rejected
io=>inputoutput: catch something...|request

st->op1(right)->cond
cond(yes, right)->c2
cond(no)->sub1(left)->op1
c2(yes)->io->e
c2(no)->op2->e
```
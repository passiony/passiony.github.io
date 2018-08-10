---
layout:     post
title:      "Markdown Tutorial"
subtitle:   "markdown"
date:       2018-08-10
author:     "Passion"
header-img: "img/post-bg-js-version.jpg"
tags:
    - markdown
    - tutorial
---

## Foreword

MarkDown 语法是当下非常流行的文本标记语言，它语法简便排版优美，深受有文字处理和排版需求的人士的喜爱。如当下各种博客也支持MarkDown语法编辑博客，著名的分布式系统社群 github 也用的MarkDown语法来让用户写 readme 文件。可见MarkDown语法在当下的使用是非常多的。

简单说说 MarkDown 语法

---

## Catalog


1.  [标题 Headers](#headers)
2.  [文字斜体和加粗 Bold&Italic](#bold--italic)
3.  [列表 List](#list)
4.  [图片 Image](#image)
5.  [连接 Links](#links)
6.  [引用：Blockquotes](#blockquotes)
7.  [内联代码 inline code](#inline--code)
8.  [多行内联代码 Multi-line code](#multi--line--code)
9.  [删除线 Strikethrough](#strikethrough)
10. [横向分割线 Horizontal Rules](#horizontal rules)
11. [MarkDown的注释](#markdown)
12. [表格 Form](#form)



## 标题 Headers

Example：
```
# 我是1级标题 ---> <h1>标签
## 我是2级标题 ---> <h2>标签
### 我是3级标题 ---> <h3>标签
···
###### 我是6级标题 ---> <h6>标签
```

Result：

# 我是1级标题 ---> <h1>标签
## 我是2级标题 ---> <h2>标签
### 我是3级标题 ---> <h3>标签
···
###### 我是6级标题 ---> <h6>标签
···


## 文字斜体和加粗 Bold&Italic

Example：
```
*这样会是斜体*
_这样也会是斜体（下划线）_

**这样会是粗体**
__这样也是粗体__

*当然 **粗体** 和 _斜体_ 是可以混合使用的*
```

快捷键
CMD + U 这个是underline命令就是添加下划线
CMD + I 这个是italic命令就是斜线
CMD + B 这个是bold命令就是加粗

Result：

*这样会是斜体*
_这样也会是斜体（下划线）_

**这样会是粗体**
__这样也是粗体__

*当然 粗体 和 斜体 是可以混合使用的*



## 列表 List

语法：* + 空格 或者数组+点+空格

### 1.无序的列表
Example：
```
* 我是第1条
* 我是第2条
* 我是第3条
* 我是第4条
```
Resule：

* 我是第1条
* 我是第2条
* 我是第3条
* 我是第4条

### 2.有序的列表
Example：（有序的，如果有子项也是一样tab+序号+.+空格）
```
1. 我是第1条
    1. 我是第1条的第1条子项
    2. 我是第1条的第2条子项
2. 我是第2条
3. 我是第3条
    1. 我是第3条的第1条子项
        1. 我是第3条的第1条子项的子项
        2. 我是第3条的第2条子项的子项
    2. 我是第3条的第2条子项
4. 我是第4条
```

Resule：

1. 我是第1条
    1. 我是第1条的第1条子项
    2. 我是第1条的第2条子项
2. 我是第2条
3. 我是第3条
    1. 我是第3条的第1条子项
        1. 我是第3条的第1条子项的子项
        2. 我是第3条的第2条子项的子项
    2. 我是第3条的第2条子项
4. 我是第4条

### 3.复选框列表
Example：
```
- [ ] 这是未选中的复选框` ‘-’ + ‘空格’ + ‘[中间有空格]’ `
- [x] 这是选中的复选框` ‘-’ + ‘空格’ + ‘[中间有空格]’ `
```

Result：

- [ ] 这是未选中的复选框` ‘-’ + ‘空格’ + ‘[中间有空格]’ `
- [x] 这是选中的复选框` ‘-’ + ‘空格’ + ‘[中间有空格]’ `



## 图片 Image

Example：
格式: ![Alt Text](url)
快捷键: Control + Shift + I

```
实例1：![GitHub set up](https://help.github.com/assets/images/site/set-up-git.gif)
实例2：![GitHub set up](https://help.github.com/assets/images/site/set-up-git.gif =200x300)
实例3：![GitHub set up](https://help.github.com/assets/images/site/set-up-git.gif =200x)
实例4：<img class="shadow" width="200x" src="/img/in-post.png" />
```

Result：

![GitHub set up](https://help.github.com/assets/images/site/set-up-git.gif =200x)



## 连接 Links

Example:
邮箱、网址和自动链接的 三种格式：
```
email链接 <example@example.com>
[GitHub链接](http://github.com)
自动链接  <http://www.github.com/>
```
快捷键: Control + Shift + L

Result:

email链接 <example@example.com>
[GitHub链接](http://github.com)
自动链接  <http://www.github.com/>

随意的一个 URL (例如 http://www.github.com/) 将会自动转换成一个可点击的链接.



## 引用：Blockquotes

Example:
```
如晓友所说:
> 每天进步一点
> 好好生活，天天向上.
```

快捷键: 选中要变成引用的那句话按 CMD + Shift + B
格式： 格式就是 大于号 >

Result:

如晓友所说:
> 每天进步一点
> 好好生活，天天向上.



## 内联代码 inline code

Example:
```
有时候会使用到 小的单行的 代码块
`<addr>` `code` 如这种的.
```
快捷键: CMD + K

Result:

有时候会使用到 小的单行的 代码块
`<addr>` `code` 如这种的.



## 多行内联代码 Multi-line code

Example:

        ```js
        function fancyAlert(arg) {
            if(arg) {
                $.facebox({div:'#foo'})
            }
        }
        ```

快捷键: CMD + Shift + K

Result:

```js
    function fancyAlert(arg) {
        if(arg) {
            $.facebox({div:'#foo'})
        }
    }
```



## 加删除线 Strikethrough

Example:
```
(就像 ~~这样~~)
```

Result:

(就像 ~~这样~~)



## 横向分割线 Horizontal Rules

以下格式都会生成下划线
```
***
*****
- - -
```

Result:

***

*****

---



## MarkDown的注释

格式： <!-- comment -->
快捷键：CMD + .

## 表格 Form

Example：
```
  |名字|性别|年龄|国籍|
  |:-|:-|:-|:-|
  |张三|男|23|中国|
  |小红|女|18|中国|
  |Tom|男|46|美国|
```
Result：

|名字|性别|年龄|国籍|
|:-|:-|:-|:-|
|张三|男|23|中国|
|小红|女|18|中国|
|Tom|男|46|美国|


  ---

# 专注内容创作，使用VScode来发布知乎文章

本文目录：  
- [专注内容创作，使用VScode来发布知乎文章](#专注内容创作使用vscode来发布知乎文章)
	- [一、为什么要用markdown？](#一为什么要用markdown)
	- [二、VSCode使用Zhihu插件](#二vscode使用zhihu插件)
		- [1. 内容发布](#1-内容发布)
		- [2. 上传图片的三种方式](#2-上传图片的三种方式)
		- [3. 在知乎文章使用链接卡片](#3-在知乎文章使用链接卡片)
	- [三、Markdown基本语法](#三markdown基本语法)
	- [四、VSCode的快捷操作](#四vscode的快捷操作)

## 一、为什么要用markdown？
markdown是一种语言，而不是二进制文件格式，Markdown 实际上是 HTML 的一种简写，在显示时会『解压缩』成 HTML。可以纯文本读和写，相比word或者其他富文本工具，更专注于创作，简洁规范的排版更易读，生成的md文件也小得多。


## 二、VSCode使用Zhihu插件
[插件介绍地址，插件作者——牛岱 ](https://zhuanlan.zhihu.com/p/106057556)

### 1. 内容发布
在问题下或者更新自己的文章, 复制到文件顶部  


>#! +网址
>新版本发布会默认加上文章链接
杀杀杀杀杀杀杀杀杀
水水水水水水  

**ps：第一个一级标题会被当做文章标题**   
发布按钮如下:  
![20200708204605](https://raw.githubusercontent.com/Achuan-2/Passage/master/images/20200708204605.png)  
![20200708204650](https://raw.githubusercontent.com/Achuan-2/Passage/master/images/20200708204650.png)


### 2. 上传图片的三种方式
支持上传`.gif` `.png`, `.jpg`, 到知乎图床

1.  从粘贴板上传图片
调用`Zhihu:PasteImage`命令,将系统剪贴板的图片上传
 2. 工作区中右键上传
复制文件路径,然后再调用 `Zhihu: PasteImageFromPath`
3.  打开文件浏览器选择图片
`Zhihu: Upload Image From Explore`

上传的图片会上传到知乎图床，在md文件里以图片链接形式展示
```
![这里放图片描述](https://pic4.zhimg.com/80/v2-1092f2ea535280eb16427573f01b4b49.png)  
```
>建议将文字内容处理之后,再上传图片 , 可以先在要添加图片的地方添加描述，再集中上传
>**ps:第一张图片会作为知乎文章的背景图**  

### 3. 在知乎文章使用链接卡片
[在 VSCode 下用 Markdown Preview Enhanced 愉快地写文档 - Aya Magician的文章 - 知乎](https://zhuanlan.zhihu.com/p/56699805 "card")

```
[在 VSCode 下用 Markdown Preview Enhanced 愉快地写文档 - Aya Magician的文章 - 知乎](https://zhuanlan.zhihu.com/p/56699805 "card")

在超链接后加上"card"
```
---  


## 三、Markdown基本语法
 
 想要换行，在VSCode至少需要在行后**打两个空格** 然后回车，或者回车两次  
 如果觉得这个设定不习惯，可以在设置搜索`preview` ,勾选  `markdown>preview: breaks`, 就可得到enter换行效果啦!
 好奇怪，为什么原来的能够正常显示，然而放到Github却不能正常换行？
 > 注意这个只是预览效果改变，然而上传到其他网站依然还是需要两个空格换行的！
![20200708204845](https://raw.githubusercontent.com/Achuan-2/Passage/master/images/20200708204845.png)

* ### 多级标题
Markdown使用`#`的个数来表示级别  
> 一级标题只能有一个，就是文档标题。而文档正文中的一级标题应该用二级 Markdown 标题。

* ### 文本格式化
加粗、斜体、加粗斜体
>`*斜体*  **加粗斜体**  ***加粗斜体***`      
>*斜体*  **加粗斜体**  ***加粗斜体***

删除线  
>` ~~删除线~~`  
>~~删除线~~  
>怎么好像不支持<s>删除线</s>

* ### 无序列表和有序列表
*/-+space 无序列表

**数字+.** 便是有序列表啦  
这应该和许多富文本软件一样

* ### 超链接和图片
markdown使用`[链接显示文字](链接地址) `来表示超链接, 

图片呢, 其实和超链接非常相似, 用`![]()`表示图片, 就是用链接来表示图片

* ### 代码块
使用 \` \`号来引用代码, 一个代表单行, 三个代表多行

`单行代码样式`

``` python
多行代码样式
```


* ### 水平分割线

使用---来表示分割线


* ### 表格
  ~~Zhihu插件不支持表格~~~~

```
|   日期   |   待办事项   |           完成情况           |
| :------: | :----------: | :--------------------------: |
| 2020.7.5 | 了解markdown | 半途看《乘风破浪的小姐姐》了 |
```

---

## 四、VSCode的快捷操作

* 对于 行 的操作：  
	• 重开一行：光标在行尾的话，回车即可；不在行尾，ctrl + enter 向下重开一行；ctrl+shift + enter 则是在上一行重开一行  
	• 删除一行：光标没有选择内容时，ctrl + x 剪切一行；ctrl +shift + k 直接删除一行  
	• 移动一行：alt + ↑ 向上移动一行；alt + ↓ 向下移动一行  
  • 复制一行：shift + alt + ↓ 向下复制一行；shift + alt + ↑ 向上复制一行  
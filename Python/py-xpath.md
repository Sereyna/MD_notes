# Xpath
---
>XPath 是一门在 XML 文档中查找信息的语言。XPath 可用来在 XML 文档中对元素和属性进行遍历。
XPath 使用路径表达式来选取 XML 文档中的节点或者节点集。这些路径表达式和我们在常规的电脑文件系统中看到的表达式非常相似。

## 1. Xpath节点
在 XPath 中，有七种类型的节点：元素、属性、文本、命名空间、处理指令、注释以及文档节点（或称为根节点）。

例如：
```xml
<?xml version="1.0" encoding="ISO-8859-1"?>

<bookstore>（文档节点）

<book>
  <title lang="en"（属性节点）>Harry Potter</title>
  <author>J K. Rowling</author> （元素节点）
  <year>2005</year>
  <price>29.99</price>
</book>

</bookstore>
```

**节点关系**

父节点、子节点、同胞、先辈节点、后代节点这五种关系

## 2. Xpath语法
XPath 使用路径表达式来选取 XML 文档中的节点或节点集。节点是通过沿着路径 (path) 或者步 (steps) 来选取的。


## 3. Xpath Helper
`Xpath Helper` 是一款谷歌应用商店推出的免费工具，下载完毕后，谷歌浏览器会将其作为插件自动安装在扩展程序中。

安装完毕后，在需要匹配数据的页面处，使用快捷键打开助手工具（快捷键：ctrl+shift+x）

将鼠标悬停在需要选取数据的文本上，并按下`shift`按键就会自动出现 Xpath 表达式，然后再根据自己的需求对表达式稍微修改即可。

![xpath-helper](py-img/xpathhelper.png "Xpath Helper正反匹配皆可")

谷歌开发者调试工具也内置了 Xpath 表达式匹配功能，首先打开调试工具，在下方的调试工作区内使用快捷键ctrl+F打开 Xpath 匹配功能，==只能正向匹配==，如下图所示：

![xpathchrome](py-img/xpathchrome.png "Xpath Helper正向匹配")

下载链接：

>云盘链接：https://pan.baidu.com/s/18LcxOCLqALlob33UybTATA
提取码：eo1m


【引用】
Xpath W3C文档 https://www.w3school.com.cn/xpath/xpath_syntax.asp
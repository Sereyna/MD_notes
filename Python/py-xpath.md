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

## 2.Xpath语法
XPath 使用路径表达式来选取 XML 文档中的节点或节点集。节点是通过沿着路径 (path) 或者步 (steps) 来选取的。


【引用】
Xpath W3C文档 https://www.w3school.com.cn/xpath/xpath_syntax.asp
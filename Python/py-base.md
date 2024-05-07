# Python 基础语法
---
## 1. 变量类型
**dict和json的区别**
字典（Dictionary）和JSON是两种不同的数据结构，它们之间的主要区别如下：

1. 数据结构不同：
字典是一个键-值对（key-value）集合，键必须是唯一的且不可变。在Python中，字典是一个无序的数据结构。
JSON 是一种轻量级的数据交换格式，它依赖于特定的键值对来组织数据，其中键必须是字符串，值可以是字符串、数字、对象、数组等。JSON 是一种有序的数据结构。
2. 键的类型不同：
字典的键只能是不可变的类型，如整数、浮点数、字符串、元组。
JSON的键总是字符串。
3. 数据的表现形式：
字典在Python中表现为花括号 {} 包围的键值对集合。
JSON 是以文本形式（即字符串形式）表现的，文本形式的JSON可以在不同的编程语言中交换数据。
4. 嵌套数据结构：
字典可以嵌套字典、列表等Python原生数据类型。
JSON只能通过数组和对象来嵌套数据，对应Python中的列表和字典。
5. 序列化和反序列化：
字典不是自描述的，需要额外的信息来进行序列化和反序列化。
JSON 是自描述的，可以直接通过工具或库进行序列化和反序列化。
6. 使用场景：
字典主要在编程语言内部使用，如Python、JavaScript等。
JSON 主要用于跨应用程序、语言交换数据。
### 1.1 类型转换

**任何类型转String**

```python
string = f'{anytype}'
```

**String转list**

```python
# string = "['我加入的社区', '我管理的社区', '官方推荐社区', '其他社区']"
listtype = list(eval(string))
```
**String转json**
```python
# 假设你有一个已经是JSON格式的字符串
json_str = '{"name": "John", "age": 30, "city": "New York"}'

# 使用json.loads将字符串解析成Python字典
data = json.loads(json_str)

# 美化输出
# 使用json.dumps将字典转换回JSON格式的字符串，并以美观的形式输出
pretty_json_str = json.dumps(data, indent=4)
print(pretty_json_str)

# 如果需要保存到文件
with open('pretty_json.json', 'w') as f:
    json.dump(data, f, indent=4)  # 默认会将中文转换成十六进制
    json.dump(text_json, f, indent=4, ensure_ascii=False)  # 将中文原样输出
```

**dict转json**
```py
import json
 
dictionary = {'name': 'John', 'age': 30, 'city': 'New York'}
json_string = json.dumps(dictionary)
print(json_string)  # 输出: {"name": "John", "age": 30, "city": "New York"}
 
# JSON字符串转字典
dictionary_from_json = json.loads(json_string)
print(dictionary_from_json)  # 输出: {'name': 'John', 'age': 30, 'city': 'New York'}
```

### 1.2 字典（dictionary）
在Python中，如果你想获取字典的第一个键值对
```py
my_dict = {'a': 1, 'b': 2, 'c': 3}
first_key, first_value = next(iter(my_dict.items()))
print(first_key, first_value)
```
这段代码使用了`items()`方法来获取字典中的键值对（以元组形式），然后使用`next()`函数来获取第一个键值对。`iter()`函数用于创建一个迭代器。
## 2. 读写文件

### 2.1 读写csv文件

**json数组写入csv文件**

```python
import json
import csv
 
# 假设json_array是你的JSON数组字符串
json_array = '[{"name": "John", "age": 30, "city": "New York"}, {"name": "Anne", "age": 25, "city": "Chicago"}]'
 
# 将JSON数组字符串解析成Python列表
data = json.loads(json_array)
 
# 打开CSV文件进行写入
with open('output.csv', 'w', newline='', encoding='utf-8') as csvfile:
    writer = csv.writer(csvfile)
    
    # 写入标题行
    writer.writerow(data[0].keys())
    
    # 写入数据
    for item in data:
        writer.writerow(item.values())
```

### 2.2 读写Json文件
```python
# 读
# 编码可以不设置
with open('data.json', 'r', encoding='utf-8') as file:
    data = json.load(file)

# 写
with open('pretty_json.json', 'w') as f:
    json.dump(data, f, indent=4)  # 默认会将中文转换成十六进制
    json.dump(text_json, f, indent=4, ensure_ascii=False)  # 将中文原样输出
```
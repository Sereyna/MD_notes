# Python 基础语法
---
## 1. 类型转换

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
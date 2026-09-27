# 楚河脚本：JSON 数据格式命令

本文件记录楚河脚本中用于读取文本 JSON、创建 JSON 对象、访问 JSON 数组、访问 JSON 对象以及获取 JSON 字符串值的命令。

---

## 1. 从文本创建String对象

### 功能

从文本文件中读取内容，并创建一个 String 对象。

### 语法

```text
从文本创建String对象("String变量名", 文本路径)
```

### 示例

```text
path = "c:/楚河脚本/资源目录/保存json字符串的文本.txt"

从文本创建String对象("str1", path)
```

创建后，可以使用 `str1` 作为 String 对象传递给 JSON 相关命令。

---

## 2. json_创建对象

### 功能

使用 String 对象中的 JSON 字符串创建 JSON 对象。

### 语法

```text
json_创建对象("json变量名", String变量名)
```

### 示例

```text
json_创建对象("json1", str1)
```

---

# JSON 对象读取

## 3. json_获取数组

### 功能

从 JSON 对象中获取指定字段对应的 JSON 数组。

### 语法

```text
json_获取数组("json数组名", json变量名, "字段名")
```

### 示例

```text
json_获取数组("arr1", json1, "words_result")
```

获取后，`arr1` 可以继续用于 JSON 数组相关命令。

---

## 4. json_获取数组长度

### 功能

获取 JSON 数组中的元素数量。

### 语法

```text
length = json_获取数组长度(json数组名)
```

### 示例

```text
length = json_获取数组长度(arr1)

println("length的值是: ", length)
```

---

## 5. json_数组里获取对象

### 功能

从 JSON 数组中按照下标获取一个 JSON 对象。

### 语法

```text
json_数组里获取对象(json对象名, json数组名, 下标)
```

### 示例

```text
json_数组里获取对象(tempObj, arr1, i)
```

---

## 6. json_对象里获取String

### 功能

从 JSON 对象中获取指定字段对应的 String 对象。

### 语法

```text
json_对象里获取String("String对象名", json对象名, "字段名")
```

### 示例

```text
json_对象里获取String("stringObj", tempObj, "words")
```

获取后，可以通过 `获取String对象()` 将 String 对象转换为普通字符串使用。

---

## 7. 获取String对象

### 功能

获取 String 对象中的实际字符串内容。

### 语法

```text
resultStr = 获取String对象(String对象名)
```

### 示例

```text
resultStr = 获取String对象(stringObj)

println(resultStr)
```

---

# 完整示例

## 示例1：读取 JSON 对象中的嵌套字段

假设文本中保存以下 JSON：

```text
{"words_result":[{"chars":[],"location":{"top":4,"left":12,"width":73,"height":25},"words":"单选题"}],"direction":0,"words_result_num":1,"log_id":6143331651545}
```

示例代码：

```text
path = "c:/楚河脚本/资源目录/保存json字符串的文本.txt"

从文本创建String对象(stringObj, path)
json_创建对象(json1, stringObj)

json_获取数组(arr1, json1, "words_result")
json_数组里获取对象(tempObj, arr1, 0)
json_对象里获取对象(locationObj, tempObj, "location")

json_对象里获取String(widthObj, locationObj, "width")
json_对象里获取String(heightObj, locationObj, "height")

widthStr = 获取String对象(widthObj)
heightStr = 获取String对象(heightObj)

println(widthStr)
println(heightStr)
```

### 数据访问过程

上述代码的 JSON 数据结构相当于：

```text
json1
└── words_result
    └── [0]
        └── location
            ├── width
            └── height
```

访问过程：

```text
json_获取数组
→ json_数组里获取对象
→ json_对象里获取对象
→ json_对象里获取String
→ 获取String对象
```

---

## 示例2：循环读取 JSON 数组

假设文本中保存以下 JSON：

```text
{"words_result":[{"words":"西瓜要怎么切才会好看"},{"words":"海洋里的可爱动物"},{"words":"青春永远是珍贵的"}],"words_result_num":3,"log_id":51516518566632}
```

示例代码：

```text
path = "c:/楚河脚本/资源目录/保存json字符串的文本.txt"

从文本创建String对象(stringObj, path)
json_创建对象("json1", stringObj)

json_获取数组("arr1", json1, "words_result")

length = json_获取数组长度(arr1)

println("length的值是: ", length)

i = 0
循环 i 小于 length
	println("第", i, "条")

	json_数组里获取对象(tempObj, arr1, i)
	json_对象里获取String(tempString, tempObj, "words")

	resultStr = 获取String对象(tempString)

	println(resultStr)
	println()

	i = i + 1
循环完成
```

运行后可以依次读取 `words_result` 数组中的每一个 `words` 字段。

---

# AI 使用注意事项

1. `从文本创建String对象()` 是从文本文件创建 String 对象。
2. `json_创建对象()` 使用 String 对象中的 JSON 字符串创建 JSON 对象。
3. `json_获取数组()` 用于从 JSON 对象中获取数组。
4. `json_获取数组长度()` 用于获取 JSON 数组长度。
5. `json_数组里获取对象()` 用于根据数组下标获取 JSON 对象。
6. `json_对象里获取对象()` 用于从 JSON 对象中继续获取嵌套 JSON 对象。
7. `json_对象里获取String()` 用于从 JSON 对象中获取 String 对象。
8. `获取String对象()` 用于取得 String 对象中的实际字符串内容。
9. JSON 命令中的变量名、字段名和下标必须按照实际 JSON 数据结构使用。
10. 不要把 JSON 字符串、String 对象、JSON 对象和 JSON 数组当成同一种数据类型。
11. 教程中的 `json_数组里获取对象("String对象名", json数组名, "字段名")` 命令介绍属于文字错误；实际对应的命令是 `json_对象里获取String()`，正式文档以实际演示和命令定义为准。
12. 如果需要访问 JSON 中嵌套的对象，应使用 `json_对象里获取对象()` 继续向下访问。
13. 如果需要读取数组中的对象，应先使用 `json_获取数组()`，再使用 `json_数组里获取对象()`。
14. 不得根据其他 JSON 库或其他编程语言自行创造楚河脚本 JSON 命令。

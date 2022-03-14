---
title: CMake基本常用语法CMakeLists.txt
date: 2022-03-14 17:57
categories: [语言, C\C++]
tags: [C/C++]     # TAG names should always be lowercase
---

## Hello, World
~~~cmake
cmake_minimum_required(VERSION 3.5) # 指定CMake的版本
project(hello-world) # 项目名称
add_executable(hello helloworld.cpp) # 创建一个可执行程序

~~~

## 基本语法

### 文件编码-Encoding
[官方说明文档:文件编码](https://cmake.org/cmake/help/latest/manual/cmake-language.7.html#id15)
+ 在3.0以下的版本，CMake文件必须使用7-bit的ASCII编码，在3.0以后可以使用UTF-8编码
+ 文件的换行符必须使用`\n`或`\r\n`

### 注释-Comments

[官方说明文档:Comments1](https://cmake.org/cmake/help/latest/manual/cmake-language.7.html#id24)
[官方说明文档:Comments2](https://cmake.org/cmake/help/latest/manual/cmake-language.7.html#id25)

#### 单行注释
~~~cmake
test # comment
~~~

#### 多行注释
~~~makefile
# 最简单的多行注释
test #[[comment]]
# 注释两边的`=`个数要相等
test #[={len}[comment]={len}]
~~~

### 字符串
[官方说明文档:字符串](https://cmake.org/cmake/help/latest/manual/cmake-language.7.html#id20)
1. 字符串的格式为`string`
2. 对于多行字符串，可以在行尾添加`\`来忽略换行符
   ```makefile
   ### Example-1
   message("12
   34")
   # 12
   # 34
   ### Example-2
   message("12\
   34")
   # 1234

   ```

3. 使用`[={len}[string]={len}]`可以禁止字符串转义；左右两边的`=`个数需要相等
   ```makefile
   ### Example-1
   message([=[string\n]=])
   # string\n
   ### Example-2
   message("string\n")
   # string
   #
   ### Example-3
   message([==[string]=]) # ERROR: 左右两边的=个数不相等
   ### Example-4
   message([=[string]==]) # ERROR: 左右两边的=个数不相等
   ### Example-5
   message([==========[string]==========]) # OK: 左右两边的=个数相等
   # string

   ```

### 变量设置-set
[官方说明文档:设置变量](https://cmake.org/cmake/help/latest/command/set.html#command:set)
```makefile
set(<variable> <value>
       [[CACHE <type> <docstring> [FORCE]] | PARENT_SCOPE])

```
1.设置普通变量

```makefile
set(var0 "string")
```

2.设置数组

```makefile
set(arr0 "item0;item0;item2")
set(arr1 item0 item1 item2)
set(arr2 ${arr0} ${arr1})
set(arr3 "${arr0};${arr1}")
# 使用'\ '来转义空格
set(arr4 item0 item1\ item2)
foreach(item ${arr4})
	message(${item})
endforeach()
# item0
# item1 item2
# 使用'\;'来转义分号
set(arr5 "item0;item0\;item2")
foreach(item ${arr5})
	message(${item})
endforeach()
# item0
# item1;item2

```

3.设置环境变量

```makefile
set($ENV{TEST_ENV_VARIABLE} 233)
```

### 取消设置变量-unset

[官方说明文档:取消设置变量](https://cmake.org/cmake/help/latest/command/unset.html#command:unset)

```makefile
unset(<variable> [CACHE | PARENT_SCOPE])
```

+ 将变量、缓存变量或环境变量改为未定义状态

---
title: _tmain()和main()
date: 2022-04-13 15:30
categories: [语言, C\C++]
tags: [C/C++]     # TAG names should always be lowercase
---

首先，这个_tmain()是为了支持unicode所使用的main一个别名而已。

既然是别名，应该有宏定义过的，在哪里定义的呢？就在那个让你困惑的<stdafx.h>里，有这么两行：

\#include <stdio.h>

\#include <tchar.h>

我们可以在头文件<tchar.h>里找到_tmain的宏定义   

\#define  _tmain  main

所以，经过预编译以后， _tmain就变成main了



main()是标准C++的函数入口。标准C++的程序入口点函数,默认字符编码格式ANSI

函数签名为：

int main();

int main(int argc, char* argv[]);



_tmain()是windows提供的对unicode字符集和ANSI字符集进行自动转换用的程序入口点函数

函数签名为:

int _tmain(int argc, TCHAR *argv[])

(1) 当你程序当前的字符集为unicode时，int _tmain(int argc, TCHAR *argv[])会被翻译成

int wmain(int argc, wchar_t *argv[])

//wmain也是main的另一个别名,是为了支持二个字节的语言环境

(2) 当你程序当前的字符集为ANSI时，int _tmain(int argc, TCHAR *argv[])会被翻译成

int main(int argc, char *argv[])

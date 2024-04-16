---
title:  Shell 编程简单实战
comments: 
categories: [doc]
tags: 
  - 语言
date: 2024-04-01 16:46:38
---

[TOC]

Shell 编程是一种在 Unix 或类 Unix 系统中操作命令行的脚本语言，它提供了一种自动化执行任务的方式。本文将介绍 Shell 编程的基础知识和常用技巧。

## 1. 注释与输出

在 Shell 脚本中，注释以 `#` 开头，用于解释代码或添加备注。输出内容通常使用 `echo` 命令。

```bash
#!/bin/bash

# 输出 Hello
echo "Hello"
```
## 2. 变量与字符串操作
Shell 中的变量使用方式类似于其他编程语言，可以存储数据和进行操作。字符串操作包括拼接、获取长度、截取等。


```bash
name="YunDinger"
echo "Hello, $name"
echo ${#name} # 输出字符串长度
```
## 3. 数组操作
Shell 支持数组，可以存储多个值。通过 ${array[@]} 或 ${array[*]} 获取数组的所有元素，${#array[@]} 获取数组长度。

```bash
arr=(1 2 3 4 5)
echo ${arr[@]}
echo ${#arr[@]}
````
## 4. 流程控制
Shell 支持条件语句（if...else）、循环语句（for、while）等流程控制结构，用于根据条件执行不同的代码块或者重复执行某段代码。

```bash
if [ condition ]; then
    # do something
elif [ condition ]; then
    # do something
else
    # do something
fi

for i in {1..5}; do
    # do something
done

n=0
while (( n < 5 )); do
    # do something
    (( n++ ))
done
```
## 5. 函数
Shell 函数可以封装一些逻辑以便重复使用，类似于其他编程语言中的函数。定义函数和调用函数的方式如下：

```bash
# 定义函数
say_hello() {
    echo "Hello, $1"
}

# 调用函数
say_hello "YunDinger"
```
## 6. 文件操作与运算符
Shell 可以通过文件运算符检查文件是否存在、可读等。还支持关系运算符、逻辑运算符、算术运算符等。

```bash
file="/path/to/file"
if [ -r $file ]; then
    echo "File is readable"
fi

score=90
maxScore=100
if [ $score -lt 60 ]; then
    echo "Failed"
else
    echo "Passed, max score is $maxScore"
fi
```

## 7.长代码
```bash
#!/bin/bash

# ./shell编程.sh 运行该脚本

# 这是一行注释
echo "Hello,Yungding"
echo "Hello," $HOME $HOSTNAME $LOGNAME

# 命名只能使用英文字母，数字和下划线
# 首个字符不能以数字开头，但是可以使用下划线（_）开头
# 中间不能有空格，可以使用下划线（_）
# 不能使用标点符号。不能使用 bash 里的关键字（可用 help 命令查看保留关键字）。

# 定义变量
name="YunDinger"
echo "你好,$name"

# 字符串操作
# 双引号拼接（推荐）
userName1="Y1"
userName2="Y2"
echo "两个人,"$userName1"_"$userName2""
echo "两个人,${userName1}_${userName2}"

# 获取字符串长度
testStr="alejfhjadfhlajf123 0-m"
# 第一种方式
echo ${#testStr} #输出 10
# 第二种方式
expr length "${testStr}" # 该方法在Mac上不适用;

# expr 使用
expr 5+6 # 直接输出 5+6
expr 5 + 6 # 输出 11
expr 5 * 6 # 输出 syntax error
expr 5 \* 6 # 输出 30

# 字符串截取
str="It's MYGO!!!!!"
# 从字符串第 i 个字符开始截取 j 个字符
echo ${str:5:4} # 输出 MYGO

# 正则表达式截取
var="https://www.runoob.com/linux/linux-shell-variable.html"
# %表示删除从后匹配, 最短结果
# %%表示删除从后匹配, 最长匹配结果
# #表示删除从头匹配, 最短结果
# ##表示删除从头匹配, 最长匹配结果
# 注: *为通配符, 意为匹配任意数量的任意字符
s1=${var%%t*} #h
s2=${var%t*}  #https://www.runoob.com/linux/linux-shell-variable.h
s3=${var%%.*} #https://www
s4=${var#*/}  #/www.runoob.com/linux/linux-shell-variable.html
s5=${var##*/} #linux-shell-variable.html

# 数组 （不支持多维）
arr=(1 2 3 4 5)
arr_test=(1,2,3,4,5) # 这样算一个元素，不同元素之间要使用空格分割
# 获取数组长度
echo ${#arr[@]}
echo ${#arr[*]}
# 获取数组元素
echo ${arr_test[0]}
echo ${arr[2]}
# 删除数组元素
unset arr[1]
# 遍历输出数组
for i in ${arr[*]}; do echo $i; done
# 删除数组全部元素
unset arr
for i in ${arr[*]}; do echo $i; done

# 关系运算符
echo 1 -eq 2 # ==
echo 1 -ne 2 # !=
echo 1 -gt 2 # >
echo 1 -lt 2 # <
echo 1 -ge 2 # >=
echo 1 -le 2 # <=

# if + 关系运算符
score=90;
maxScore=100;
if [ $score -lt 60 ] # 与 [] 间保持空格
then
    echo "挂了"
else
    echo "没挂(>o<)，满分${maxScore}"
fi # final

# 逻辑运算符
# && ||

# 布尔运算符
# ! -o -a 非 或 与 后两者已被优化

# 字符串相等比较 使用 =

# 文件运算符 [参数] file
# 例子
file="/Users/yunding/shell编程.sh"
if [ -r ${file} ]
then
    echo "report: 文件 ${file} 可写"
fi

# 流程控制
if [ 1 -eq 2 ]
then
    echo "1等于2"
elif [ 2 -eq 2 ]
then
    echo "2等于2"
else
    echo "2不等于2"
fi

# for 循环
# 输出列表
for i in 1 2 3 4 5
do
    echo "The value is ${i}"
done
# 产生10个随机数
for i in {0..9}
do
    echo "RANDOM:" $RANDOM
done
# 输出1-n
n=8
for ((i=1; i<=n; i++))
    do
        echo ${i}
    done
    
# while 语句
# 基本循环
n=10
while(( ${n}<=20 ))
do
    echo ${n}
    let "n++"
done
# 读取键盘输出
echo -n "输入你的昵称："
while read FILM
do
    echo "${FILM} 堂堂登场!!!"
    break
done

# shell 函数
# 编写函数 （没有返回值）
echoRandomBigNum() {
    echo "啊，我被调用了"
    echo $RANDOM$RANDOM$RANDOM
    echo "调用结束"
}
# 函数调用
echoRandomBigNum
# 又返回值的函数
getHello() {
    echo "输入姓名: "
    read name
    return $(("你好${name}"))
}
getHello
echo $?
# 带参数
printArr() {
    arr=$*
    len=${#arr}
    echo "数组长度"$len""
    for ((i=0; i<len; i++))
    do
        echo "我是"$i""
    done
}

```
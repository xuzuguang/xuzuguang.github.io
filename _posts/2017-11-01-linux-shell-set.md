---
layout: post
title:  "linux中常用的shell命令"
date:   2017-11-01
categories: linux  shell
tags: shell
---

* content
{:toc}

平时工作中遇到很多常用的但是又不太记得的shell，记录下来，以防不备之需。





## 删除指定文件之外的全部文件


    //删除keep之外的全部文件
    ls | grep -v keep |xargs rm 

## 文件解压和压缩相关命令

    //解压tar压缩文件到指定路径

    tar -xvf tar文件路径 -C 路径


    //解压zip压缩文件到指定路径

    unzip test.zip                  -- 解压到当前目录
    unzip -v test.zip               -- 查看zip文件中的目录
    unzip -o test.zip -d tmp/       -- 解压到指定目录
 
## 查看库文件的依赖

> ldd(选项)(参数) <br/>
	选项 --version：打印指令版本号； <br/>
	     -v：详细信息模式，打印所有相关信息； <br/>
	     -u：打印未使用的直接依赖； <br/>
	     -d：执行重定位和报告任何丢失的对象； <br/>
	     -r：执行数据对象和函数的重定位，并且报告任何丢失的对象和函数； <br/>
	     --help：显示帮助信息。 参数 文件：指定可执行程序或者文库。 <br/>


## 生成文件的Md5值
    md5sum  文件路径


未完待续.......................................

---
layout: post
title:  "重装系统后的配置清单"
date:   2017-11-01
categories: 系统配置  
tags: 系统配置
---

* content
{:toc}

最近频繁的重装系统，每次都会在系统初始化配置下做各种重复工作，现特将其记录下来。






## Linux使用Oracle JDK替换OpenJDK详解

1. 去java官网下载jdk压缩包，解压到/opt/java/（随意目录，这个是笔者的目录） 目录下。

2. 修改/opt/java/bin/目录下文件的权限。  为了方便，直接执行
    ```
    chmod 777 /opt/java/bin/* 
    ```

3. 修改.bashrc文件，添加java对应的环境变量
    添加：
    ```
    export JAVA_HOME=/opt/java
    export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
    export PATH=$PATH:$JAVA_HOME/bin
    ```
    添加完成后，保存.bashrc 执行：
    ```
    source ~/.bashrc
    ```

4. 更新/usr/bin目录下原有java javac文件的软链接
    ```
    cd /usr/bin
    ln -s -f /opt/java/bin/java 
    ln -s -f /opt/java/bin/javac
    ```

5. 测试检查是否生效
    执行
    ```
    java -version
    java version "1.7.0_71"
    Java(TM) SE Runtime Environment (build 1.7.0_71-b14)
    Java HotSpot(TM) 64-Bit Server VM (build 24.71-b01, mixed mode)
    ```
6. 配置成功
## 系统版本
    ubuntu17.10 
## 软件配置清单
    . git <br/>
    . Visual Studio Code: 基本用他替代笔记本  vi了。<br/>
    . nixnote2：       ubuntu下的evernote的开源客户端版本，最主要的是还不占evernote的终端连接数。<br/>
    . eclipse：        写代码都在这里了。<br/>
    . sudo apt-get install gnome-panel  这家伙完美的替换了东西，太他妈喜欢了。看截图 <br/>
    
![Alt text](/img/imgScreen.png)
    
---
title: IIS实现局域网联调
date: 2018-3-15 20:58:33
tags: IIS
categories: IIS
---
## 建立局域网络
    1.手机打开热点
    2.使你的笔记本连接至手机热点
    3.这时手机与笔记本就已经处于同一个局域网内了
## 基于IIS建立本地项目服务
    1.打开控制面板
    2.点击程序
    3.点击启用或关闭Windows功能
    4.选中Internet Information Services(IIS)--不必全选默认方块状态就可以了
    5.确定后会进行功能应用
## IIS管理器基础设置
    1.本地搜索并打开IIS管理器
    2.在右侧本地服务的网站目录上右键添加网站
    3.网站名称随意,物理路径选择你的本地项目地址
    4.IP地址为手机热点局域网地址(IP查看方式:运行CMD打开命令行->输入ipconfig->复制IPv4地址)
    5.端口号范围为1~65535(http默认80,https默认443)
    6.主机名为域名,可不填
    7.启动服务后即可在PC端与手机端输入ip地址与端口号同时浏览网站进行调试
## 基本流程图
![IIS流程图](https://github.com/FangFangZhenZhen/FangFangZhenZhen.github.io/raw/SourceCode/source/images/IIS.png)
## 实现效果
* PC端
![PC](https://github.com/FangFangZhenZhen/FangFangZhenZhen.github.io/raw/SourceCode/source/images/PC.png)
* 手机端
![PHONE](https://github.com/FangFangZhenZhen/FangFangZhenZhen.github.io/raw/SourceCode/source/images/PHONE.jpg)

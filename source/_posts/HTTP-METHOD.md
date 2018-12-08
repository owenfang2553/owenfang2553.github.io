---
title: AJAX中的HTTP请求方式
date: 2017-10-07 14:24:25
tags: AJAX
categories: AJAX
---
## HTTP METHOD
    HTTP常用的几种请求方式：
    1.GET
    2.POST
    3.PUT
    4.DELETE
    5.HEAD
这些请求方式不管是哪一种都可以向服务器请求获取数据或者传递数据，从本质上无区别，只是在行业内被开发人员约定俗成各自不同的用处，并非是一种标准。
## 方法分析
    1.GET:一般用于从服务器获取数据（给服务器数据少，获取数据多，此方法最为常用）
    2.POST:一般用于推送数据给服务器（给服务器多，获取数据少）
    3.PUT：一般用于向服务器传递资源文件（上传图片功能）
    4.DELETE:一般用于删除服务器资源文件
    5.HEAD:一般用于获取服务器响应头信息
## GET PK POST
传递方式：

    GET:向服务器传递内容一般通过"URL问号传参方式"
    xhr.open("GET","/getlist?a=2&num=4",true);

    POST:向服务器传递内容一般通过"请求主体的方式"
    xhr.open("POST","/postlist",true);
    xhr.send('{"name":"fang","age":"22"}'); //传递的为JSON对象格式的字符串

大小问题：

    GET请求传递给服务器的内容存在大小限制，而POST理论上无限制（实际一般最大2MB）
    原因：GET是通过URL传参形式，而每个浏览器对URL长度有限制，谷歌8KB，火狐7KB，IE2KB,所以当兼容IE情况下最大上传2KB大小内容。

缓存问题：

    GET请求会出现缓存 => 因为请求地址与传参相同（不一定是304），POST请求无缓存
    在项目中我们的GET请求一般不允许出现缓存 => “清除缓存” => 改变URL => 在URL末尾追加一个随机数 => xhr.open("GET","/getlist?a=2&num=5&_="+Math.random(),true);

安全问题：

    一般来说GET不安全（URL传参）,而POST相对安全
    实际当攻击者想攻击时都不安全



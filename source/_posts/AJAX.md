---
title: AJAX兼容与惰性思想
date: 2017-09-28 21:50:21
tags: Ajax
categories: Ajax
---
## JS编程技巧:惰性思想
    所谓的惰性思想可以简单地理解为:只执行一次可以搞定的事，绝不执行多次。
### 惰性思想代码块:
```javascript{.line-number}
    var utils=(function(){
        var flag="getComputedStyle" in window; //若返回true则为标准浏览器，false为IE8及以下浏览器
        function getCss(){
            if(flag){  //因为以上已判断此浏览器结果并保存,直接使用flag而不需要用window.getComputedStyle判断

            }else{

            }
        }
        return{
            getCss:getCss;
        }
    })()
    utils.getCss();
    utils.getCss();
    utils.getCss();
```
## 惰性思想创建AJAX对象
    利用函数创建且兼容所有浏览器
### 源代码
```javascript{.line-number}
var createXHR = function () {
    var xhr = null,
        flag=false, //判断浏览器是否支持AJAX
        // 定义一个数组用于存放创建AJAX对象的各种方法
        arr = [
            function () {
                return new XMLHttpRequest(); //只兼容IE7及以上
            },
            // 以下三种方法兼容低版本浏览器
            function () {
                return new ActiveXObject("Microsoft.XMLHTTP");
            },
            function () {
                return new ActiveXObject("Msxml2.XMLHTTP");
            },
            function () {
                return new ActiveXObject("Msxml3.XMLHTTP");
            }
        ];
    //  遍历这个数组中的小方法,当可用时即可取用
    for (var i = 0; i < arr.length; i++) {
        var curFn = arr[i]; //获取当前遍历到的小方法
        try {
            // 本次循环获取的方法执行没有报错：说明此方法可用，下次直接执行此小方法即可=>将createXHR重写为此小方法
            xhr = curFn();
            createXHR = curFn;
            flag = true; //浏览器可兼容AJAX则为true;
            break; //结束循环
        } catch (e) {
            //本次获取的方法执行报错则继续循环遍历下一个方法
        }
        if (!flag) {
            throw new Error("please updata your brower"); //浏览器不支持AJAX时抛出错误

        }
    }
    return xhr;
};
```
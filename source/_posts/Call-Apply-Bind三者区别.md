---
title: Call_Apply_Bind三者区别
date: 2018-3-10 22:12:59
tags: Js
categories: Js
---
## 非严格模式下
```javascript(.line-number)
        // var obj = {
        //     'name': 'fang',
        //     'sex': 'man'
        // };
        // // console.log(obj['name'],obj.sex); 获取对象属性值
        // var fn = function (num1, num2) {
        //     console.log(num1 + num2);
        //     console.log(this);
        // }
        // fn.call();//输出： NaN window对象 非严格模式下未指定this则默认指向window
        // fn.call(undefined); //输出： NaN window对象
        // fn.call(null); //输出： NaN window对象
        // fn(100,200);  //输出 300  window对象 （fn为window调用）
        // fn.call(obj, 100, 200); //改变this指向，并传参

        // fn.apply(obj, [100, 200]); //IE6~8下不兼容 改变this指向，并以数组形式传参  与call只是传参形式不同  在严格与非严格模式下this指向输出相同
        // var tempFn = fn.bind(obj, 100, 200); //无输出  只是提前修改了this指向并传参  但并未执行  简称预处理！！！可将返回值预先传给变量 需要时再调用执行
        // tempFn();
```
## 严格模式下
```javascript(.line-number)
         // 'use strict'
        // var obj = {
        //     'name': 'fang',
        //     'sex': 'man'
        // };
        // var fn = function (num1, num2) {
        //     console.log(num1 + num2);
        //     console.log(this);
        // }
        // fn.call(); //输出 NaN undefined
        // fn.call(undefined);//输出  NaN undefined
        // fn.call(null);//输出  NaN null
        // fn.call(100,200); //输出 NaN 100 this指向100  只传了一个参数所以为200+num2 为 NaN
        // 严格模式下call指向谁就输出谁 未输入则输出undefined
        // call与apply使用情况由项目决定
```
## 获取数组最大值与最小值思想
### 思想一
排序法（arr.sort）
```javascript(.line-number)
        // var arr = [12, 25, 1, 22, 8, 6, 88, 7, 9, 44];
        // arr.sort(function (a, b) {
        //     return a - b;
        // }); //小到大排序
        // var max = arr[arr.length - 1];
        // var min = arr[0];
        // console.log(min, max);
```
### 思想二
eval() 字符串拼接实现  Math.max() Math.min()
```javascript(.line-number)
        // var arr = [12, 25, 1, 22, 8, 6, 88, 7, 9, 44];
        //  var max=   Math.max(arr); //输出NaN  此方法需要一个一个写入值
        // var max=Math.max(2,5,11,1,4); //输出：11
        // 所以需要将arr转为一个一个数传入此方法
        // var str = arr.toString(); // '12, 25, 1, 22, 8, 6, 88, 7, 9, 44'
        // // eval:此方法将会将字符串作为JS语句执行  可用于计算表达式类型字符串的值eval("1+2+4")===7
        // // var evalstr= eval(str); //输出44  失败  原因:当括号括号函数中传入多个需要执行的值,将只执行最后一个  并会改变this指向
        // // 尝试字符串拼接的思想  ->成功
        // var max = eval('Math.max(' + str + ')'); //eval('Math.max(12, 25, 1, 22, 8, 6, 88, 7, 9, 44)');
        // var min = eval('Math.min(' + str + ')'); //eval('Math.min(12, 25, 1, 22, 8, 6, 88, 7, 9, 44)');
        // console.log(min, max);
```
### 思想三
假设法 假设一个为最大值或最小值 循环比较 替换值
```javascript(.line-number)
  // var arr = [12, 25, 1, 22, 8, 6, 88, 7, 9, 44];
        // var min=arr[0],max=arr[0];
        // for(var i=1;i<arr.length;i++){
        //     arr[i]<min?min=arr[i]:null;
        //     arr[i]>max?max=arr[i]:null;
        // }
        // console.log(min,max);
```
### 思想四
apply(null,arr)  Math.max() Math.min();
```javascript(.line-number)
       // var arr = [12, 25, 1, 22, 8, 6, 88, 7, 9, 44];
        // var max = Math.max.apply(null, arr);
        // var min = Math.min.apply(null, arr);
        // console.log(max);
        // console.log(min);
```

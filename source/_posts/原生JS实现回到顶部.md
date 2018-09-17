---
title: 原生JS实现回到页面顶部
categories: JavaScript
tags: JavaScript
---
## 实现效果
        1.右下角按钮回到顶部(点击后按钮消失，随即页面滑动到顶部;页面下拉距离超过一屏幕显示回到顶部按钮，反之按钮消失)
        2.列表hover时显示随机颜色(利用时间委托机制)
        3.点击左侧列表，页面滑动到对应模块(通过获取所点击列表的ID值滑动到对应CLASS值相同的模块)
[点击查看源代码](https://github.com/FangFangZhenZhen/Example/blob/master/toTop.html)
## 代码分析
### 回到顶部按钮
            1.获取当前scrollTop距离（var distance=document.documentElement.scrollTop||document.body.scrollTop）
            2.总时间(var timer=500;单位：ms)  总共需要运动500ms
            3.频率( var frequency=10;单位：ms) 每10ms运动一次
            4.步长(var step=distance/timer*frequency)  每毫秒运动距离*频率=每一次运动所需步长
#### 回到顶部代码
```javascript{.line-number}
            var span = document.getElementsByTagName('span')[0];
            var i = document.getElementsByTagName('i')[0];
            var timer = 1000; //总时间
            var frequency = 5; //频率
            window.onscroll = show;
            function show() {
                var dis = document.documentElement.scrollTop || document.body.scrollTop;
                var windis = window.screen.height; //屏幕分辨率高度
                // console.log(windis); 
                //当前滚动条距离大于一个屏幕高度时显示回到顶部按钮
                if (dis >= windis) {

                    span.style.display = 'block';
                    i.style.display = 'block';
                } else {
                    //当前滚动条距离小于一个屏幕高度时隐藏回到顶部按钮
                    span.style.display = 'none';
                    i.style.display = 'none';
                }
            };
            span.addEventListener('click', function (e) {
                span.style.display = 'none'; //单击后虽然设置为隐藏，但是由于滚动条顶部距离大于屏幕高度又触发上一个事件，使其又显示，所以当点击时需要阻止上一个事件发生；
                i.style.display = 'none';
                window.onscroll = null; //点击后阻止前一个DOM 0级事件  二级事件无法用此方法阻止
                e = e || event;
                var disTop = document.documentElement.scrollTop || document.body.scrollTop; //获取运动前滚动条顶部距离
                var step = (disTop / timer) * frequency; //步长
                //定时器：每隔一定频率运动距离
                var intime = window.setInterval(function () {
                    var curTop = document.documentElement.scrollTop || document.body.scrollTop; //获取滚动时当前滚动条距离
                    if (curTop === 0) {

                        window.clearInterval(intime); //当距离为0时清除定时器
                        window.onscroll = show;
                        return; //中断执行
                    } else {
                        curTop -= step; //使当前滚动条距离每隔frequency时间在当前距离基础上减去原始步长
                    }
                    //每隔frequency时间设置一次滚动条距离
                    document.documentElement.scrollTop = curTop;
                    document.body.scrollTop = curTop;
                }, frequency);

            });
```
### 列表随机颜色
        1.利用事件委托机制给父级DIV加一个mouseover事件
        2.mouseover在其子对象li上时触发事件，获取触发事件的目标(var elem = e.target;),对此目标进行操作
        3.定义一个数组存放十六进制数，用于组成随机十六进制颜色，生成6个随机数(0-15)作为数组下标，将对应值存入一个空数组并转为字符串，即为十六进制颜色。
#### 随机颜色代码
```javascript{.line-number}
            var items = document.getElementsByTagName('li');
            var ul = document.getElementsByTagName('ul');
            // console.log(ul[0]);
            //事件委托：给父元素添加触发事件
            ul[0].addEventListener('mouseover', function (e) {
                e = e || event;
                var elem = e.target; //获取目标
                elem.style.background = null; // 每一次触发事件都先初始化
                var num = ['a', 'b', 'c', 'd', 'e', 'f', 0, 1, 2, 3, 4, 5, 6, 7, 8, 9]; //十六进制颜色
                var colorNum = [];
                do {
                    var randomNum = Math.floor(Math.random() * 16); //0-15 //随机的0-15之间的一个数字作为十六进制数组下标
                    colorNum.push(num[randomNum]);
                } while (colorNum.length < 6); //将获取的6个随机数到空数组
                // console.log(colorNum);
                var colorstr = colorNum.join(''); //转为字符串
                //   console.log(colorstr);
                elem.style.background = "#" + colorstr; //设置随机颜色
            });
            //鼠标离开时清空颜色
            ul[0].addEventListener('mouseout', function (e) {
                e = e || event;
                var elem = e.target;
                elem.style.background = null;
            });           
```
### 滑动模块
        1.与获取随机颜色原理相同，需要利用事件委托，通过给父元素DIV加点击事件获取目标事件
        2.获取所点击目标的ID值，寻找模块中CLASS值与ID值相同的模块，并获取其距离顶部高度
        3.需要设置过渡动画效果则与按钮部分相同需要设置运动总时间、总距离、频率等
#### 滑动模块代码
```javascriptP{.line-number}
            ul[0].addEventListener('click', function (e) {
                e = e || event;
                var liElem = e.target; //利用时间委托获取目标
                var liName = liElem.getAttribute('id') //获取目标id值
                var divName = document.getElementsByClassName(liName)[0]; //获取对应名称的DIV模块元素
                var top = divName.offsetTop; //获取对应的滚动条高度
                var timer = 500; //总时间
                var frequencytwo = 3; //频率
                if (top < timer) {
                    top += 500;
                }
                var stepgo = (top / timer) * frequencytwo; //步长
                //定时器：每隔一定频率运动距离
                var intn = window.setInterval(function () {
                    var curTop = document.documentElement.scrollTop || document.body.scrollTop; //获取滚动时当前滚动条距离
                    if (curTop === top) {
                        window.clearInterval(intn); //当两者相同时清除定时器
                        return; //中断执行       
                    }
                    if (curTop > top) {
                        curTop -= stepgo; //使当前滚动条距离每隔frequency时间在当前距离基础上减去原始步长    
                    }
                    if (curTop < top) {
                        curTop += stepgo;
                    }
                    //每隔frequency时间设置一次滚动条距离
                    document.documentElement.scrollTop = curTop;
                    document.body.scrollTop = curTop;

                }, frequencytwo);
            });
```
<code> 本人才疏学浅，暂时只能展示所知显浅部分，有误或可改进之处请留言告知 ◕‿-</code>




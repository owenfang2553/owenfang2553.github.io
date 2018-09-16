---
title: 原生JS实现回到页面顶部
categories: JavaScript
tags: JavaScript
---

## 先上源代码(有空再写文)
```javascript{.line-number}

 <!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        * {
            padding: 0;
            margin: 0;
        }

        #div1 {
            width: 100%;
            height: 100%;
            /* background: linear-gradient(to bottom, #CDDC39, rgb(36, 92, 212), green, red, gray, pink); */
        }

        #div1>div {
            width: 100%;
            height: 500px;
            text-align: center;
            font-size: 80px;
        }

        .demo1 {
            background-color: red;
        }

        .demo2 {
            background-color: green;
        }

        .demo3 {
            background-color: grey;
        }

        .demo4 {
            background-color: rgb(47, 0, 255);
        }

        .demo5 {
            background-color: rgb(216, 34, 207);
        }

        .demo6 {
            background-color: rgb(20, 162, 206);
        }

        .demo7 {
            background-color: rgb(179, 129, 22);
        }

        .demo8 {
            background-color: rgb(100, 196, 36);
        }

        .demo9 {
            background-color: rgb(19, 241, 85);
        }

        .demo10 {
            background-color: rgb(240, 40, 67);
        }

        span {
            width: 30px;
            height: 30px;
            border: 5px solid white;
            display: inline-block;
            position: fixed;
            right: 50px;
            bottom: 150px;
            border-radius: 50%;
            background: linear-gradient(to top right, rgb(82, 55, 12), rgb(43, 128, 197));
            cursor: pointer;
            z-index: 2;
            opacity: 0.5;
            display: none;
        }

        span:hover {
            opacity: 1;
        }

        i {
            display: inline-block;
            width: 60px;

            height: 60px;
            border: 5px solid white;
            position: fixed;
            right: 35px;
            bottom: 135px;
            border-radius: 50%;
            background: linear-gradient(to top right, rgb(22, 134, 27), rgb(186, 218, 48));
            display: none;
        }

        ul {
            position: fixed;
            left: 100px;
            top: 30%;
            width: 80px;
            height: 237px;
        }

        ul>li {
            cursor: pointer;
            text-align: center;
            margin-bottom: 1px;
            background-color: rgb(86, 87, 82);
            list-style: none;
            border: 1px solid gray;
            color: white
        }

        /* li:hover{
            background-color: green;
        } */
    </style>
</head>

<body>
    <div id='div1'>
        <div class="demo1">一</div>
        <div class="demo2">二</div>
        <div class="demo3">三</div>
        <div class="demo4">四</div>
        <div class="demo5">五</div>
        <div class="demo6">六</div>
        <div class="demo7">七</div>
        <div class="demo8">八</div>
        <div class="demo9">九</div>
        <div class="demo10">十</div>
    </div>
    <span></span>
    <i></i>
    <ul class="item">
        <li id="demo1">one</li>
        <li id="demo2">two</li>
        <li id="demo3">three</li>
        <li id="demo4">four</li>
        <li id="demo5">five</li>
        <li id="demo6">six</li>
        <li id="demo7">seven</li>
        <li id="demo8">eight</li>
        <li id="demo9">nine</li>
        <li id="demo10">ten</li>
    </ul>
    <script>
        /*原生JS实现回到顶部动画分析：
            1.获取当前scrollTop距离（var distance=document.documentElement.scrollTop||document.body.scrollTop）
            2.总时间(var timer=500;单位：ms)  总共需要运动500ms
            3.频率( var frequency=10;单位：ms) 每10ms运动一次
            4.步长(var step=distance/timer*frequency)  每毫秒运动距离*频率=每一次运动所需步长
        */
        window.onload = function () {
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

            //左侧LI部分(使用事件委托给LI加随机背景颜色)

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

        }
    </script>
</body>

</html>
```




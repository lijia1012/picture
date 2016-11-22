# picture   相册
<!-- 尚硅谷 HTML5 2016年-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta http-equiv="Expires" content="-1">
    <meta http-equiv="Cache-Control" content="no-cache">
    <meta http-equiv="Pragma" content="no-cache">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black">
    <meta name="format-detection" content="telephone=no">
    <link href="/H50623/img/logo.png" rel="shortcut icon" type="image/x-icon"/>
    <title>尚硅谷HTML5</title>
    <link rel="stylesheet" href="/H50623/css/reset.css">
    <style type="text/css">
        /** 添加样式 **/
        .wrapper{
            position: absolute;
            top: 0px;
            right: 0px;
            bottom: 0px;
            left: 0px;
            width: 200px;
            height: 200px;
            background: red;
            margin: auto;
        }

    </style>
</head>
<body>
<div class="wrapper"></div>
<script type="text/javascript" src="./js/cssTransform2.0.js"></script>
<script type="text/javascript">
    //添加脚本
    document.addEventListener('touchstart',function (event) {
        event.preventDefault();
    });

    //			touches 当前屏幕上的手指列表
    //			targetTouches 当前元素上的手指列表
    //			changedTouches 触发当前事件的手指列表

    var wrapper = document.querySelector('.wrapper');
    var start = {s:0,r:0};
    var now = {s:0,r:0};

    //自定义一个函数来兼容安卓机
    var callBack = {
        start:function (event) {
            start.s = cssTransform(wrapper,'scale');
            start.r = cssTransform(wrapper,'rotate');
        },
        change:function (event) {
            now.s = event.scale;
            now.r = event.rotation;
            var s = cssTransform(wrapper,'scale',start.s*now.s);
            var r = cssTransform(wrapper,'rotate',start.r+now.r);
        },
        end:function (event) {
            wrapper.style.background = 'black';
        }
    };

    //获取缩放比例
    function getScale(point1,point2) {
        var targetX = point1.clientX - point2.clientX;
        var targetY = point1.clientY - point2.clientY;
        var targetScale = Math.sqrt(targetX*targetX+targetY*targetY);
        return targetScale;
    }

    //获取旋转角度
    function getRotate(point1,point2) {
        var targetX = point1.clientX - point2.clientX;
        var targetY = point1.clientY - point2.clientY;
        var targetRotate = (Math.atan2(targetY,targetX)*180)/Math.PI;
        return targetRotate;
    }

    function getTure(wrapper,callBack) {
        wrapper.addEventListener('touchstart',function () {
            if(event.touches.length>=2){
                start.s = getScale(event.touches[0],event.touches[1]);
                start.r = getRotate(event.touches[0],event.touches[1]);
            }

            if(callBack&&callBack['start']){
                callBack['start']();
            }
        });
        wrapper.addEventListener('touchmove',function (event) {
            if(event.touches.length>=2){
                now.s = event.scale;
                now.r = event.rotation;
                var s = getScale(wrapper,'scale',start.s*now.s);
                var r = getRotate(wrapper,'scale',start.r*now.r);
            }
            if(callBack&&callBack['moving']){
                callBack['moving']();
            }
        });
    }
    getTure(wrapper,callBack);
</script>
</body>
</html>

# 一、基于 JavaScript 实现玫瑰花
## 1、简介
基于 JavaScript 实现多个不同的形状图来组成一个逼真的玫瑰花。共使用了 31 个形状：24 个花瓣，4 个萼片，2 个叶子和 1 根花茎，其中每一个形状图都用代码进行描绘。
JavaScript+计算机图像
## 2、步骤
HTML-----index.html
### 1.简单的框架准备
```
<!DOCTYPE HTML>
<html>
<head>
<title>Rose</title>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
</head>
<body style="margin-left:200px">
<div style="text-align: center">
    <canvas id="c"></canvas>
</div>

<script>

</script>

</body>
</html>
```
### 2.绘制花瓣
#### 定义采样范围：
```
<!DOCTYPE HTML>
<html>
<head>
<title>Rose</title>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
</head>
<body style="margin-left:200px">
<div style="text-align: center">
    <canvas id="c"></canvas>
</div>

<script>

</script>

</body>
</html>
```
#### 编写形状描绘代码：
```
<!DOCTYPE HTML>
<html>
<head>
<title>Rose</title>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
</head>
<body style="margin-left:200px">
<div style="text-align: center">
    <canvas id="c"></canvas>
</div>

<script>

</script>

</body>
</html>
```
采样间隔越来越密集，点越来越接近，到最高密度时，相邻点之间的距离小于一个像素，肉眼就看不到间隔（见a=b=0.01）。
公式来绘制一个圆形（为了防止溢出，还要加上一个采样条件）：
两种方式：
第一种需要采样+采样限制条件：
```
<!DOCTYPE HTML>
<html>
<head>
<title>Rose</title>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
</head>
<body style="margin-left:200px">
<div style="text-align: center">
    <canvas id="c"></canvas>
</div>

<script>

</script>

</body>
</html>
```
采样限制条件：
```
<!DOCTYPE HTML>
<html>
<head>
<title>Rose</title>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
</head>
<body style="margin-left:200px">
<div style="text-align: center">
    <canvas id="c"></canvas>
</div>

<script>

</script>

</body>
</html>
```
第二种极坐标的方式+密集采样
```
<!DOCTYPE HTML>
<html>
<head>
<title>Rose</title>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
</head>
<body style="margin-left:200px">
<div style="text-align: center">
    <canvas id="c"></canvas>
</div>

<script>

</script>

</body>
</html>
```
让圆变形成一个花瓣+色彩：
形状变形+添加色彩
```
<script>
function surface(a, b) {
    var x = a * 100,
        y = b * 100,
        radius = 50,
        x0 = 50,
        y0 = 50;

    if ((x - x0) * (x - x0) + (y - y0) * (y - y0) < radius * radius) {
        return {
            x: x,
            y: y * (1 + b) / 2,            //变形
            r: 100 + Math.floor((1 - b) * 155), // 添加梯度
            g: 50,
            b: 50
        };
    } else {
        return null;
    }
}

for (a = 0; a < 1; a += .01) {
    for (b = 0; b < 1; b += .001) {
        if (point = surface(a, b)) {
            context.fillStyle = "rgb(" + point.r + "," + point.g + "," + point.b + ")";
            context.fillRect(point.x, point.y, 1, 1);
        }
    }
}
```
### 3.3D 曲面和透视投影
定义三维表面——定义一个管状物体

![touyin](https://github.com/TanYanjieZYX/WebGame/blob/master/pic/game1.gif)

```
<script>
function surface(a, b) {
    var x = a * 100,
        y = b * 100,
        radius = 50,
        x0 = 50,
        y0 = 50;

    if ((x - x0) * (x - x0) + (y - y0) * (y - y0) < radius * radius) {
        return {
            x: x,
            y: y * (1 + b) / 2,
            r: 100 + Math.floor((1 - b) * 155), // 添加梯度
            g: 50,
            b: 50
        };
    } else {
        return null;
    }
}

for (a = 0; a < 1; a += .01) {
    for (b = 0; b < 1; b += .001) {
        if (point = surface(a, b)) {
            context.fillStyle = "rgb(" + point.r + "," + point.g + "," + point.b + ")";
            context.fillRect(point.x, point.y, 1, 1);
        }
    }
}
```
将摄像头放置在（0，0，Z）位置，画布在一个跟X/Y平行但是在X/Y平面下方350距离的平面上。
投影到画布上的采样点为：
```
<script>
function surface(a, b) {
    var x = a * 100,
        y = b * 100,
        radius = 50,
        x0 = 50,
        y0 = 50;

    if ((x - x0) * (x - x0) + (y - y0) * (y - y0) < radius * radius) {
        return {
            x: x,
            y: y * (1 + b) / 2,
            r: 100 + Math.floor((1 - b) * 155), // 添加梯度
            g: 50,
            b: 50
        };
    } else {
        return null;
    }
}

for (a = 0; a < 1; a += .01) {
    for (b = 0; b < 1; b += .001) {
        if (point = surface(a, b)) {
            context.fillStyle = "rgb(" + point.r + "," + point.g + "," + point.b + ")";
            context.fillRect(point.x, point.y, 1, 1);
        }
    }
}
```
### 4. z-buffer——计算机图形学
在为物件进行着色时，执行“隐藏面消除”工作，使隐藏物件背后的部分就不会被显示出来
```
<script>
function surface(a, b) {
    var x = a * 100,
        y = b * 100,
        radius = 50,
        x0 = 50,
        y0 = 50;

    if ((x - x0) * (x - x0) + (y - y0) * (y - y0) < radius * radius) {
        return {
            x: x,
            y: y * (1 + b) / 2,
            r: 100 + Math.floor((1 - b) * 155), // 添加梯度
            g: 50,
            b: 50
        };
    } else {
        return null;
    }
}

for (a = 0; a < 1; a += .01) {
    for (b = 0; b < 1; b += .001) {
        if (point = surface(a, b)) {
            context.fillStyle = "rgb(" + point.r + "," + point.g + "," + point.b + ")";
            context.fillRect(point.x, point.y, 1, 1);
        }
    }
}
```
### 5. 旋转
矢量旋转的方法：欧拉旋转，实现绕 Y 轴旋转
```
<script>
function surface(a, b) {
    var x = a * 100,
        y = b * 100,
        radius = 50,
        x0 = 50,
        y0 = 50;

    if ((x - x0) * (x - x0) + (y - y0) * (y - y0) < radius * radius) {
        return {
            x: x,
            y: y * (1 + b) / 2,
            r: 100 + Math.floor((1 - b) * 155), // 添加梯度
            g: 50,
            b: 50
        };
    } else {
        return null;
    }
}

for (a = 0; a < 1; a += .01) {
    for (b = 0; b < 1; b += .001) {
        if (point = surface(a, b)) {
            context.fillStyle = "rgb(" + point.r + "," + point.g + "," + point.b + ")";
            context.fillRect(point.x, point.y, 1, 1);
        }
    }
}
```
### 6. 蒙特卡罗方法
设置合理的采样间隔——蒙特卡罗方法。
设置 a 和 b 为随机参数，用足够的采样完成表面填充。每次绘制 10000 点，然后静待屏幕完成更新。
```
<script>
function surface(a, b) {
    var x = a * 100,
        y = b * 100,
        radius = 50,
        x0 = 50,
        y0 = 50;

    if ((x - x0) * (x - x0) + (y - y0) * (y - y0) < radius * radius) {
        return {
            x: x,
            y: y * (1 + b) / 2,
            r: 100 + Math.floor((1 - b) * 155), // 添加梯度
            g: 50,
            b: 50
        };
    } else {
        return null;
    }
}

for (a = 0; a < 1; a += .01) {
    for (b = 0; b < 1; b += .001) {
        if (point = surface(a, b)) {
            context.fillStyle = "rgb(" + point.r + "," + point.g + "," + point.b + ")";
            context.fillRect(point.x, point.y, 1, 1);
        }
    }
}
```
注意：如果随机数发生错误时，表面填充效果会出错。
有些浏览器中，Math.random 的执行是线性的，这就有可能导致表面填充效果出错。这时，就得使用类似 Mersenne Twister（一种随机数算法）这样的东西去进行高质量的 PRNG 采样，从而避免错误的发生。

## 3、总结
为了使玫瑰的每个部分在同一时间完成并呈现，还需要添加一个功能，为每部分设置一个参数以返回值来进行同步。并用一个分段函数代表玫瑰的各个部分。比如在花瓣部分，可以使用旋转和变形来创建它们。
##参考实验楼的教程，多读代码！！！
# 二、基于HTML5 Canvas实现小游戏
## 1.简介
HTML5游戏开发的流程及游戏开发中需要处理的东西，canvas 游戏开发
## 2.实验步骤
使用画布的官方文档：
https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial
### 1、创建画布——game.js
```
var canvas = document.createElement("canvas");
var ctx = canvas.getContext("2d");
canvas.width = 512;
canvas.height = 480;
document.body.appendChild(canvas);
```
### 2、准备图片
通过创建简单的图片对象来实现加载的，类似bgReady的三个变量用来标识图片是否已经加载完成，如果在图片加载未完成情况下进行绘制是会报错的
```
var canvas = document.createElement("canvas");
var ctx = canvas.getContext("2d");
canvas.width = 512;
canvas.height = 480;
document.body.appendChild(canvas);
```
console.log(bgImage)来查看照片，你看到的将是类似：
`<img src="images/background.png" >`
### 3、游戏对象
英雄抓获怪物，我们得要有一个英雄和怪物的对象。而英雄有一个speed属性用来控制他每秒移动多少像素。怪物游戏过程中不会移动，所以暂时不用设置属性。monstersCaught则用来存储怪物被捉住的次数，初始值当然为0了。
```
var hero = {
    speed: 256 // movement in pixels per second
};
var monster = {};
var monstersCaught = 0;
```
### 4、处理用户的输入
监听用户的输入：键盘、鼠标。根据游戏的逻辑对用户的输入进行处理。监听这两个对象——**keydown+keyup**
```
// Handle keyboard controls
var keysDown = {};

addEventListener("keydown", function (e) {
    keysDown[e.keyCode] = true;
}, false);

addEventListener("keyup", function (e) {
    delete keysDown[e.keyCode];
}, false);
```
**在前端开发中，一般是用户触发了点击事件然后才去执行动画或发起异步请求之类的**
### 5、开始新一轮的游戏
**reset()函数用于开始新一轮和游戏，在这个方法里我们将英雄放回画布中心同时将怪物放到一个随机的地方。**
```
// Reset the game when the player catches a monster
var reset = function () {
    hero.x = canvas.width / 2;
    hero.y = canvas.height / 2;


    // Throw the monster somewhere on the screen randomly
    monster.x = 32 + (Math.random() * (canvas.width - 64));
    monster.y = 32 + (Math.random() * (canvas.height - 64));

};

```
### 6、更新对象
及时更新游戏的对象: **update函数**负责更新游戏的各个对象，会被规律地重复调用。
* 首先它负责检查用户当前按住的是哪种方向键，然后将英雄往相应方向移动。
* 传入的modifier 变量（控制英雄移动速度）-----基于1开始并随时间变化的一个因子，modifier的值会很小，但有了这一因子后，不管我们的代码跑得快慢，都能够保证英雄的移动速度是恒定的。
* 判断怪物和英雄的根据：31,32是由hero和monster图片的大小决定的，我们的hero图片是32x32，monster图片是30x32，所以根据坐标的位于图片中心的法制，就可以得到下面的判断条件。
* 接下来检查移动过程中所触发的事件了，也就是英雄与怪物相遇。这就是本游戏的胜利点，monstersCaught +1然后重新开始新一轮。
```
var update = function (modifier) {
    if (38 in keysDown) { // Player holding up
        hero.y -= hero.speed * modifier;
    }
    if (40 in keysDown) { // Player holding down
        hero.y += hero.speed * modifier;
    }
    if (37 in keysDown) { // Player holding left
        hero.x -= hero.speed * modifier;
    }
    if (39 in keysDown) { // Player holding right
        hero.x += hero.speed * modifier;
    }

    // Are they touching?
    if (
        hero.x <= (monster.x + 32)
        && monster.x <= (hero.x + 32)
        && hero.y <= (monster.y + 32)
        && monster.y <= (hero.y + 32)
    ) {
        ++monstersCaught;
        reset();
    }
};
```
### 7、渲染物体
**物体东西画出来：**
ctx就是最前面我们创建的变量。然后利用canvas的drawImage()首先当然是把背景图画出来。画英雄和怪物的顺序是有讲究的，因为后画的物体会覆盖之前的物体。
改变Canvas的绘图上下文的样式，调用fillText来绘制文字，也就是记分板那一部分。
```
// Draw everything
var render = function () {
    if (bgReady) {
        ctx.drawImage(bgImage, 0, 0);
    }

    if (heroReady) {
        ctx.drawImage(heroImage, hero.x, hero.y);
    }

    if (monsterReady) {
        ctx.drawImage(monsterImage, monster.x, monster.y);
    }

    // Score
    ctx.fillStyle = "rgb(250, 250, 250)";
    ctx.font = "24px Helvetica";
    ctx.textAlign = "left";
    ctx.textBaseline = "top";
    ctx.fillText("Goblins caught: " + monstersCaught, 32, 32);
};
```
### 8、主循环函数
**游戏的循环结构：main函数控制了整个游戏的流程**
* 先是拿到当前的时间用来计算时间差（距离上次主函数被调用时过了多少毫秒）。
* 得到modifier后除以1000(也就是1秒中的毫秒数)再传入update函数。
* 最后调用render 函数并且将本次的时间保存下来。
```
// The main game loop
var main = function () {
    var now = Date.now();

    var delta = now - then;
    //console.log(delta);
    update(delta / 1000);
    render();

    then = now;

    // Request to do this again ASAP
    requestAnimationFrame(main);
};
```
### 9、设置requestAnimationFrame()
**多的||就是考虑到浏览器兼容问题。**
```
var w = window;
requestAnimationFrame = w.requestAnimationFrame || w.webkitRequestAnimationFrame || w.msRequestAnimationFrame || w.mozRequestAnimationFrame;
```
### 10、启动游戏
**调用相应的函数来启动游戏：**
* 先是设置一个初始的时间变量then用于首先运行main函数使用。
* 然后调用 reset 函数来开始新一轮游戏（如果你还记得的话，这个函数的作用是将英雄放到画面中间同时将怪物放到随机的地方以方便英雄去捉它）
```
// Let's play this game!
var then = Date.now();
reset();
main();
```
## 3.总结
在玩游戏的过程中，你会发现每一次hero捕获到monster，hero就回到了canvas画布的正中间。**那么现在需要做的就是，将hero在捕捉到monster的时候让hero就停留在捕获的位置，不再是回到canvas正中间。**
##### 待思考

# 三、网页版拼图游戏
## 1、简介
基于 HTML+CSS+JavaScript 实现网页版的拼图游戏——九宫格拼图。它的玩法是移动空格块旁边的方块，使得它们按照方块上面标的数字顺序排好。
## 2、实验原理
* 设计视图:设置一个大DIV用来包裹里面的小DIV，然后在里面设置8个小DIV，从1开始给他们编号。右边设置两个按钮，点击**开始按钮**开始计时，完成拼图后停止计时，并弹出一个框，提示完成了。**重来按钮**是当用户觉得当前有难度的时候，点击重来可以重新开始一个新的拼图，把所有方块打乱顺序，然后开始计时。
* 判断方块移动：鼠标点击其中一个方块时，要判断当前方块是否可移动，如果可移动，则移动到相应的位置，如不可移动，则不做任何事。
* 当移动完一块后，判断是否完成拼图：把那个大DIV想象成一个盒子，它有九个位置，1开始，到9编号，他们的位置和编号都是不会变的。把里面的8个小DIV想象成8个小盒子，给他们设置top和left就可以控制他们的位置。每个小DIV从1 开始到 8编号。他们的位置是可以随意改变的。所以当小DIV的编号和大DIV的编号全部重合时，就完成了拼图。
* 判断是否可移动：设置一个一维数组变量，用来保存大DIV它里面装的小DIV 的编号。如果大DIV没有小方块，也就表面它是空白块，那么就设为0。如果当前大DIV有小DIV，那就设置为小DIV的编号。然后再设置一个二维数组变量，用来保存大DIV的可移动编号。也就是保存这个大DIV它所有的可去的位置。比如大DIV编号为2的，它只能向1号，3号，5号这三个方向移动。又比如5，它能向2、4、6、8这四个方向移动。我们循环遍历这个变量，如果对应的方向它没有方块，也就是值为0，那么它就可以往这个方向移动了。
### 编写布局——HTML文件
这里为了简化逻辑，更易编写代码，把所有操作都封装了。只要执行move(2)，就是点击了编号为2的小方块，后面的一系列操作都完成了。
```
<!doctype html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>Puzzle</title>
    <link rel="stylesheet" type="text/css" href="puzzle.css">
    <script type="text/javascript" src="puzzle.js"></script>
</head>
<body>
    <div id="container">
    <!--最外面的DIV，用来包含里面的结构-->
        <div id="game">
        <!--游戏区，也就是大DIV方块-->
            <div id="d1" onclick="move(1)">1</div>
            <!--小DIV，也就是8个小方块。当点击的时候执行move()函数，参数是显示的编号，这样我们就知道点击了那个方块-->
            <div id="d2" onclick="move(2)">2</div>
            <div id="d3" onclick="move(3)">3</div>
            <div id="d4" onclick="move(4)">4</div>
            <div id="d5" onclick="move(5)">5</div>
            <div id="d6" onclick="move(6)">6</div>
            <div id="d7" onclick="move(7)">7</div>
            <div id="d8" onclick="move(8)">8</div>
        </div>
        <div id="control">
        <!--游戏控制区-->
            <p>
                <rowspan id="timeText">总用时</rowspan>
                <rowspan id="timer"></rowspan>
            </p>
            <!--显示游戏时间区域-->
            <p>
                <rowspan id="start" onclick="start()">开始</rowspan>
                <rowspan id="reset" onclick="reset()">重来</rowspan>
            </p>
            <!--显示控制按钮区域-->
        </div>
    </div>
</body>
</html>
```
### 编写样式——CSS
```
<!doctype html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>Puzzle</title>
    <link rel="stylesheet" type="text/css" href="puzzle.css">
    <script type="text/javascript" src="puzzle.js"></script>
</head>
<body>
    <div id="container">
    <!--最外面的DIV，用来包含里面的结构-->
        <div id="game">
        <!--游戏区，也就是大DIV方块-->
            <div id="d1" onclick="move(1)">1</div>
            <!--小DIV，也就是8个小方块。当点击的时候执行move()函数，参数是显示的编号，这样我们就知道点击了那个方块-->
            <div id="d2" onclick="move(2)">2</div>
            <div id="d3" onclick="move(3)">3</div>
            <div id="d4" onclick="move(4)">4</div>
            <div id="d5" onclick="move(5)">5</div>
            <div id="d6" onclick="move(6)">6</div>
            <div id="d7" onclick="move(7)">7</div>
            <div id="d8" onclick="move(8)">8</div>
        </div>
        <div id="control">
        <!--游戏控制区-->
            <p>
                <rowspan id="timeText">总用时</rowspan>
                <rowspan id="timer"></rowspan>
            </p>
            <!--显示游戏时间区域-->
            <p>
                <rowspan id="start" onclick="start()">开始</rowspan>
                <rowspan id="reset" onclick="reset()">重来</rowspan>
            </p>
            <!--显示控制按钮区域-->
        </div>
    </div>
</body>
</html>
```
### 控制代码——JavaScript
```
<!doctype html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>Puzzle</title>
    <link rel="stylesheet" type="text/css" href="puzzle.css">
    <script type="text/javascript" src="puzzle.js"></script>
</head>
<body>
    <div id="container">
    <!--最外面的DIV，用来包含里面的结构-->
        <div id="game">
        <!--游戏区，也就是大DIV方块-->
            <div id="d1" onclick="move(1)">1</div>
            <!--小DIV，也就是8个小方块。当点击的时候执行move()函数，参数是显示的编号，这样我们就知道点击了那个方块-->
            <div id="d2" onclick="move(2)">2</div>
            <div id="d3" onclick="move(3)">3</div>
            <div id="d4" onclick="move(4)">4</div>
            <div id="d5" onclick="move(5)">5</div>
            <div id="d6" onclick="move(6)">6</div>
            <div id="d7" onclick="move(7)">7</div>
            <div id="d8" onclick="move(8)">8</div>
        </div>
        <div id="control">
        <!--游戏控制区-->
            <p>
                <rowspan id="timeText">总用时</rowspan>
                <rowspan id="timer"></rowspan>
            </p>
            <!--显示游戏时间区域-->
            <p>
                <rowspan id="start" onclick="start()">开始</rowspan>
                <rowspan id="reset" onclick="reset()">重来</rowspan>
            </p>
            <!--显示控制按钮区域-->
        </div>
    </div>
</body>
</html>
```
#### 多看代码！！！！！
## 3、总结
* 当前的实现是基于鼠标点击的，添加键盘监听，让游戏使用方向键也能玩
* 思考如何实现更健全的算法，让其可以完全复原
因为实验中使用的随机打乱方块的算法非常简单，但是存在 bug，有 50% 的概率生成的顺序是无法复原的，这个时候就只能点击重新开始。
提供一种思路：按照人的方式随机去移动方块，而移动方块的代码已经写好了，直接调用就可以，所以只要自己编写一个函数，让方块移动几十步就可以打乱。而这种方式是一定可以拼回去的。
# 四、网页版别踩白块游戏
## 1.简介
网页版需要点击黑块，黑块才会消失。使代码尽量简单，逻辑清晰，去掉了很多的事件控制按钮，刷新页面即可以开始游戏，只保留了实现这个小游戏最重要的部分代码。
## 2、实验原理
整个游戏的流程：一定的速度下移，点击黑块，黑块消失，新的黑在普通游戏玩家眼中，应该是游戏开始，黑块以块不断向下移动，若黑块触底则游戏结束；
而以开发者来说，应将每一个黑块和白块抽象成一个个的**数据结构**，黑块的**消失和出现**其实就是数据结构的**创造和销毁**，游戏的流程图：

![liucheng](https://github.com/TanYanjieZYX/WebGame/blob/master/pic/game2.png)

## 3、实验步骤
### 1.页面布局——div
div+css布局来实现别踩白块的静态效果展示。div将主界面分解成一个4x4的大矩形格子，每一个方块代表其中一个小的矩形格，其中每一行的四个白块中有一个黑块，每一行中黑块位于那一列是随机生成的。
```
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>别踩白块</title>
    <style type="text/css">
    </style>
</head>
<body>
    <div id="main">
        <div id="con">
            <div class="row">
                <div class="cell"></div>/*白块*/
                <div class="cell black"></div>/*黑块*/
                <div class="cell"></div>
                <div class="cell"></div>
            </div>
            <div class="row">
                <div class="cell"></div>
                <div class="cell black"></div>
                <div class="cell"></div>
                <div class="cell"></div>
            </div>
            <div class="row">
                <div class="cell"></div>
                <div class="cell"></div>
                <div class="cell black"></div>
                <div class="cell"></div>
            </div>
            <div class="row">
                <div class="cell black"></div>
                <div class="cell"></div>
                <div class="cell"></div>
                <div class="cell"></div>
            </div>
        </div>
    </div>
</body>
<script>
</script>
</html>
```
### 2.添加样式——css
将div#con块级元素向上提了100 px，这样在游戏的开始就出现了最底一行的空白，隐藏最上面那行。
```
 #main {
        width: 400px;
        height: 400px;
        background: white;
        border: 2px solid gray;
        margin: 0 auto;
        overflow: hidden;
    }

    #con {
        width: 100%;
        height: 400px;
        position: relative;
        top: -100px; /*隐藏最上层的那行*/
        border-collapse:collapse;
    }

    .row{
        height: 100px;
        width: 100%;
    }

    .cell{
        height: 100px;
        width: 100px;
        float: left;
    }

    .black {
        background: black;
    }
```
### 3.游戏初始化——js
每个` <div class="cell">`就代表一个白块，`<div class="cell black">`就代表一个黑块，每点击一个黑块消失其实是删除了一个`<div class="row">`，然后从上面添加一个新的` <div class="row"> `所以我们首先要通过 js 来控制` <div class="row"> `的创造和生成（记得删除在编写静态页面时候指定生成的4个 div.row）。
```
    //创建div, 参数className是其类名
    function creatediv(className){
        var div = document.createElement('div');
        div.className = className;
        return div;
    }


    // 创造一个<div class="row">并且有四个子节点<div class="cell">
    function createrow(){
        var con = $('con');
        var row = creatediv('row'); //创建div className=row
        var arr = creatcell(); //定义div cell的类名,其中一个为cell black

        con.appendChild(row); // 添加row为con的子节点

        for(var i = 0; i < 4; i++){
            row.appendChild(creatediv(arr[i])); //添加row的子节点 cell
        }

        if(con.firstChild == null){
            con.appendChild(row);
        }else{
            con.insertBefore(row, con.firstChild);
        }
    }

    //删除div#con的子节点中最后那个<div class="row">
    function delrow(){
            var con = $('con');
            if(con.childNodes.length == 6) {
                   con.removeChild(con.lastChild);
               }
        }

    //创建一个类名的数组，其中一个为cell black, 其余为cell
    function creatcell(){
        var temp = ['cell', 'cell', 'cell', 'cell',];
        var i = Math.floor(Math.random()*4);//随机生成黑块的位置
        temp[i] = 'cell black';
        return temp;
    }
  ```
### 4.让黑块动起来——js
通过 js 来创造和销毁 div 后，让黑块动起来，这个时候就用到了之前css提到的设定` <div id="con"> `隐藏了一行的` <div id="row">`，通过 js 的 DOM 操作使其向下方移动，并设置定时器每30毫秒移动一次，这样就实现了黑块的平滑移动，在黑块移动的同时，要判断黑块是否已经触底，触底则游戏失败，停止调用move()，触底后调用函数 fail() 游戏失败。
```
   //使黑块向下移动
    function move(){
        var con = $('con');
        var top = parseInt(window.getComputedStyle(con, null)['top']);

        if(speed + top > 0){
            top = 0;
        }else{
            top += speed;
        }
        con.style.top = top + 'px';

        if(top == 0){
            createrow();
            con.style.top = '-100px';
            delrow();
        }else if(top == (-100 + speed)){
            var rows = con.childNodes;
            if((rows.length == 5) && (rows[rows.length-1].pass !== 1) ){
                fail();
            }
        }
    }

    function fail(){
            clearInterval(clock);
            confirm('你的最终得分为 ' + parseInt($('score').innerHTML) );
        }
  ```
### 5.点击黑块事件——js
判断用户有没有点击到黑块，同时用户若点击到黑块我们要让所在那一行消失          judge 方法
```
//判断用户是否点击到了黑块，
function judge(ev){
    if (ev.target.className.indexOf('black') != -1) {
        ev.target.className = 'cell';
        ev.target.parentNode.pass = 1; //定义属性pass，表明此行row的黑块已经被点击
        score();
    }
}
```
添加一个记分器记录用户分数的功能，同时设置加速方法，使黑块的移动越来越快等等，可以尝试着添加事件按钮，使这个游戏更接近 APP 版本。
## 4、总结
  为游戏添加开始游戏按钮
  修改代码使游戏结束后不能再点击黑块
  将游戏改写为 jQuery 版

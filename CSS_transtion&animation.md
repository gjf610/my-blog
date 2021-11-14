## CSS知识总结

### 动画
* 定义
    由许多精致的画面（帧），以一定的速度（如每秒30张）连续播放时，肉眼因视觉残象产生错觉，而误以为是活动的画面。
* 概念

    帧：每个静止的画面都叫做帧

    播放速度：每秒24帧（影视）或者每秒30帧（游戏）
#### 做简单的例子
* 将div从左往右移动
* 原理 每过一段时间（用setInterval做），将div移动一小段距离，直到移动到目标地点
* 过程 CSS渲染过程依次包含布局、绘制、合成。（绿色表示重新绘制）。
#### 使用transform
* 原理 transform: translateX(200px),直接修改会被合成，需要等一会修改，transition过渡属性可以自动脑补中间帧
* 过程 CSS渲染过程没有repaint(重新绘制)，比修改left性能好
### 浏览器渲染原理
步骤
1. 根据HTML构建HTML树（DOM-> Document Object Model）
2. 根据CSS构建CSS树（CSSOM-> Cascading Style Sheets Object Model）
3. 将两棵树合并成一颗渲染树（render tree）
4. Layout布局（文档流、盒模型、计算大小和位置）
5. paint绘制（把边框颜色、文字颜色、阴影等画出来）
6. compose合成（根据层叠关系展示画面）

三种更新模式
1. JS/CSS -> 样式 ->布局->绘制->合成
   ```js
   display='none';//或者
   div.remove()
   ```
   会触发当前元素消失，其他元素reflow
2. JS/CSS -> 样式 ->绘制->合成
   改变背景颜色，直接repaint+composite
3. JS/CSS -> 样式 ->合成
   改变transform，只需要composite

#### CSS动画优化

JS优化

使用requestAnimationFrame代替setTimeout或setInterval

CSS优化

使用will-change或translate

### transform
四个常用功能
* 位移 translate
* 缩放 scale
* 旋转 rotate
* 倾斜 skew
  
经验

    一般都需要配合transition（过渡）。
    inline元素不支持transition，需要先变成block。
#### transform之translate
常用写法
* translateX: length|percentage;
* translateY: length|percentage;
* translate: length|percentage,length|percentage;
* translateZ: length|percentage; 且父容器加perspective
* translate3d: (X,Y,Z);

经验

    translate(-50%, -50%)可做绝对定位元素的居中。
#### transform之scale
常用写法
* scaleX: number;
* scaleY: number;
* scale: number,number;

经验

    用得少，因为容易出现模糊。
#### transform之rotate
常用写法
* rotate: angle|zero;
* rotateZ: angle|zero;
* rotateX: angle|zero;
* rotateY: angle|zero;
* rotate3d: (X,Y,Z);

经验

    一般用于360度旋转制作loading。
#### transform之skew
常用写法
* skewX: angle|zero;
* skewY: angle|zero;
* skew: angle|zero,angle|zero;

经验

    用得少
#### transform多重效果
组合使用
```CSS
transform: scale(0.5) translate(-100%, -100%);
transform: none;
```

### transtions过渡
作用：补充中间帧

语法：
transition: 属性名 时长 过渡方式 延迟
```CSS
transition: left 200ms linear;
```
可以用逗号分隔两个不同属性
```CSS
transition: left 200ms, top 400ms;
```
可以用all代表所有属性
```CSS
transition: all 200ms;
```
过渡方式：linear|ease|ease-in|ease-out|ease-in-out|cubic-bezier|steps|step-start|step-end

#### 注意：不是所有属性都能过渡
* display:none=>block没法过渡
* 一般改成visibility: hidden=>visible
* background-color 颜色√
* opacity 透明度√

#### 过渡必须有起始
一般只有一次动画变化，或者两次
如果处理起始，还有中间点
1. 使用两次transform

        a==>transform==>b==>transform==>c
    用setTimeout或监听transitionend事件
2. 使用animation

    声明关键帧，添加动画
### @keyframes (关键帧)
完整语法
1. from/to
   ```CSS
   @keyframes slidein {
       from{
           transform: translateX(0%);
       }
       to {
           transform: translate()100%;
       }
   }
   ```
2. 写百分数
      ```CSS
   @keyframes slide_in {
       0%{ top: 0; left: 0 }
       30% { top: 50px }
       68%, 72% { left: 50px }
       100% { top: 100px; left:100% }
   }
   ```
### animation动画
缩写语法

animation: 时长|过渡方式|延迟|次数|方向|填充模式|是否暂停|动画名
* 时长：1s或者750ms
* 过渡方式：跟transition取值一样，如linear
* 次数：3/2.4/infinite
* 方向： reverse|alternate|alternate-reverse
* 填充模式：none|forwards|backwards|both
* 是否暂停：paused|running

以上所有属性都有对立的单独属性

### 总结
学习CSS一定要多结合浏览器练习，看到浏览器的表现开会对CSS的对应属性有更深印象。

资料来源： &copy;饥人谷
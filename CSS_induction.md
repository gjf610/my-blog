# CSS 基础概念

## CSS的历史
### CSS是由谁发明的？
蒂姆·伯纳斯·李的挪威同事赖先生(Håkon W Lie)首先提出[CSS](https://www.w3.org/People/howcome/p/cascade.html)
### CSS的牛X之处在哪里？
  <font color=red>层叠</font>样式表 (Cascading Style Sheets)

### 层叠是指什么？
* 样式层叠 可以多次对同一选择器进行样式声明
* 选择器层叠 可以用不同选择器对统一元素进行样式声明
* 文件层叠 可以用多个文件进行层叠

  这些特性使CSS极度灵活，因此CSS被经常吐槽
### CSS的版本
  由W3C制定标准
|版本|时间|重点|
|----|----|---|
|CSS1|1996|不用管|
|CSS2|1998|添加定位、z-index、media，不用管|
|CSS2.1|2004~2011|使用最广泛的的版本（IE支持）|
|CSS3|1999开始起草|现代版本，分模块更新（IE8部分支持）|
|CSS4*|分模块升级|
### 使用[CANIUSE.com](https://caniuse.com/)来查询浏览器的支持特性 
## CSS语法
  1. 样式语法
  ```CSS
   [select]{
     [property]:[value]
   }
   /*注释*/
  ```
  ### 注意事项
  * 所有符号都是英文符号，如果写错了，浏览器会有警告
  * 要区分大小写，a和A是不同的东西
  * 没有//注释
  * 最后一个分号可以省略，但建议不要省略
  * 任何地方写错了，浏览器不会报错，直接忽略

  2. @语法
  ```CSS
   @charset "UTF-8";
   @import url(index.css);
   @media (min-width: 100px) and (max-width: 200px){
    [select]{
      [property]:[value]
    }
   }
  ```
  ### 注意事项
  * @charset必须放第一行
  * 前两个@语法必须以分号结尾
  * charset即字符集，但UTF-8是字符编码，encoding，历史遗留问题
## 文档流
### 流动方向
* inline元素从左到右，到达最右边才会换行
* block元素从上到下，每一个都另起一行
* inline-block也是从左到右
### 宽度
* inline宽度为内部inline元素之和，不能用width指定
* block默认计算宽度，可用width指定
* inline-block结合前两者特点，可用width指定
### 高度
* inline高度由line-height间接决定，跟height无关
* block高度由内部文档流元素决定，可用height指定
* inline-block跟block类似，可用height指定
### overflow溢出
* 当内容的宽度和高度大于容器时，会溢出
* 可以overflow设置是否显示滚动条
* auto是灵活设置
* scroll是永远显示
* hidden是直接隐藏溢出部分
* visible直接显示溢出部分
* overflow可以分为overflow-x和overflow-y
### 脱离文档流
### 回忆
> block高度由内部文档流元素决定，可用height指定
这句话是不是说有些元素可以不在文档流中
### 哪些元素可以脱离文档流
1. float
2. position: absolute/fixed/sticky
## CSS有两种盒模型
  ### 分类
  1. <code>content-box</code>

    内容盒，内容就是盒子的边界
  2. <code>border-box</code> 

    边框盒，边框才是盒子的边界
  ### 计算方式
  1. <code>content-box</code>

    width=内容宽度
  2. <code>border-box</code>
  
    width=内容宽度+padding+border
  ### border-box更好用

  ### margin合并
  #### 合并情景
  1. 父子margin合并
  2. 兄弟margin合并
  ### 如何阻止合并
  1. 父子合并用padding/border挡住
  2. 父子合并用overflow:hidden挡住
  3. 父子合并用display:flex
  4. 兄弟合并是符合预期的
  5. 兄弟合并可以用inline-block消除
  ## 单位
  ### 长度单位
  * px 像素
  * em 相对于自身的font-size倍数
  * % 百分数
  * 整数
  * vw和vh
  ### 颜色
  * 十六进制 #FF6600 或者#f60
  * RGBA rgb(0,0,0) 或者 rgba(0,0,0,1)
  * hsl 颜色 hsl(360, 100%, 100%)或者hsla(360, 100%, 100%, 0.5)


资料来源： &copy;饥人谷
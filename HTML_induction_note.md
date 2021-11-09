# HTML入门笔记1

## 一、HTML的起源
<font color=red>HTML第一个正式版本是从2.0开始的，从来没有1.0版本。</font>Tim Berners Lee （蒂姆·伯纳斯·李）于1992年发表了一篇名为[HTML Tags](https://web.archive.org/web/20100131184344/http:/www.w3.org/History/19921103-hypertext/hypertext/WWW/MarkUp/Tags.html)的文章。其中的部分标签一直用到现在，但这篇文章并非官方的规范。这篇文章一共定义了18个元素，除了a标签外，其他设计都深受SGML的影响。使用标签、尖括号、title等等，并不是蒂姆·伯纳斯·李首创的想法。当时的SGML里就有了这些概念。

HTML 2.0是由IETF，因特网工程任务组（Internet Engineering Task Force）制定的。但从第三个版本开始往后，W3C，万维网联盟（World Wide Web Consortium）（<font color=blue>ps:蒂姆·伯纳斯·李担任万维网联盟的主席</font>）开始接手，并负责后续版本的制定工作。

## 二、开发HTML页面的起手式
在主流编辑器<code>VS Code</code>、<code>WebStorm</code>、<code>Sublime Text</code>、<code>Eclipse</code>、<code>Atom</code>、<code>Notepad++</code>里可以通过Emmet语法规则 ! => Tab来快速生成基础的结构，这样一个开发HTML页面的起手式就完成了。
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  
</body>
</html>
```
下面来介绍一下这个起手式里的内容。
```html
<!DOCTYPE html>
```
声明文档类型为 HTML5 文档。
```html
<html lang="en">
```
<code>html</code>元素是 HTML 页面的根元素。<code>lang="en"</code>定义页面使用的基础语言为英语。（tips：在中国内使用的页面要一般改为"zh-CN"，便于浏览器翻译。）
```html
<head></head>
```
<code>head</code> 元素包含了文档的元（meta）数据，标题（title）等，在页面内一般不可见。
```html
<meta charset="UTF-8">
```
定义网页字符集编码格式为 utf-8。（tips：统一为utf-8。）
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
定义网页视窗，防止视窗缩放。
```html
<meta http-equiv="X-UA-Compatible" content="ie=edge">
```
定义使用最新IE浏览器版本（edge）解析编译页面。
```html
<title>Document</title>
```
定义网页标题。
```html
<body></body>
```
<code>body</code> 元素展示可见的页面内容。

## 三、常用章节标签
表示文章、书的层级
1. 标题 <code>h1~h6</code> （大小依次递减的标题）
2. 章节 <code>section</code>（文档的某个区域，比如章节、头部、底部或者文档的其他区域）
3. 文章 <code>article</code>（有意义的且必须是独立于文档的其余部分）
4. 段落 <code>p</code>（文章段落）
5. 头部 <code>header</code>（放在顶部，用于显示广告）
6. 脚部 <code>footer</code>（放在底部，用于显示相关链接和服务，以及版权生命）
7. 主要内容 <code>main</code>（展示网页的主体部分）
8. 旁支内容 <code>aside</code>（展示网页的旁支内容，可用作页面的侧边栏）
9. 划分 <code>div</code> （一个分隔区块或者一个区域部分）

## 四、全局属性
所有标签都有的属性
* class （定义元素的类名）
  用法<code>[class='className']</code>或者<code>.className</code>。
* contenteditable（定义是否可编辑元素的内容）
  1. 将<code>style</code>标签移到<code>body</code>里，添加下面样式，用户就可以自己调试样式。
  ```css
  style {
    display: block;
  }
  ```
  2. 用来做自己的编辑器
* hidden （对元素进行隐藏）
* id（定义元素的唯一id）
  1. 在<code>style</code>标签里使用<code>#idName</code>定义样式
  2. 在js里直接修改样式
  ```javascript
  idName.style.color = 'red';
  ```
  <font color=red>注意：id的全局唯一性没有保障，就算有两个重复的 id，HTML 也不报错</font>
* style（设置元素的行内样式）
  样式权重<font color=green>（tips: js是调用style属性在时序里覆盖样式）</font>
  1. style属性
  2. id属性
  3. class属性
* tabindex（设置元素的 Tab 键控制次序）
  1. tabindex可以是正数，不必是连续的
  2. tabindex可以是 0，表示最后才被 tab 访问
  3. tabindex可以是 -1，表示不可被 tab 访问
* title （设置元素的额外信息）
  用处：设置省略号（...）时，鼠标上浮显示全部信息
  ```css
  // 设置省略号
  style {
    white-space: nowarp;
    text-overflow: ellipsis;
    overflow: hidden;
  }
  ```
## 五、默认样式
  * ##### 为什么有默认样式
    因为HTML被发明时，CSS还没有诞生。
  * ##### 怎么看默认样式
    打开开发者工具->Elements->Styles->user agent stylesheet。
  * ##### User Agent
    使用的浏览器。
  * ##### CSS reset
    默认样式已经不符合我们的需求，需要重置后，再自己定义。


## 六、常用内容标签
* ol+li (ordered list + list item) 有序列表
  ```html
  <ol>
    <li>早餐</li>
    <li>午餐</li>
    <li>晚餐</li>
  </ol>
  ```
* ul+li (unordered list + list item) 无效列表
  ```html
  <ul>
    <li>鸡腿</li>
    <li>牛扒</li>
    <li>饺子</li>
  </ul>
  ```
* dl+dt/dd (description list + term + data)
  ```html
  <dl>
    <dt>小王</dt>
    <dd>宅男</dd>
  </dl>
  ```
* pre (preview)
  ```html
  <pre>
    第一篇   中国社会各阶级的分析
  </pre>
  ```
* hr (horizontal route)
  水平分割线
* br (break)
  换行中断
  ```html
  谁是我们的敌人？<br>谁是我们的朋友？
  ```
* a (anchor)
  超链接
  ```html
    我的博客<a href=""></a>
  ```
* em (emphasis)
  表示语气的强调。
* strong
  定义重要的文本。
* code
  学过的编程语言有<code>HTML</code> <code>CSS</code> <code>JavaScript</code> <code>PHP</code> 。
  ```html
  <pre>
    <code>
      var text = '坚持';
      console.log(text);
    </code>
  </pre>
  ```
* q (quote的缩写)
  行内引用。
* blockquote
  换行引用。

资料来源： &copy;饥人谷
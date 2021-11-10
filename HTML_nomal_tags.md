# HTML常用标签

## 一、a 标签
作用
* 跳转到外部页面
* 跳转到内部锚点
* 跳转到邮箱或电话等
### 1. href(hyper reference: 超链接)
  * 网址 96
    * https://google.com
    * http://google.com
    * //google.com
  * 路径
    * /a/b/c以及a/b/c
    * index.html以及./index.html
  * 伪协议
    * javascript:(code);
    * mailto:@eamil.com
    * tel:phone number
  * id 
    * href=#xxx (页面跳转到id=xxx的位置)    
### 2. target
  * 内置名字
    * _blank 空白页打开 
    * _top 顶级页打开
    * _parent 父级页打开
    * _self 自身页打开
  * 程序员命名
    * window的name
    * iframe的name
### 3. download
  #### 作用 
    不用打开页面，而是下载页面
  #### 问题 
    不是所有浏览器都支持，尤其是手机浏览器可能不支持  
## 二、img 标签的用法
  #### 作用
    发出get请求，展示一张图片
  
  #### 属性
  * alt 请求不到图片时，用来提示
  * width：宽 /height：高 
  * src 资源路径
  
  #### 事件
    * onload 加载成功事件
    * onerror 加载失败事件（设置加载失败备用图片）
  
  #### 响应式
  ```css
  min-width: 100%;
  ```
  #### * 可替换元素
  什么是可替换元素？顾名思义就是可以被替换的元素。举例img：
  ```html
  <img src='/ZZZ'>
  ```
  我们并没有在<code>img</code>标签里写入任何内容，它的内容是浏览器根据scr路径下载的图片，并且用该图片替换掉<code>img</code>标签，而且浏览器在下载前并不知道图片的宽高。

  [MDN释义](https://developer.mozilla.org/en-US/docs/Web/CSS/Replaced_element)
  > 在CSS里，可替换元素(replaced element)的展现效果不是由CSS来控制的。这些元素是独立于CSS格式模型表现的外部对象。简而言之，它们是内容不受当前文档样式影响的元素。CSS 可以影响可替换元素的位置，但不会影响到可替换元素自身的内容。
## 三、table 标签

  相关标签 （thead、tbody、tfoot打乱顺序亦可，浏览器按语义排序）
  * table 表格
  * thead 表头部 
  * tbody 表身体
  * tfoot 表底部 
  * tr (table row) 表行
  * th (table head) 表头
  * td  (table data) 表数据

  相关样式
  * table-layout
    * auto 自动算法: 表格及单元格的宽度取决于其包含的内容。
    * fixed 表格和列的宽度通过表格的宽度来设置 
  * border-collapse 合并border空隙
  * border-spacing 设置border空隙
## 四、form 标签
  #### 作用
    发出get或post请求，然后刷新页面
  
  #### 属性
  * action 请求路径
  * autocomplete 配合<code>input</code>的<code>name</code>来使用浏览器保存的数据
  * method 设置请求方法
  * target 打开方式可参见<code>a</code>的<code>target</code>
  
  #### 事件
  * onsubmit 点击提交表单事件
## 五、input 标签
  #### 作用
    让用户输入内容
  
  #### 属性
  * 类型 type: button/radio/checkbox/email/file/hidden/
    text/number/password/tel/search/submit
  * 其他 name/autofocus/checked/disabled/maxlength/pattern/valur/placeholder
  
  #### 事件
  * onchange 输入值改变事件
  * onfocus 获得焦点事件
  * onblur 失去焦点事件
  #### 问题
  1. <code>input</code>的<code>submit</code>与<code>button</code>的<code>submit</code>有什么区别？
  <code>button</code>的<code>submit</code>里面可以设置内容标签，
  <code>input</code>的<code>submit</code>则不可以.
     ```html
     <input type=submit value='<strong>查询<strong>'/>✖
     <button type=submit><strong>查询<strong></button>✔
     ```
## 四、其他感想
  作为一名前端要考虑页面显示的内容的流畅性。

资料来源： &copy;饥人谷和[MDN HTML](https://developer.mozilla.org/zh-CN/docs/Web/HTML)
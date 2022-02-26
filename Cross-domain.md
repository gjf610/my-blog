# 跨域

## 1. 同源策略

### 同源定义

#### 源

源 = 协议 + 域名 + 端口号

如果两个URL的
* 协议
* 域名
* 端口号
完全一致，那么这两个URL就是同源

#### 举例

https://qq.com、https://www.baidu.com <b>不同源</b>
https://baidu.com、https://www.baidu.com <b>不同源</b>
完全一致才算同源

### 同源策略
不同域名、协议或者端口无法直接进行AJAX请求。同源策略只针对语浏览器端，浏览器一旦检测到请求结果不同源后，会堵塞请求结果。

### 优点
浏览器这样做的目的是：为了保护用户隐私！

#### 如果没有同源策略
以qq.com为例，已经通过了登陆验证，AJAX请求获取用户好友列表。

然后WeiXin.com兴起，用户也注册了一个账号。WeiXin.com就想qq.com你有这么多用户，不如拿一点给我。WeiXin.com就在用户登录时偷偷地向qq.com发请求，这样就拿到了用户的好友列表

#### 问题的根源
qq.com页面里的JS和WeiXin.com页面的JS发的请求几乎没有区别（Referer）有区别。如果后台开发者没有检查Referer，那么就完全没有区别。所以，没有同源策略，任何页面都能偷所有网站的数据，甚至网银里的钱。


### 2. 跨域
目前，如果非同源，共有三种行为受到限制。

>（1） Cookie、LocalStorage 和 IndexDB 无法读取。

>（2） DOM 无法获得。

>（3） AJAX 请求不能发送。

虽然这些限制是必要的，但是有时很不方便，合理的用途也受到影响。下面，我将详细介绍，如何规避上面三种限制。

若要实现服务器与客户端间的跨域通信，可以使用JSONP和CORS两种方法。
#### CORS 跨域

浏览器默认不同源之间不能相互访问数据，但是qq.com和WeiXin.com其实都是我的网站
我就是想要两个网站相互访问，浏览器为什么阻止？

因此浏览器厂商就提出CORS方法。如果要共享数据，需要提前声明！qq.com在响应头里写WeiXin.com可以访问。具体语法：

* Access-Control-Allow-Origin: WeiXin.com

具体文档查看：[MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS#%E7%AE%80%E5%8D%95%E8%AF%B7%E6%B1%82)

注意：CORS分为简单请求和复杂请求

#### JSONP 跨域

浏览器告诉服务器一个回调函数的名称，服务在返回的script标签里面调用这个回调函数，同时传进客户端需要的数据，这样返回的代码就能在浏览器上执行了。

步骤
* qq.com将数据写到/friends.js
* WeiXin.com用script标签引用/friends.js
* /friends.js执行WeiXin.com事先定义好的window.xxx的callback函数
* 然后WeiXin.com就能通过window.xxx获得了数据

```js
// /friends.js
window.xxx = (data) => {
    console.log(data)
}
const script = document.createElement('script')
script.src = 'http://qq.com/friends.js'
script.onload = () => {
    script.remove()
}
script.onerror = (err) => {
    console.error(err)
}
document.body.appendChild(script)
```

优化
window.xxx能不能改其他名字？
名字不重要，只要WeiXin.com定义的函数名和qq.com/friends.js执行的函数名是同一个即可！
那就把名字传给/friends.js
```js
const random = 'WeiXinCallBack' + Math.random()
window[random] = (data) => {
    console.log(data)
}
const script = document.createElement('script')
script.src = `http://qq.com/friends.js?callback=${random}`
script.onload = () => {
    script.remove()
}
script.onerror = () => {
    console.error(err)
}
document.body.appendChild(script)

```

封装JSONP

使用Promise封装
```js
function jsonp(url) {
  return new Promise((resolve, reject) => {
    const random = 'jianfengCallBack' + Math.random()
    window[random] = (data) => {
      resolve(data)
    }
    const script = document.createElement('script')
    script.src = `${url}?callback=${random}`
    script.onload = () => {
      script.remove()
    }
    script.onerror = () => {
      reject()
    }
    document.body.appendChild(script)
  })
}

jsonp('http://qq.com:8888/friends.js')
  .then(data => {
    console.log(data)
})
```
JSONP是什么？
当前浏览器不能使用CORS进行跨域，而使用的一种跨域方式。于是我们创建一个script标签就去请求另外一个网站的JS文件，JS文件会执行一个回调函数，回调函数就会有我们所需要的数据。
回调的名字是随机生成的，是一个随机数，我以callback的形式传给后台，后台会把名字返回给前端并执行。
优点：兼容IE
缺点：由于它是一个script标签，
1. 不能像读取AJAX精确的状态
2. 只能发get请求，不支持post
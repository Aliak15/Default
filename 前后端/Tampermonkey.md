[toc]

## console

```javascript
console.log(`第${i + 1}行: ${v}`);  //格式化打印

console.table({"a":2}); 	  	   //结构化打印

console.trace();  			 	   //携带当前调用堆栈函数信息打印

console.assert(1==='1')          //当为False时会提醒

console.count('计数的标签')       // 计数此标签执行的次数 
console.countReset('计数的标签')  // 归0

console.time('计时标签')				//开始计时
console.timeEnd('计时标签')				//即使结束返回时间
```

## fetch

```js
fetch('https://api.example.com/submit', {
  method: 'POST',
  credentials: 'include',  // 确保请求携带 Cookies
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN'
  },
  body: JSON.stringify({ name: 'John', age: 30 })  // 发送 JSON 数据
})
  .then(response => response.json())  // 解析 JSON 响应
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));

```

## 基础

```javascript
// 去除元素末尾的\r
payload2 = payload2.map(item => item.replace(/\r$/, ''))
// 移除变量
delete payload2

// js执行代码

eval('alert``')				//同步, 当前域
setTimeout('alert()',0)		 //异步, 全局域
Function('alert``')() 		 //同步, 全局域


// 字符串
'dassa'.substr(1,2) ==> as

// 正则
'dass78378a'.match('s(\\d+)')[0]	==>'s78378'
'dass78378a'.match('s(\\d+)')[1]	==>'78378'
```



## 1. 介绍

常见的元数据包含：

```ini
- @name			:油猴脚本的名称
- @namespace 	 :脚本的命名空间，用于确定脚本的唯一性
- @version 		 :脚本的版本号，用于脚本的更新
- @description 	 :脚本的描述信息
- @author 		 :作者
- @require 		 :定义脚本运行之前需要引入的外部 JS，比如：jQuery
# @match 		 :使用通配符执行需要匹配运行的网站地址
- @exclude 		 :排除匹配到的网站
- @grant 		 :指定脚本运行所属的权限
- @connect		 :用于跨域访问时指定的目标网站域名
# @run-at		 :指定脚本的运行时机  
- @icon 		 :用于指定脚本的图标，可以设置为图片 URL 地址或 base64 的字符串
```

## 2. 实战一下

以某一新闻网站的自动加载下一页为例进行说明

目标网站：`https://www.pingwest.com/`

首先，我们使用关键字 @match 指定匹配的网站 URL，使用 @grant 设置权限 GM_log，使用关键字@run-at 指定执行时机为页面加载完成，即：document-end

```js
// ==UserScript==
// @name         新闻查看更多
// @namespace    com.xag.more
// @version      0.1
// @license      GNU General Public License v3.0
// @description  自动查看下一页
// @author       xingag
// @match        目标网站
// @icon         图标icon地址
// @grant        GM_log
// @run-at       document-end
// ==/UserScript==
...
```

接着，添加一个定时任务，获取每一页底部的加载更多按钮

最后，判断元素存在时，执行点击操作即可

```js
...
(function() {
    'use strict';
    console.log("location.hostname:",location.hostname)
    if(location.hostname == "www.pingwest.com"){

        setInterval(() => {
            const more_element = document.querySelector(".load-more-box").querySelector("a")
            if(more_element){
                GM_log("元素存在，点击加载更多。。。")
                more_element.click();
            }
        }, 2000);
    }
})()
...
```

## 3. 常见 API

- ==打开新标签==

格式：GM_openInTab(url, options)	该 API 可用于打开一个新的标签页面

其中，第一个参数用于指定新标签页面的 URL 地址，第二个参数用于指定页面展示方式及焦点停留页面

```js
// 授权
// @grant        GM_openInTab

// 打开新页面
var onpenNewTap = function (){
    //打开百度页面
    //active:true，新标签页获取页面焦点
    //setParent :true:新标签页面关闭后，焦点重新回到源页面
    newTap = GM_openInTab("https://www.baidu.com",{ active: true, setParent :true});
...
```

- ==跨域请求==

在授予 GM_xmlhttpRequest 权限之后，就可以跨域发送请求了

PS：第一次跨域请求时，会弹出请求对话框，需要选中允许，才能正常进行跨域请求

```js
// 授权
// @grant        GM_xmlhttpRequest

...
GM_xmlhttpRequest({
    url:"http://www.httpbin.org/post",
    method:'POST',
    headers: {
    "content-type": "application/json"
      },
    data:"",
    onerror:function(res){
        console.log(res);
    },
    onload:function(res){
        console.log(res);
    }
});
...
```

## 4. HOOK-btoa

```JS
// ==UserScript==
// @name         Hookbtoa
// @namespace    https://scrape.cuiqingcai.com/
// @version      0.1
// @description  Hook Base64 encode function
// @author       Germey
// @match        https://scrape.cuiqingcai.com/login1
// @grant        none
// ==/UserScript==
(function () {
    'use strict'
    function hook(object, attr) {
        var func = object[attr]
        object[attr] = function () {
            console.log('hooked', object, attr)
            var ret = func.apply(object, arguments)
            debugger
            return ret
        }
    }
    hook(window, 'btoa')
})()
```


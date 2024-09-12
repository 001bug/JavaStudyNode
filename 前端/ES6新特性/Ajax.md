**Ajax概述**
**Ajax**（Asynchronous JavaScript and XML）是一种用于在网页中进行**异步数据交互**的技术。它允许网页在不刷新整个页面的情况下，从服务器请求数据并更新页面的部分内容，从而提高了用户体验

**Ajax原理**
![](assest/Pasted%20image%2020240910175609.png)

**Ajax使用的简单例子**
```javascript
// 创建一个 XMLHttpRequest 对象
let xhr = new XMLHttpRequest();

// 配置请求类型和 URL
xhr.open('GET', 'https://example.com/data', true);

// 当请求状态改变时的回调函数
xhr.onreadystatechange = function() {
  if (xhr.readyState === 4 && xhr.status === 200) {
    // 处理服务器返回的数据
    console.log(xhr.responseText);
  }
};

// 发送请求
xhr.send();
```
**readyState**
`readyState` 属性表示 **XMLHttpRequest** 对象的当前状态，它有 5 种不同的值，分别对应请求的不同阶段：
- **0 (UNSENT)**: 请求还未初始化，即调用 `open()` 方法之前的状态。
- **1 (OPENED)**: 请求已被打开，`open()` 方法已调用，但还未发送请求。
- **2 (HEADERS_RECEIVED)**: 请求已发送，并且服务器已接收该请求，响应头也已经被接收到。
- **3 (LOADING)**: 响应体正在下载中，部分数据已经接收，但还没有完全接收完毕。
- **4 (DONE)**: 请求完成，服务器的响应已完全接收。此时可以处理服务器返回的数据

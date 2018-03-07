# BOM - Browser Object Model

## 1. window对象
BOM的核心对象是window对象，他表示一个浏览器的实例，它有双重角色：
1. 通过javascript访问浏览器窗口的一个接口
2. 又是ECMAScript规定的Global对象（因此有权访问parseInt()等方法）

### 1.1 窗口位置
获取窗口左上角距离桌面左上角的距离
```
var leftPos = (typeof window.screenLeft == "number") ?
                  window.screenLeft : window.screenX;
var topPos = (typeof window.screenTop == "number") ?
                  window.screenTop : window.screenY;
```

### 1.2 窗口大小
window.innerWidth  
window.innerHeight  
或者  
document.body.clientWidth  
document.body.clientHeight  

### 1.3 导航和打开窗口
const win = window.open(url, target)  
alert(win.opener === window) // 确定是不是当前窗口打开的  
// 可以通过win是否为null 判断是否成功打开了窗口  
win.close() // 可以关闭打开的窗口  
alert(win.closed)  

### 1.4 间歇调用和超时调用
[阮一峰定时器详解](http://www.ruanyifeng.com/blog/2018/02/node-event-loop.html)
```
var timeoutId = setTimeout(function() {
    alert("Hello world!");
}, 1000);
clearTimeout(timeoutId);
```
取消interval
```
var num = 0;
var max = 10;
var intervalId = null;

function incrementNumber() {
    num++;

    //if the max has been reached, cancel all pending executions
    if (num == max) {
        clearInterval(intervalId);
        alert("Done");
    }
}

intervalId = setInterval(incrementNumber, 500);
```
### 1.5 系统对话框
alert, confirm, prompt  
通过这几个打开的对话框是同步和模态的：显示这些对话框的时候代码会停止执行，关掉后恢复执行  

通过window.print()打开打印对话框  
通过window.find(str)执行浏览器的查询，很方便

## 2 location对象
window.location和document.location引用的是同一个对象
<html>
<table class="dataintable">
  <tbody><tr>
    <th style="width:30%">属性</th>
    <th>描述</th>
  </tr>
  <tr>
    <td><a href="/jsref/prop_loc_hash.asp">hash</a></td>
    <td>设置或返回从井号 (#) 开始的 URL（锚）。</td>
  </tr>
  <tr>
    <td><a href="/jsref/prop_loc_host.asp">host</a></td>
    <td>设置或返回主机名和当前 URL 的端口号。</td>
  </tr>
  <tr>
    <td><a href="/jsref/prop_loc_hostname.asp">hostname</a></td>
    <td>设置或返回当前 URL 的主机名。</td>
  </tr>
  <tr>
    <td><a href="/jsref/prop_loc_href.asp">href</a></td>
    <td>设置或返回完整的 URL。</td>
  </tr>
  <tr>
    <td><a href="/jsref/prop_loc_pathname.asp">pathname</a></td>
    <td>设置或返回当前 URL 的路径部分。</td>
  </tr>
  <tr>
    <td><a href="/jsref/prop_loc_port.asp">port</a></td>
    <td>设置或返回当前 URL 的端口号。</td>
  </tr>
  <tr>
    <td><a href="/jsref/prop_loc_protocol.asp">protocol</a></td>
    <td>设置或返回当前 URL 的协议。</td>
  </tr>
  <tr>
    <td><a href="/jsref/prop_loc_search.asp">search</a></td>
    <td>设置或返回从问号 (?) 开始的 URL（查询部分）。</td>
  </tr>
  </tbody></table>
</html>
位置操作：
```
window.location.assign("http://www.mozilla.org"); // or
window.location = "http://www.mozilla.org"; // or
window.location.href = "http://www.mozilla.org";
```
强制从服务器重新加载
```
window.location.reload(true);
```
## 3 navigator对象
window.navigator
```
txt = "<p>Browser CodeName: " + navigator.appCodeName + "</p>";
txt+= "<p>Browser Name: " + navigator.appName + "</p>";
txt+= "<p>Browser Version: " + navigator.appVersion + "</p>";
txt+= "<p>Cookies Enabled: " + navigator.cookieEnabled + "</p>";
txt+= "<p>Platform: " + navigator.platform + "</p>";
txt+= "<p>User-agent header: " + navigator.userAgent + "</p>";
txt+= "<p>User-agent language: " + navigator.systemLanguage + "</p>";
```

## history对象
window.history
```
history.go(-1)
history.go(2)
history.go('wrox.com') // 跳转到最近的
if (history.length === 0) // 判断是否是打开窗口个的第一个页面
```

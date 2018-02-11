# 基本概念
## 严格模式
```
"use strict"
```
告诉javascript引擎切换到严格模式
也可以定义在函数中
```
function doSomething () {
  "use strict"
}
```
## 数据类型
1. undefined
2. null
3. boolean
4. number
5. string
6. object

```
const x = null
alert(typeof x) // "object"
```

```
var num1 = Number("Hello world!");  //NaN
var num2 = Number("");              //0
var num3 = Number("000011");        //11
var num4 = Number(true);            //1
```

```
var num1 = parseInt("1234blue");    //1234
var num2 = parseInt("");            //NaN
var num3 = parseInt("0xA");         //10 - hexadecimal
var num4 = parseInt(22.5);          //22
var num5 = parseInt("70");          //70 - decimal
var num6 = parseInt("0xf");         //15 – hexadecimal
```

```
function sayHi() {
  alert("Hello " + arguments[0] + ", " + arguments[1]);
}
```

## 块级作用域
在{}里面用var声明的变量不会被销毁  
```
if (true) {
  var x = 123
}
console.log(x) // 123

for (var i = 0; i < 10; i++) {
}
console.log(i) // 10
```

而es6的let已经对这个进行了优化，块级作用域里面用let定义的变量会正常被销毁


## 垃圾收集
### 方法1：标记清除
主流
### 方法2：引用计数
有循环引用的问题
### 性能问题
垃圾收集时间间隔是一个重要的问题
IE6：根据内存分配量运行的，达到临界值，就运行；但是有的程序脚本上来就达到了临界值，导致频繁运行垃圾收集；  
IE7重写了垃圾收集流程：初始临界值一样，触发收集的时候，如果回收量低于15%，则临界值加倍；如果回收大于85%，临界值重置回默认值。
### 管理内存
通过“解除引用”，主要针对全局变量；解除引用的内容，会在下一次垃圾收集时回收

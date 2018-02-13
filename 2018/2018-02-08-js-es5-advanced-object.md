# 引用类型
## Object
没什么要mark的
## Array
### Array.isArray
instanceof 只会判断Prototype，这样的话，别的框架如果实现了另外一个array，instanceof就会认为不是数组了  
但是isArray是es5才有的一个可靠的判断方法，如下：
```
var iframeEl = document.createElement('iframe');
document.body.appendChild(iframeEl);
iframeArray = window.frames[window.frames.length - 1].Array;

var array1 = new Array(1,1,1,1);
var array2 = new iframeArray(1,1,1,1);

console.log(array1 instanceof Array);  // true    
console.log(Array.isArray(array1));  // true

console.log(array2 instanceof Array);  // false    
console.log(Array.isArray(array2));  // true    
```

### 队、栈操作
- push
- pop
- unshift
- shift

### 操作方法
- slice (begin, end)，提取，返回新数组
- splice (pos, num, value1?, value2?, ...) 修改原数组

### 迭代方法
- every
- filter
- forEach
- map
- some
(item, index, array) => {}

### 缩小方法
- reduce 从前往后
- reduceRight 从后往前  
参数1，(pre, cur, index, array) => 下一个的pre  
参数2，初始值，可选：
  - 没有：index从1开始，第一次pre是array[0]
  - 有：index从0开始，第一次pre是传入的初始值

没有参数2样例：
```
var values = [1,2,3,4,5];
var sum = values.reduce(function(prev, cur, index, array){
    return prev + cur;
});
alert(sum); // 15
```
有参数2样例：
```
const a = ['a', 'b', 'c', 'd', 'e']
const ret = a.reduce((pre, cur, index, ary) => {
        pre[index]=cur
        return pre
}, {})
console.lot(ret) // { '0': 'a', '1': 'b', '2': 'c', '3': 'd', '4': 'e' }
```

## Date
## RegExp
## Function
### 函数声明
下面的代码是work的，解析器会率先读取函数声明，并使其在执行任何代码前可用
```
alert(sum(10, 10))
function sum (a, b) {return a + b}
```
### 函数内部属性 arguments
除了表示传入的参数，arguments.callee也是这个函数本身，可以用在递归时调用arguments.callee()，解除函数执行和函数名称的紧耦合
### 函数内部属性 this
this引用的是函数据以执行的环境的对象：
全局下调用函数，this就是window
某个引用对象x调用函数，this就是x
```
window.color = "red";
var o = { color: "blue" };

function sayColor(){
    alert(this.color);
}

sayColor();     //red

o.sayColor = sayColor;
o.sayColor();   //blue
```
### caller

可以通过函数内部的caller()查看是哪个函数调用了自己
下面代码会打印出outer函数的源代码
```
function outer(){
  inner();
}

function inner(){
  alert(inner.caller);
}

outer();
```
由于上面提到arguments.callee也是函数本身，可以通过arguments.callee.caller来解耦
### length
调用函数的length，可以知道函数希望接收几个参数：
```
function sayName(name){
  alert(name);
}      

function sum(num1, num2){
  return num1 + num2;
}

function sayHi(){
  alert("hi");
}

alert(sayName.length);  //1
alert(sum.length);      //2
alert(sayHi.length);    //0
```

### prototype
所有的引用类型，prototype是保存他们实例方法的所在之处  
在ES5中，prototype是不可枚举的，所以通过for in 无法访问到

### apply和call
apply，传入this，和数组形式的参数，如下:
```
function sum(num1, num2){
  return num1 + num2;
}

function callSum1(num1, num2){
  return sum.apply(this, arguments);
}

function callSum2(num1, num2){
  return sum.apply(this, [num1, num2]);
}

alert(callSum1(10,10));   //20
alert(callSum2(10,10));   //20
```
call和apply功能一样，就是传参形式不一样，参数不是数组：
```
function sum(num1, num2){
  return num1 + num2;
}

function callSum(num1, num2){
  return sum.call(this, num1, num2);
}

alert(callSum(10,10));   //20
```
### bind
```
window.color = "red";
var o = { color: "blue" };

function sayColor(){
    alert(this.color);
}
var objectSayColor = sayColor.bind(o);
objectSayColor();   //blue
```

## 基本包装类型
为了便于操作基本类型，还提供了3种特殊的引用类型:
- Boolean
- Number
- String  

理解：  
```
const s1 = 'some text'
const s2 = s1.substring(2)
```
实际上，在读取模式下访问基本类型时，就会创建对应的基本包装类型的一个对象，从而方便数据的操作，可以想象成进行如下的操作：
```
let s1 = new String('some text')
const s2 = s1.substring(2)
s1 = null
```
最后置为null，一个是回收机制，一个是s1最后还是基本类型，而不是引用类型  
所以下面的操作不会成立也就是自然的了：
```
const s1 = 'some text'
s1.color = 'red'
console.log(s1.color) // undefined
```
注意下面的例子，一个是基本类型，是一个是引用类型，当是引用类型的时候，即使字符串是空，if (newStr)也是true
```
const str = 'some text'
const newStr = new String('some text')
console.log(str instanceof String) // false
console.log(newStr instanceof String) // true
```
另外一个需要注意的是：使用new调用基本包装类型的构造函数，与直接调用同名的转型函数是不一样的：new是引用，直接调用同名还是基本类型
```
const value = '25'
const number = Number(value)
console.log(typeof number) // number

const obj = new Number(value)
console.log(typeof obj) // object
```
### Boolean类型
注意new Boolean()是object，if判断失效的问题
### Number类型
- toString(进制)
- toFixed(保留几位小数)
- toExponential(科学计数法中小数的位数)
- toPrecision(一共几位字符) // 有的会用科学计数法

### String类型
- slice(start, end[optional]) // 支持负数
- substring(start, end[optional]) // 负数缺省为0
- substr(start, length[optional]) // 支持负数
- trim() // 删除前后空白

## 单体内置对象
> 由ECMAScript 实现提供的，不依赖于宿主环境，在程序执行前就已经存在

开发人员不用显示的实例化对象，因为它们已经实例化，比如Object，Array，String  
有Global和Math两个，在大多数ECMAScript实现中都不能直接访问Global对象（不过Web浏览器实现了承担该角色的window对象）；全局变量和函数都是Global的属性；
### Global对象
比如isNaN parseInt，URI编码等都是
#### URI编码
- encodeURI 用于整个URI，不会对本身属于URI的特殊字符进行编码，比如http://
- encodeURIComponent 用于URI中的某一段，会对任何非标准字符进行编码，包括http://

这两个方法分别对应：
- decodeURI 不会解码http里面可能有的字符，比如: // #
- decodeURIComponent 会解码任何字符

### eval
把变量里面的js代码执行
严格模式下，不能访问eval里面定义的变量

### window
后面探讨

### Math对象
各种函数

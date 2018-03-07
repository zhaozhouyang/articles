# 函数表达式
## 函数声明提升
```
sayHi();
function sayHi(){
    alert("Hi!");
}
```
## 函数名称
```
function functionName(){
    //noop
}

//works only in Firefox, Safari, Chrome, and Opera
alert(functionName.name); //"functionName"
```
## 拉姆达函数（匿名函数）
创建一个函数并将它赋值给变量  
这类函数没有“函数生命提升”的特性，下面的代码将出错：  
```
saiHi()
const sayHi = function () {
  alert('Hi!')
}
```
## 递归函数解耦
通过callee
```
function factorial(num){
    if (num <= 1){
        return 1;
    } else {
        return num * arguments.callee(num-1);
    }
}

var anotherFactorial = factorial;
factorial = null;
alert(anotherFactorial(4));  //24
```
## 闭包
闭包是函数，是一个有权限访问另一个函数作用域中的变量的函数。  
常见形式就是在一个函数内部创建另一个函数
### 闭包的作用域链
![image](https://raw.githubusercontent.com/zhaozhouyang/markdown-photots/master/css/js-function-scope.png)
## 关于this对象
匿名函数的this很可能指向global（window），如果匿名函数中需要用到当前的this，建议把this赋值到另一个变量中
## 模仿块级作用域
用
```
(function(){

})()
```
模拟实现，es6中已经有了
## 私有变量和特权方法
```
function Person(name){

    this.getName = function(){
        return name;
    };

    this.setName = function (value) {
        name = value;
    };
}

var person = new Person("Nicholas");
alert(person.getName());   //"Nicholas"
person.setName("Greg");
alert(person.getName());   //"Greg"
```
> 更多内容看书

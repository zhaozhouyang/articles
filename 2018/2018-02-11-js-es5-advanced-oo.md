# 面向对象编程
## 属性类型
### 1 数据属性  
创建数据属性时，有4个描述其行为的特性：  
- configurable  
  默认ture，表示能否通过delete删除该属性从而重新定义该属性；或者能把该属性修改为”访问器属性”
- enumerable  
  默认true，表示能否通过for-in循环返回该属性
- writable  
  默认true
- value  
  默认undefined，读取属性的时候从这个地方读取，写入的时候写在这里

```
const person = {
  name: 'jojo'
}
```
创建了一个名为name的属性，它的value是jojo，其他三个描述属性的特性都是默认值  
如果要修改默认值，必须用es5的Object.defineProperty(obj, name, descriptor)
```
const person = {}
Object.defineProperty(person, 'name', {
    writable: false,
    value: 'jojo'
})
console.log(person.name) // jojo
person.name = 'test'
console.log(person.name) // jojo
```
上述的修改不会生效  
```
const person = {}
Object.defineProperty(person, 'name', {
    configurable: false,
    value: 'jojo'
})
console.log(person.name) // jojo
delete person.name
console.log(person.name) // jojo

Object.defineProperty(person, 'name', {
    configurable: true,
    value: 'jojo'
}) // 抛出错误
```
上述delete不会生效，并且由于configurable是false，再次调用defineProperty时会抛出错误
### 2 访问器属性
访问器属性不包含值，但是包含一对儿getter和setter函数（不是必须的）  
访问器属性必须通过defineProperty来定义
- configurable  
  默认ture，表示能否通过delete删除该属性从而重新定义该属性；或者能把该属性修改为”访问器属性”
- enumerable  
  默认true，表示能否通过for-in循环返回该属性
- get  
  默认undefined
- set  
  默认undefined
```
const book = {
  _year: 2004,
  edition: 1
}
Object.defineProperty(book, 'year', {
    get: function () {
      return this._year
    }
    set: function (newValue) {
      if (newValue > 2004) {
        this._year = newValue
        this.edition += newValue - 2004
      }
    }
})
```

### 定义多个属性
通过Object.defineProperties(object, {...})

## 创建对象
### 1 工厂模式
```
function createPerson(name, age, job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
        alert(this.name);
    };    
    return o;
}

var person1 = createPerson("Nicholas", 29, "Software Engineer");
var person2 = createPerson("Greg", 27, "Doctor");

person1.sayName();   //"Nicholas"
person2.sayName();   //"Greg"
```
### 2 构造函数模式
必须使用new操作符
```
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function(){
        alert(this.name);
    };    
}

var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");

person1.sayName();   //"Nicholas"
person2.sayName();   //"Greg"

alert(person1 instanceof Object);  //true
alert(person1 instanceof Person);  //true
alert(person2 instanceof Object);  //true
alert(person2 instanceof Person);  //true

alert(person1.constructor == Person);  //true
alert(person2.constructor == Person);  //true

alert(person1.sayName == person2.sayName);  //false        
```
new的时候经历4个步骤
1. 创建一个新的对象Object
2. 将构造函数（Person）的作用域赋给新对象（因此this就是这个新的Object）
3. 执行构造函数（Person）中的代码
4. 返回新对象

其中person1.constructor指向构造函数Person

如果直接把Person当做函数调用，如下，this指向window，直接赋值到了window上面
```
Person("Greg", 27, "Doctor");  //adds to window
window.sayName();   //"Greg"
```

#### 构造函数模式的问题
函数定义的问题  
1. 如果函数定义在构造函数里面，那么每一个Person的实例都会包含一个不同的同名Function，没有必要
2. 如果函数定义在外面Global，函数里面通过this.x=x引用，那么完全没有封装性可言

### 3 原型模式
- 每个函数都有一个prototype属性，指向一个原型对象，这个原型对象会自动获得一个constructor属性，其他的属性都是从Object继承过来的     
- 所有的实例都有一个\__proto__属性，指向（共享）这个原型对象
- p.isPrototypeOf(obj) 查看obj对象的原型(\__proto\__)是否是p，其实等价于(因为一些浏览器无法访问\__proto__)
> obj.\__proto__ === p  

- obj.hasOwnProperty(name) 判断name是否是obj自己的属性
- name in obj 判断obj是否有（哪怕是原型对象中的）name属性，所以可以通过!obj.hasOwnProperty(name) && (name in obj) 判断name是否只是存在于原型中

```
function Person(){
}

Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
    alert(this.name);
};

var person1 = new Person();
person1.sayName();   //"Nicholas"

var person2 = new Person();
person2.sayName();   //"Nicholas"

alert(person1.sayName == person2.sayName);  //true

alert(Person.prototype.isPrototypeOf(person1));  //true
alert(Person.prototype.isPrototypeOf(person2));  //true

//only works if Object.getPrototypeOf() is available
if (Object.getPrototypeOf){
    alert(Object.getPrototypeOf(person1) == Person.prototype);  //true
    alert(Object.getPrototypeOf(person1).name);  //"Nicholas"
}
```
![image](https://raw.githubusercontent.com/zhaozhouyang/markdown-photots/master/css/js-oo-proto.png)

当读取一个对象的某个属性的时候，首先搜索实例本身，如果没有，搜索原型对象。
```
function Person(){
}

Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
    alert(this.name);
};

var person1 = new Person();
var person2 = new Person();

person1.name = "Greg";
alert(person1.name);   //"Greg" from instance
alert(person2.name);   //"Nicholas" from prototype

delete person1.name
alert(person1.name);   //"Nicholas" from instance
```
图解：
![image](https://raw.githubusercontent.com/zhaozhouyang/markdown-photots/master/css/js-oo-proto2.png)

#### 更简单的原型语法
如下，可以直接Person.prototype = {}  
但是这样的话constructor不再指向Person了，因为已经重写了那个constructor指向Person的prototype了
```
function Person(){
}

Person.prototype = {
    name : "Nicholas",
    age : 29,
    job: "Software Engineer",
    sayName : function () {
        alert(this.name);
    }
};

var friend = new Person();

alert(friend instanceof Object);  //true
alert(friend instanceof Person);  //true
alert(friend.constructor == Person);  //false
alert(friend.constructor == Object);  //true
```

#### 原生对象的原型
```
alert(typeof Array.prototype.sort);         //"function"
alert(typeof String.prototype.substring);   //"function"

String.prototype.startsWith = function (text) {
    return this.indexOf(text) == 0;
};

var msg = "Hello world!";
alert(msg.startsWith("Hello"));   //true
```
### 4 组合使用原型模式和构造函数模式
函数共享，属性值不共享  
注意，在Person函数里面，this可以访问后面定义的在prototype的属性和方法，不受代码顺序的影响
```
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.friends = ["Shelby", "Court"];
}

Person.prototype = {
    constructor: Person,
    sayName : function () {
        alert(this.name);
    }
};

var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");

person1.friends.push("Van");

alert(person1.friends);    //"Shelby,Court,Van"
alert(person2.friends);    //"Shelby,Court"
alert(person1.friends === person2.friends);  //false
alert(person1.sayName === person2.sayName);  //true
```
## 继承
许多oo语言支持两种继承：
- 接口继承：只继承函数方法签名
- 实现继承：继承实际的方法

ECMAScript只支持“实现继承”
### 1 原型链继承
简单回顾：
- 构造函数：每个构造函数有一个原型对象prototype
- 原型对象：原型对象包含一个指向构造函数的指针constructor
- 实例：构造函数的实例，包含一个指向原型对象的内部指针\__proto__

实现继承：
通过把子类的prototype设置为父类的实例即可实现原型链，instanceof和isPrototypeOf可正常使用

```
function SuperType(){
    this.property = true;
}

SuperType.prototype.getSuperValue = function(){
    return this.property;
};

function SubType(){
    this.subproperty = false;
}

//inherit from SuperType
SubType.prototype = new SuperType();

SubType.prototype.getSubValue = function (){
    return this.subproperty;
};

var instance = new SubType();
alert(instance.getSuperValue());   //true

alert(instance instanceof Object);      //true
alert(instance instanceof SuperType);   //true
alert(instance instanceof SubType);     //true

alert(Object.prototype.isPrototypeOf(instance));    //true
alert(SuperType.prototype.isPrototypeOf(instance)); //true
alert(SubType.prototype.isPrototypeOf(instance));   //true
```

这样的话，搜索属性时按照实例->实例原型->原型的原型->...的顺序实现继承和多态

![image](https://raw.githubusercontent.com/zhaozhouyang/markdown-photots/master/css/js-oo-proto4.png)

> 原型链存在的问题：这样的话，由于prototype设置为了父类的实例（含有属性），等于所有有value的属性又成了共享的了

### 2 借用构造函数
通过在子类的构造函数中，通过call或者apply调用父类的构造函数的同时，把自己的this传递给父类构造函数，来解决“原型链”继承存在的问题
```
function SuperType(name){
    this.name = name;
}

function SubType(){  
    //inherit from SuperType passing in an argument
    SuperType.call(this, "Nicholas");

    //instance property
    this.age = 29;
}

var instance = new SubType();
alert(instance.name);    //"Nicholas";
alert(instance.age);     //29
```

### 3 组合继承
把上述两种组合起来，就是最经典的继承形式：
```
function SuperType(name){
    this.name = name;
    this.colors = ["red", "blue", "green"];
}

SuperType.prototype.sayName = function(){
    alert(this.name);
};

function SubType(name, age){  
    SuperType.call(this, name);

    this.age = age;
}

SubType.prototype = new SuperType();

SubType.prototype.sayAge = function(){
    alert(this.age);
};

var instance1 = new SubType("Nicholas", 29);
instance1.colors.push("black");
alert(instance1.colors);  //"red,blue,green,black"
instance1.sayName();      //"Nicholas";
instance1.sayAge();       //29


var instance2 = new SubType("Greg", 27);
alert(instance2.colors);  //"red,blue,green"
instance2.sayName();      //"Greg";
instance2.sayAge();       //27
```

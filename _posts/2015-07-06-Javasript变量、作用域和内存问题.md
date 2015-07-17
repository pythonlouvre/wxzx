---
layout: post
title: Javascript中的变量、作用域和内存
tags: Javascript
---

---
layout: post
title: Javascript中的变量、作用域和内存
preview_previewType: presentation
tags: Javascript
---

# Javascript中的变量、作用域和内存

----

## 基本类型和引用类型

### 动态属性

基本类型

```javascript
var name = "Nicholas";
name.age = 27;
alert(name.age);					//undefined
```

引用类型

```javascript
var person = new Object();
person.name = "Nicholas";
alert(person.name);				//"Nicholas"
```

----



### 复制变量值


基本类型

```javascript
var num1 = 5;
var num2 = num1;            //num2和num1是两块不同的内存地址
```

引用类型

```javascript
var obj1 = new Object();
var obj2 = obj1;
obj1.name = "Nicholas";
alert(obj2.name);			//"Nicholas"

```

----


### 传递参数

>javascript中没有引用传递，只有值传递


----


基本类型

```javascript
function addTen(num) {
      num += 10;
	   return num; 
}
var count = 20;
var result = addTen(count); 
alert(count); //20,没有变化 
alert(result); //30
```

引用类型

```javascript
function setName(obj) {
    obj.name = "Nicholas";
}
var person = new Object();
setName(person);
alert(person.name);    //"Nicholas"
```

----

引用类型的值传递

```javascript
function setName(obj) { 
	obj.name = "Nicholas";
	obj = new Object();
	obj.name = "Greg";
}
var person = new Object();
setName(person);
alert(person.name);    //"Nicholas"
```

----


### 检测类型

基本类型

```javascript
typeof
var s = "Nicholas";
var b = true;
var i = 22;
var u;
var n = null;
var o = new Object();
alert(typeof s);//string
alert(typeof i);//number
alert(typeof b);//boolean
alert(typeof u);//undefined
alert(typeof n);//object
alert(typeof o);//object
```

----


引用类型

instanceof
```javascript
alert(person instanceof Object); // 变量 person 是 Object 吗?
alert(colors instanceof Array); // 变量 colors 是 Array 吗? 
alert(pattern instanceof RegExp); //变量pattern是RegExp吗?
```

----


## 执行环境及作用域

### 延长作用域链

1. try-catch中的catch块
2. with语句

```javascript
function buildUrl() {
    var qs = "?debug=true";
    with(location){
        var url = href + qs;
    }
    return url; 
}

```

----

### 没有块级作用域

if语句块中没有独立的作用域

```javascript
if (true) {
        var color = "blue";
}
alert(color);    //"blue"
```

for语句同理

```javascript
 for (var i=0; i < 10; i++){
        doSomething(i);
}
alert(i);      //10
```

----


1. 声明变量
在函数内部如果不使用var声明的变量，则自动添加至全局作用域

```javascript
function add(num1, num2) {
        var sum = num1 + num2;
        return sum; 
}
var result = add(10, 20); //30
alert(sum); //由于 sum 不是有效的变量,因此会导致错误
```

```javascript
function add(num1, num2) {
    sum = num1 + num2;
    return sum; 
}
var result = add(10, 20); //30 alert(sum); //30
```

----




2. 查询标识符

```javascript
var color = "blue";
function getColor(){
    return color;
}
alert(getColor());  //"blue"
```

在这个搜索过程中,如果存在一个局部的变量的定义,则搜索会自动停止:

```javascript
var color = "blue";
function getColor(){
    var color = "red";
    return color;
}
alert(getColor());  //"red"
```

----

## 垃圾收集
JavaScript 具有自动垃圾收集机制，用于标识无用变量的策略可能会因实现而异,但具体到浏览器中的实现,则通常有两个策略。

----

### 标记清除
最常用的垃圾收集策略

1. 进入环境
2. 离开环境

到 2008 年为止,IE、Firefox、Opera、Chrome 和 Safari 的 JavaScript 实现使用的都是标记清除式的 垃圾收集策略(或类似的策略),只不过垃圾收集的时间间隔互有不同。

----

### 引用计数
引用计数的含义是跟踪记录每个值被引用的次数。
引用次数变成 0 时,释放。


----

循环引用:

```javascript
function problem(){
    var objectA = new Object();
    var objectB = new Object();
	 objectA.someOtherObject = objectB; 
    objectB.anotherObject = objectA;
}
```

通过各自的属性相互引用;
函数多次调用,无法回收。
Netscape 在 Navigator 4.0中放弃了引用计数方式

----

### 性能问题
确定垃圾收集的时间间隔是一个非常重要的问题。

IE6的垃圾收集器:
1. 256 个变量
2. 4096个对象(或数组)字面量和数组元素
3. 64KB字符串。


----


1. IE7中的各项临界值在初始时与IE6 相等。

2. 如果垃圾收集例程回收的内存分配量低于15%,则变量、字面量和(或)数组元素的临界值就会加倍。

3. 如果例程回收了85%的内存分配量,则将各种临界值重置回默认值。


----

### 管理内存
分配给 Web 浏览器的可用内存数量通常要比分配给桌面应用程序的少。

----

优化内存占用的方式--解除引用(dereferencing)。

```javascript
function createPerson(name){
    var localPerson = new Object();
    localPerson.name = name;
    return localPerson;
}
var globalPerson = createPerson("Nicholas"); 
// 手工解除 globalPerson 的引用
globalPerson = null;
```



























































































































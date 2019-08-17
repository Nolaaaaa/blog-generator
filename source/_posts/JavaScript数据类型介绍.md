---
title: JavaScript数据类型介绍
date: 2018-04-26 16:06:37
tags: [数据类型,JavaScript]
categories: 前端
---

最新的 ECMAScript 标准定义了JS的 7 种数据类型<escape><!-- more --></escape>，其中包括：
6 种基本类型:Boolean、Null、Undefined、Number、String、Symbol (ECMAScript 6 新定义)；
1个引用类型： Object（包含狭义的对象，Array，function）。
两种类型的值传递方式：基本类型是值传递，引用类型是引用传递。
```javascript
// 第一题 引用传递
function test(m) { m.k = 5 }
var m = {
  k: 30
}
test(m)
console.log(m.k)  // 5

// 第二题 值传递
function test(m) { m = 5 }
var m = 30
test(m)
console.log(m)  // 30
```

### 1  Boolean
1. Bealean类型的值有两个：true、false
2. 所有其他数据类型都有对应的Boolean值，使用Boolean(value)方法可以强制转换任意值为boolean类型
```javascript
console.log(Boolean("Hello")); //true
```

### 2  Null
1. Null类型的值只有一个：null
2. Typeof(null)时返回“object”：这是历史原因造成的，但是可以理解成：unll表示一个空对象(Object)的引用
```javascript
typeof(null); // “object”
```

### 3  Undefined
1. undefined类型的值只有一个：undefined
2. 只进行了声明而未初始化的变量，其值都是
```javascript
undefined var m;
console.log(m);    //undefined
```
3. undefined值派生自null值，两者都是表示“没有值”。两者相等，但是由于数据类型不一样，两者不全等（==是相等操作符会对数据类型进行转化，===是全等操作符不会转化数据类型）
```javascript
console.log(undefined == null); //true 
console.log(undefined === null); //false
```
4. 如何区分undefined和null：表示一个还没赋值的**对象**用null；表示一个还没赋值的**字符串、数字、布尔、symbol**时用undefined

### 4  Number
1. Number包括：整数和小数(如:1/1.1)、科学计数法(如:1.11e2)、二进制(如:0b11)、八进制(如:011或0o11)、十六进制(如:0x11)
2. 保存浮点数所需的内存空间是整数值的2倍
```javascript
* 浮点数值相加结果会不准确 console.log(0.1+0.2);    //0.30000000000000004
```

3. NaN是一个特殊的Number值；它的存在是为了避免程序直接报错；NaN的任何操作都会返回NaN；NaN与任何值都不相等，包括它自身 
```javascript
console.log(NaN === NaN);  //false；
```

### 5  String
1. 字符串String类型是由引号括起来的一组由16位Unicode字符组成的字符序列。
2. 用单引号（’ ‘）或双引号（” “）皆可，但是必须双引号配双引号，单引号配单引号
3. 任何字符串的长度都是可以通过length属性来取得 var a=“nihao”;
```javascript
console.log(a.length);
//5
```

4. ECMAScript中字符串是不可变，如要改变该变量保存的字符串，首先要销毁原来的字符串，再用另一个包含新值的字符串填充该变量

### 6  Symbol
1. symbol是基本类型，实现唯一标识　　
2. 通过调用symbol(name)创建symbol　　name
3. 我们创建一个字段，仅为知道对应symbol的人能访问，使用symbol很有用　　
4. symbol不会出现在for..in结果中　　
5. 使用symbol(name）创建的symbol，总是不同，即使name相同。如果希望相同名称的symbol相等，则使用全局注册　　
6. symbol.for(name)返回给定名称的全局symbol，多次调用返回相同symbol　　
7. Javascript有系统symbol，通过Symbol.*访问。我们能使用他们去修改一些内置行为

### 7  Object
1. 对象由 { } 分隔，在 { } 内部，对象的属性以名称和值对的形式 (name : value) 来定义，属性由逗号分隔　
```javascript
var cars={
"car1" : "Volvo",
"car2": "Saab",
}; 
//或者
var cars={
car1 : "Volvo",
car2: "Saab",
}; 
```
2. 寻找对象中的值有两种方式： 
```javascript
car1name=cars.car1;
car1name =cars["car1"];
```
3. 数组(Array)和函数(Function)是高级的对象
4. 注意⚠️：基本类型和引用类型的区别：
	* 基本类型：访问是按值访问值不可变，基本类型的比较是值的比较，数据是存放在栈内存中的
	* 引用类型：拥有属性和方法且值是可变的，引用类型的比较是引用的比较，数据是存放在堆内存中的

---
title: JavaScript数据类型转换
date: 2018-04-27 16:14:59
tags: [数据类型,JavaScript]
categories: 前端
---

任意数据类型转字符串、数字、布尔
<escape><!-- more --></escape>
### 1  任意转字符串
#### String(thing)   （thing：任何可以被转换成字符串的值
```javascript
String(1)    //"1"
String(true)    //"true"
String(null)    //"null"
String(undefined) //"undefined"
String({})     //"[object Object]"
```
注意⚠️：当字符串中的数字为其他进制时，会自动转化为十进制，再把十进制转化为字符串，如：
```javascript
String(0b1100)  //"12"   二进制转化为十进制
String(01100）  //"576"  八进制转化为十进制
String(0o1100)  //"576"  八进制转化为十进制
String(0x1100)  //"4352" 十六进制转化为十进制
```
#### thing.toString()
```javascript
1.toString()  //Uncaught SyntaxError: Invalid or unexpected token
1..toString()  //"1" 
(1).toString()  //"1" 
true.toString()  //"true"
null.toString()  //VM285:1 Uncaught TypeError: Cannot read property 'toString' of null at <anonymous>:1:6
undefined.toString()  //VM283:1 Uncaught TypeError: Cannot read property 'toString' of undefined at <anonymous>:1:11
{}.toString()  //"[object Object]"
```
#### thing + “”
```javascript
1 + ""   //"1"
true + ""  //"true"
null + ""  //"null"
undefined + ""  //"undefined"
{} + ""  //0
var o = {}  
o + ""  //"[object Object]"
```

### 2  任意转数字
#### Number(value)
```javascript
Number(true) //1     布尔转为数字
Number(false) //0     布尔转为数字
Number(null) //0     null转为数字
Number(undefined) //NaN   undefined转为数字，结果为NaN 
Number("123") //123   字符串转为数字
Number(" ") //0       有空格的空字符串为0
Number("") //0       为空格的空字符串为0
Number("123a")  //NaN   number转数字的字符串中不能有字母，parseFloat以及parseInt的中间可以有字母，但是开头不能有
Number("true") //NaN
Number("false") //NaN
```
#### parseInt(string, radix)  [MDN ](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt) 
```javascript
//⚠️string必须是一个字符串
//⚠️如果不是字符串而是一串数字（注意不能是数字+字母的格式），系统会自动把数字转为字符串，parseInt(01100)相当于是parseInt(String(01100))，由于01100是0开头，是一个八进制，String(01100)会把进制度转化为十进制再转为字符串，即相当于是parseInt("576")
parseInt("01100", 10) //1100      
parseInt(01100)  //576        
parseInt("01100", 8) //576  8的意思是：我字符串中的值是八进制的，请把它转化为十进制


//⚠️但是0x开头（即十六进制）的除外
parseInt("0b1100")  //0     二进制返回0，到字母b处即无法识别数字
parseInt("0o1100")  //0     八进制返回0，到字母o处即无法识别数字
parseInt("0x1100")  //4352  括号里的是字符串。十六进制返回对应的十进制
parseInt(0x1100)    //4352  括号里的不是字符串。十六进制返回对应的十进制
```
	* string：必需。要被解析的字符串。
	* radix：可选。表示要解析的数字的基数。该值介于 2 ~ 36 之间，如果省略该参数或其值为 0，则数字将以 10 为基础来解析。**如果它以 “0x” 或 “0X” 开头，将以 16 为基数**，如果该参数小于 2 或者大于 36，则 parseInt() 将返回 NaN。　　
	* 注意⚠️：radix参数为n 将会把第一个参数看作是一个数的n进制表示，而返回的值则是十进制的

#### parseFloat(string)  [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseFloat) 
```javascript
parseFloat("1.11")  //1.11       字符串中是数字（0-9）以及小数点
parseFloat(".1.11")  //0.1        如果首字符是.则会自动shi
parseFloat("0.0111e2")  //1.11       字符串中是科学计数法（e/E）
parseFloat("+1.11")  //1.11       字符串中有+
parseFloat("-1.11")  //-1.11      字符串中有-
parseFloat("1.11more")  //1.11       字符串中如果有除 小数点、+/-、数字、e/E  的字符，它以及之后的字符都会被忽略
parseFloat("more1.11")  //NaN       如果以字母开头直接NaN
parseFloat("   1.11")  //1.11       字符串开头的空格会自动忽略

//注意⚠️
parseFloat(".1.11")  //0.1        如果字符串有两个点，第二点之后的内容会被忽略掉
parseFloat("..1.11")  //NaN       开头多个点会NaN
```
	* string：需要被解析成浮点数的字符串　　　　
	* parseFloat是个全局函数,不属于任何对象　　
	* parseFloat将它的字符串参数解析成为浮点数并返回，如果在解析过程中遇到了**正负号(+或-)、数字(0-9)、小数点、或者科学记数法中的指数(e或E)以外的字符**，则它**会忽略该字符以及之后的所有字符**，返回当前已经解析到的浮点数，同时**参数字符串首位的空白符会被忽略** 　　
	* 可以通过调用isNaN函数来判断parseFloat的返回结果是否是NaN，如果让NaN作为了任意数学运算的操作数，则运算结果必定也是NaN　　
	* parseFloat 也可转换和返回Infinity值. 可以使用isFinite 函数来判断结果是否是一个有限的数值 (非Infinity, -Infinity, 或 NaN)　
	
#### string- 0 或 string*1 或string/1　
```javascript
"123"-0//123  相减
"123"*1//123  相乘
"123"/1//123  相除


//注意⚠️
"123a"-0//NaN  字符串中不能有字母　
```

#### +string 或 -string
```javascript
+"123"//123  正
-"123"//-123 负
```

### 3  任意转布尔
#### Boolean(value)   (value：可选，是用来初始化 Boolean 对象的值。)
```javascript
//Boolean()值为false的情况：参数值为 0、-0、null、NaN、undefined、空字符串（""），或者传入的参数为 DOM 对象的 document.all 时
Boolean(""); //false 
Boolean(0); //false
Boolean(-0); //false
Boolean(NaN); //false
Boolean(null); //false 
Boolean(undefined); //false 

//Boolean()值为True的情况：除以上提到的几种情况，任何其他的值，包括值为 "false" 的字符串和任何对象，都会创建一个值为 true 的 Boolean 对象。
//⚠️空字符串中如果有空格，返回的是true
Boolean(" "); //true
```
#### !!value
```javascript
!!""  //false
!!0  //false
!!-0 //false
!!NaN //false
!!null //false
!!undefined //false
```
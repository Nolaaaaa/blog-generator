---
title: TypeScript入门
date: 2019-05-24 23:25:29
tags: TypeScript
categories: 前端
---

学习[TypeScript入门教程](https://ts.xcatliu.com/)时的笔记和总结。
<escape><!-- more --></escape>

### 基本类型
**boolean**：`let temp: boolean = false`。注意：new Boolean(1)返回的是Boolean 对象，Boolean(1)返回的是boolean 类型
**number**：Ts中的16进制转化为Js的时候还是16进制，但是2、8进制会转化成10进制
**string**：可用模板字符串
**void**：空值，可以用 void 表示没有任何返回值的函数，声明一个 void 类型的变量没有什么用，因为你只能将它赋值为 undefined 和 null
**null 和 undefined**：与 void 的区别是，undefined 和 null 是所有类型的子类型，可以赋值给 number 类型的变量，而 void 不可以
**any**：任意值，允许被赋值为任意类型，对它的任何操作，返回的内容的类型也都是任意值。变量声明时，未指定其类型，且定义时未赋值，则默认为 any 类型。

**类型推论：**
如果定义时未指定其类型，且**赋值**，则会根据定义时所赋值的类型来推断其类型。
PS：如果定义时未指定其类型，且**未赋值**，则默认为 any 类型。

### 字符串字面量类型
用来约束取值只能是某几个字符串中的一个
1. 使用 `type` 进行定义，`type EventNames = 'click' | 'scroll' | 'mousemove'`

### 联合类型：
表示取值可以为多种类型中的一种。使用 | 分隔每个类型，如：`let temp: string | number`
1. 当联合类型的变量**没有被赋值**时，只能访问此联合类型的所有类型里共有的属性或方法。（当联合类型的变量**被赋值**时，会根据类型推论的规则推断出一个类型。）
2. 对于第1点，变量未被赋值时只能访问共有属性，解决办法是：使用时先 `类型断言` 某个变量为特定类型（联合类型中定义过的），如：`(<boolean>temp).length`（格式：`<类型>值`或`值 as 类型`（tsx 语法中））

### 枚举类型
用于取值被限定在一定范围内的场景
1. 使用 `enum` 进行定义，如：`enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat}`
2. 枚举成员会被赋值为从 `0` 开始递增的数字，同时也会对枚举值到枚举名进行反向映射，如：`Days["Sun"] === 0 // true   Days[0] === "Sun"  // true`
3. 手动**给部分枚举项赋值**时，未手动赋值的部分会自动编译，可能会有覆盖的情况
4. 手动赋值的枚举项**不是数字**时，需使用类型断言让 `tsc` 无视类型检查：`Sat = <any>"S"`
5. 手动赋值的枚举项**是小数或负数**时，此时后续未手动赋值的项的递增步长仍为 `1`
6. 枚举项有两种类型：**常数项**和**计算所得项**（后面若是未手动赋值的项，会因无法获得初始值而报错）
7. **常数枚举**：使用 `const enum` 定义，会在编译阶段被删除，并且**不能包含计算成员**
8. **外部枚举**：使用 `declare enum` 定义，`declare `定义的类型只会用于编译时的检查，编译结果中会被删除，常出现在声明文件中，也可同时使用 `declare` 和 `const`，如：`declare const enum Temp { ... }`
9. TypeScript 的枚举类型的概念来源于 `C#`

### 对象类型——接口
接口（Interfaces）用于定义对象的类型。
1. 赋值时，接口和变量的形状必须一致，1V1，多一个少一个都不行
2. 对于第1点：当一个属性被定义为可选属性，即可以不给这个属性赋值
3. 不允许添加未定义的属性
4. 对于第2点：当接口定义了任意属性，则可以添加未提前定义的属性
```ts
interface Person {
  readonly id: number;    // readonly 定义只读属性，初始化后不能再被赋值
  name: string;   // 必须要赋值
  age?: number;   // ?: 定义可选属性，可以不赋值
  [propName: string]: any;   // 定义任意属性
}
// Person 是 nola 的接口
let nola: Person = {
  id: 249399;
  name: 'Nola';
};
```

### 内置对象类型
1. ECMAScript 的内置对象：`Boolean`、`Error`、`Date`、`RegExp` 等
2. DOM 和 BOM 的内置对象：`Document`、`HTMLElement`、`Event`、`NodeList` 等
3. TypeScript 核心库的定义文件中定义了所有浏览器环境需要用到的类型，如使用方法`Math.pow(10, '2')`时会报错，因为`Math.pow` 必须接受两个 `number` 类型的参数
4. TypeScript 核心库的定义中不包含 `Node.js `部分
```TS
let b: Boolean = new Boolean(1);
let e: Error = new Error('Error occurred');
let d: Date = new Date();
let r: RegExp = /[a-z]/;
let body: HTMLElement = document.body;
let allDiv: NodeList = document.querySelectorAll('div');
document.addEventListener('click', function(e: MouseEvent) {});
```

### 数组类型
定义数组类型的几种方法：
1. 类型 + []：`let temp: number[] = [1, 2, 3]`，注意：数组中值的类型需和`[]`前的类型一致
2. `Array<number>`数组泛型：`let temp: Array<number> = [1, 2, 3]`，注意：数组中值的类型需和`<...>`中定义的类型一致
3. 使用接口描述数组：`interface NumberArray { [index: number]: number }`
PS：类数组不是数组，不能用数组的方式定义，可用类数组自己的接口定义，如 I`Arguments`、`NodeList`、`HTMLCollection`


### 元组类型
数组合并了相同类型的对象，而元组（Tuple）合并了不同类型的对象。
1. 定义：`let Nola: [string, number]`
2. 赋值：`Nola[0] = 'Nola'`或`Nola=['Nola',25]`
3. 使用`Nola.push('')`添加越界的元素时，它的类型会被限制为元组中每个类型的联合类型

### 函数类型
函数的类型定义：
```ts
// ts 函数声明的类型定义
function sum(x: number, y: number): number { return x + y }
// ts 函数表达式的类型定义
let mySum: (x: number, y: number) => number = function (x: number, y: number): number { return x + y }
// 使用接口定义函数需要的形状
interface Func { 
  (source: string, subString: string): boolean;
  interval: number;  // 定义函数自己的属性
  reset(): void;   // 定义函数自己的方法
}
let foo: Func
```
使用注意：
1. 使用时的参数不能 少于/多于 所定义的参数数量
2. 对于第1点：可使用 `?:` 定义可选参数，但选参数后面不允许再出现必须参数
3. TypeScript 中的 `=>`：`(输入类型) => 输出类型`
4. 参数默认值：`name: string = 'Nola'`，ts默认该参数为可选参数，但是后面也可出现必须参数
5. 剩余参数 `...rest` 是一个数组，可用 `...items: any[]` 方式定义。注意; `...rest` 参数只能是最后一个参数
6. 重载：允许一个函数接受不同数量或类型的参数时，作出不同的处理

### 类
类的用法具体参考：[TypeScript入门教程 - 类](https://ts.xcatliu.com/advanced/class)和[ECMAScript 6 入门 - Class](https://ts.xcatliu.com/advanced/class)
1. (ES6)类的继承：使用 `extends` 实现继承，如：`class A extends B`，子类中使用 `super` 调用父类的构造函数和方法
2. (ES6)静态方法：使用 `static` 修饰，如：`static tamp(a)`，它们**不需要实例化**，而是**直接通过类来调用**
3. (ES7)静态属性：使用 `static` 修饰，如：`static age = 24`
4. (TS)访问修饰符：`public`（公有的，值可以被外部获取和修改，默认）、`private`（私有的，无法直接存取，子类中也不允许被访问） 和 `protected`（和 private 类似，区别是它在子类中允许被访问）
5. (TS)抽象类：使用 `abstract` 定义，如：`abstract class A `，不允许被实例化，可以被继承，但抽象类中的抽象方法必须被子类实现，编译结果中，会存在这个类

### 类与接口
接口的另一个用途，对类的一部分行为进行抽象
1. 类的特性提取成接口：使用 `implements` 实现，如：`class A implements C`（C使用 `interface C` 定义），多个接口之间可使用`,`隔开
2. 接口继承接口：`interface C extends D `（D使用 `interface D` 定义），接口也可以继承类

### 泛型
在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性
```ts
// T、U 用来指代任意输入的类型
function foo<T>(value: T): Array<T> {...} 
function foo<T, U>(value: [T, U]): [U, T] {...}   
// 泛型约束，extends 约束了泛型 T 必须符合对应的形状
function foo<T extends Temp>(value: T): T {...}    // Temp为自定义的一个接口
function foo<T extends U, U>(value: T, source: U): T    // 多个类型参数之间也可以互相约束
```
1. 泛型中的类型参数指定默认类型，如：`<T = string>`
2. 泛型可用于函数、类的类型定义中，如：`interface Temp<T> {}` `class Temp<T> {}`

### 声明合并
如果定义了两个相同名字的函数、接口或类，那么它们会合并成一个类型
1. 函数的合并：使用**重载**定义多个函数类型
2. 接口合并：两个相同名字的属性会合并到一个接口中，但是合并的属性的类型必须是唯一的（如两个属性相同，类型相同不会报错，不同时会报错）
3. 类的合并与接口的合并规则一致

### 类型别名
类型别名常用于联合类型
1. 使用 `type` 创建类型别名

### 声明文件
内容太多了，简单总结一下，具体该教程[地址](https://ts.xcatliu.com/basics/declaration-files)
第三方声明文件使用 @types 统一管理第三方库的声明文件，如：`npm install @types/name --save-dev`
#### 全局变量的声明文件
通过 `<script>` 标签引入第三方库
1. 声明文件必需以 `.d.ts` 为后缀
2. 声明语句只能定义类型，不能在声明语句中定义具体的实现，`declare let Name: (selector: string) => any;`
3. 全局变量的声明文件的几种语法：`declare var` 声明全局变量，`declare function` 声明全局方法，`declare class` 声明全局类，`declare enum` 声明全局枚举类型，`declare namespace` 声明（含有子属性的）全局对象，`interface` 和 `type` 声明全局类型
4. 为防止命名冲突，应尽可能的减少全局变量或全局类型的数量，最好将他们放到 `declare namespace` 下

#### npm 包的声明文件
通过 `import foo from 'foo'` 导入
2. 创建声明文件前先看看它的声明文件是否已经存在
3. npm 包声明文件存在的地方：第一：与该 `npm` 包绑定在一起。判断依据是 `package.json` 中有 `types` 字段，或者有一个 `index.d.ts` 声明文件；第二：`@types` 里，尝试安装一下对应的 `@types` 包就知道是否存在该声明文件
4. 创建 `npm` 包声明文件的方法：第一种：创建一个 `node_modules/@types/foo/index.d.ts` 文件，存放 `foo` 模块的声明文件（有风险，如无法版本回溯或可能被删掉，不推荐）；第二种：创建一个 types 目录，专门用来管理自己写的声明文件，将 `foo` 的声明文件放到 `types/foo/index.d.ts` 中。这种方式需要配置下 `tsconfig.json` 中的 `paths` 和 `baseUrl` 字段
5. npm 包的声明文件的几种语法：`export` 导出变量（如：`export const/function/class/enum/interface`，`export namespace` 导出（含有子属性的）对象，也可以用`declare`声明多个变量（`interface` 不用加 `declare`），再用`export`导出 `export { name、age }`）、`export default` ES6 默认导出（导入时用`import foo from 'foo'` 而不是 `import { foo } from 'foo'`，只有 `function`、`class` 和 `interface` 可以直接默认导出，其他的变量需要先定义出来，再默认导出，如：`export default enum` 是错误的语法，需要使用 `declare enum` 先定义）、commonjs 规范的导出 `export = xxx`（导入方法：`const ... = require('...')`/`import ... from`/`import ... =  require('...')`）

#### UMD 库的声明文件
可以通过 `<script>` 标签引入，又可以通过 `import` 导入
1. 基于 npm 包的类型声明文件，需要额外声明一个全局变量，语法为 `export as namespace`
2. `export as namespace`可与`export default`一起使用

#### 改变全局变量的类型
直接扩展全局变量：通过 `<script>` 标签引入后，改变一个全局变量的结构，通过声明合并（如扩展`String`可使用`interface String`）、使用 `declare namespace` 给已有的命名空间添加类型声明
在 npm 包或 UMD 库中扩展全局变量：引用 `npm` 包或 `UMD` 库后，改变一个全局变量的结构，语法为：`declare global`
模块插件：通过 `<script>` 或 `import` 导入后，改变另一个模块的结构，语法为：`declare module`

#### 自动生成声明文件
如果库的源码本身就是由 ts 写的，那么在使用 tsc 脚本将 ts 编译为 js 的时候，添加 declaration 选项，就可以同时也生成 .d.ts 声明文件
1. 在命令行中添加 `--declaration`（简写 -d）
2. `tsconfig.json` 中添加 `declaration` 选项，`"declaration": true`

#### 发布声明文件
1. 将声明文件和源码放在一起
2. 将声明文件发布到 `@types` 下


## 一、简述 TypeScript 的特点及其主要意义

### 1. TypeScript 特点
* 作为 JavaScript 的超集，体现在：为 JavaScript 引入一套类型系统和支持一些非 ECMAScirpt 正式标准的语法，如装饰器；
* 社区活跃，绝大多数第三方库都提供了 TypeScript 的类型定义文件；
* 兼容性较好。直接把 `.js` 文件改成 `.ts` 文件即可，即使没有显示定义类型，也可以自动做类型推导；

### 2. TypeScript 主要意义
* 增加代码可读性和可维护性

TypeScript 的类型系统实际上是很好的文档，大多数函数看看类型定义就能知道如何使用。并能在编译阶段就能发现大部分错误，免得在运行阶段才发现。

* 拥抱ES6，也支持部分 ESNext 规范

支持一些非 ECMAScirpt 正式标准的语法，如装饰器。

### 3. TypeScript 缺点

* 有一定学习成本，需要理解接口（Interfaces）、泛型（Generics）、类（Classes）、枚举类型（Enums）等前端工程师可能不是很熟悉的概念；
* 短期可能会增加一些开发成本，毕竟要多写一些类型的定义，不过对于一个需要长期维护的项目，TypeScript 能够减少其维护成本；
* 集成到构建流程需要一些工作量；
* 可能和一些库结合的不是很完美；

## 二、分别列出 JS 自带类型与 TS 额外定义的类型，并简要说明

### 1. JavaScript 自带类型
最新的 ECMAScript 标准定义了 8 种数据类型:
* `Boolean`：布尔表示一个逻辑实体，可以有两个值：`true` 和 `false`；
* `Null`：只有一个值 `null`；
* `Undefined`：一个没有被赋值的变量会有个默认值 `undefined`；
* `Number`：数字类型；
* `BigInt`：用任意精度表示整数；
* `String`：用于表示文本数据；
* `Symbol`：符号类型是唯一的并且是不可修改的；
* `Object`：是一组属性的集合；

### 2. TypeScript 额外类型

* `Tuple`：允许表示一个已知元素数量和类型的数组，各元素的类型不必相同；
* `Enum`：是组织收集有关联变量的一种方式；
* `Any`：为变量定义任意类型，让类型检查器不做检查，直接通过编译阶段的检查；
* `Void`：与 `any` 类型相反，它表示没有任何类型；
* `Never`：表示那些永不存在的值的类型。

## 三、简述数组（Array）类型与元组（Tuple）类型的异同

### 1. 相同点

数组和元组，都可以用来存储元素集合。

### 2. 异同点

* 存储数据的类型不同

**数组**指定类型后，**无法确切定义每个位置上元素的类型**。
而**元组**则可以**规定每个位置元素的数据类型**，在元组范围外的元素，则是已定义位置数据类型的混合类型。

```js
// 数组
let arr: Array<string | number> = ["a", 2];  // ok
let arr: Array<string | number> = [2, "a"];  // ok

// 元组
let tuple: [number, string] = [1, "hello"];  // ok
let tuple: [number, string] = ["hello", 1];  // error
```

* 越界问题

**数组**不用担心数组越界后元素类型问题。
而**元组**则会以已知元组定义的元素类型的混合类型，一般不建议超出元组范围，超出范围就**无法保证其具体类型**。

### 3. 小结

**数组**一般用于保存**相同类型元素集合**，**元祖**可以存储**不同类型元素集合**，两者都可以作为函数参数。


## 四、类型名称首字母大写和小写有什么区别？（例如 string 与 String ）

在 JavaScript 和 TypeScript 中，首字母大写和小写的类型名称，都是不相同的。

如 `String` 和 `string`：

* `String` 是指 `String.prototype`，指其原型链中具有的对象实例，可以用来创建新字符串。

我们可以通过各种方式获得这样的实例，如：

```js
let str1: String = new String('foo'); // String {"foo"}
let str2: String = Object('foo');     // String {"foo"}

str1 instanceof String; // true
str2 instanceof String; // true
```

* `string` 是 JS 和 TS 的基本数据类型之一，表示值为 `string` 类型，如 `foo` 和 `bar`。
```js
let str: string = "foo";  // "foo"
typeof str; // "string"
```

类似的，还有 `Number` 与 `number` 等等。

## 五、在声明变量时，有几种方式可以指定变量的类型？（允许在声明变量时进行赋值）

1. 直接声明变量

```js
let name: string = "leo";
let age:  number = 18;
let test: string | number = "test";

interface Leo {
    name: string,
    age?: number
}
let leo: Leo = {
    name: 'leo',
    age: 18
}
```

2. 声明数组

```js
// 第一种，在元素类型后接上 []，表示由此类型元素组成的一个数组
let list: number[] = [1, 2, 3];

// 第二种，使用数组泛型，Array<元素类型>
let list: Array<number> = [1, 2, 3];
```

3. 声明元组

```js
let x: [string, number] = ['hello', 10];
```

4. 声明枚举

```js
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
```

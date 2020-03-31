1️⃣、请列举在 TypeScript 中声明函数类型的方式。
2️⃣、TypeScript 函数的可选参数是什么，在声明时有何要求？在实际的 Javascript 代码中，可选参数是如何实现的？
3️⃣、请简要介绍 Javascript 函数的 this ，并使用简单的代码进行举例。在 TypeScript 代码中，函数的 this 又是如何声明与使用的？
4️⃣、请简要介绍“函数签名”。
5️⃣、请简要介绍“函数重载”。 Javascript 是否能实现真正的函数重载？ TypeScript 中的函数重载是如何实现的？请使用简单的例子进行说明。


# 一、列举 TypeScript 中函数声明的方式

1. 函数声明

```ts
function sum(a: number, b: number): number {
    return a + b;
};
```

2. 函数表达式

```ts
const sum = (a: number, b: number): number => a + b;
const sum : (a: number, b: number) => number = (a: number, b: number): number => a + b;
```

3. 接口定义

```ts
interface Sum {
    (a: number, b: number): number
}

const sum: Sum = (a: number, b: number): number => a + b;
```

# 二、可选参数介绍，JavaScript如何实现可选参数

**可选参数**：即在函数调用过程中，不是必须要传入的参数。在 TypeScript 中，我们在参数名旁使用 `?` 符号将参数设置为可选参数，比如：

```ts
const sum = (a: number, b: number, total?: number): number => total || a + b;
```

JavaScript实现可选参数有：

* ES5 及之前

```js
function user(userId){
    var userList = ['leo', 'leo1', 'leo2'];
    if(!userId){
        userId = 0;
    }
    return userList[userId];
}

function user(userId){
    var userList = ['leo', 'leo1', 'leo2'];
    userId = userId || 0;
    return userList[userId];
}
```

* ES6 及之后

```js
function user(iuserId = 0) { 
    const userList = ['leo', 'leo1', 'leo2'];
    return userList[userId];
}
```

# 三、简要结合代码介绍 JavaScript 和 TypeScript 中的 this

## 1. JavaScript 中的 this

1. 在对象方法中， this 指向调用它所在方法的对象。

```js
const leo = {
    name : "leo",
    say  : function (){
        console.log('hello ' + this.name);
    }
}
leo.say(); // "hello leo"
```

1. 单独使用 this，它指向全局(Global)对象。

```js
console.log(this); // Window object
```

1. 函数使用中，this 指向函数的所属者。

```js
var leo = "leo";
function callUser(){
    console.log('hello ' + this.leo);
}
callUser(); // "hello leo"
```

1. apply 和 call 允许切换函数执行的上下文环境（context），即 this 绑定的对象，可以将 this 引用到任何对象。

```js
var leo = "leo";
function callUser(){
    console.log('hello ' + this.leo);
}
callUser(); // "hello leo"

const pingan = {
    leo: "good boy"
}
callUser.apply(pingan); // "hello good boy"
```

## 2. TypeScript 中的 this

TypeScript 中的 this 作为伪参数，放在参数第一位，编译成 ES5 之后不会产生实际参数：

```ts
function leo(this: void) {
 // this: void：表示在函数体内不允许使⽤this
}

// 转成 ES5 之后
function leo() {
 // this: void：表示在函数体内不允许使⽤this
}
```

作用有三个：

* 明确 this 参数类型

```ts
class Rectangle {
    private w: number;
    private h: number;
    constructor(w: number, h: number) {
        this.w = w;
        this.h = h;
    }
    getArea(this: Rectangle) {
        return () => {
            return this.w * this.h;
        };
    }
}
```

* 禁止使用 this

```ts
getArea(this: void) {
    return () => {
        return this.w * this.h;
    };
}
// Property 'w' does not exist on type 'void'.
// Property 'h' does not exist on type 'void'.
```

* 回调函数中 this

```ts
const button = document.querySelector("button");
button?.addEventListener("click", handleClick);
function handleClick(this: HTMLElement) {
    console.log("Clicked!");
    this.removeEventListener("click", handleClick);
}
```

# 四、请简要介绍“函数签名”

函数签名（或者类型签名，抑或方法签名）定义了函数或方法的输入与输出。

签名可包含以下内容：

* 参数及参数的类型
* 一个的返回值及其类型
* 可能会抛出或传回的异常
* 该方法在面向对象程序中的可用性方面的信息（如public、static 或 prototype）。

举个例子：

```js
MyObject.prototype.myFunction(value)
```
* 该方法是安装在一个名为 MyObject 的 对象上。
* 该方法安装在 MyObject 的原型上（因此它是一个实例方法），而不是static method。
* 该方法名为 myFunction。
* 该方法接受一个参数，该参数被称为 value，且没有进一步定义。

参考文章[《Signature (functions)（函数签名）》](https://developer.mozilla.org/zh-CN/docs/Glossary/Signature/Function)。


# 五、介绍介绍“函数重载”，JS 是否能实现真正的函数重载？举例说明 TS 中如何实现。

## 1. 函数重载

函数重载或方法重载是使用相同名称和不同参数数量或类型创建多个方法的一种能力。调用时根据实参的形式，选择与它匹配的方法执行操作。
为同一个函数提供多个函数类型定义来进行函数重载，编译器会根据这个列表去处理函数的调用。

具体如下：
* 函数名相同
* 函数的参数类型，参数个不一样
* 函数的返回类型可以不相同
* 函数的修饰符可以不相同
* 构造函数也可以被重载

## 2. 

## 3. TypeScript 实现函数重载
在定义重载的时候，一定要把最精确的定义放在最前面，因为 TypeScript编译器处理函数重载时，会查找重载列表，尝试使用第一个重载定义。

```ts
type Common = string | number;

function add(a: number, b: number): number;
function add(a: string, b: string): string;
function add(a: string, b: number): string;
function add(a: number, b: string): string;
function add(a: Common, b: Common) {
  if (typeof a === 'string' || typeof b === 'string') {
    return a.toString() + b.toString();
  }
  return a + b;
}
```

### 3.1 类中函数重载

在类中成员方法满足重载的条件是：在同一个类中，方法名相同且参数列表不同。

```ts
type Common = string | number;

class CommonObj {
  add(a: number, b: number): number;
  add(a: string, b: string): string;
  add(a: string, b: number): string;
  add(a: number, b: string): string;
  add(a: Common, b: Common) {
  if (typeof a === 'string' || typeof b === 'string') {
    return a.toString() + b.toString();
  }
    return a + b;
  }
}

const commonObj = new CommonObj();
const result = commonObj.add('leo', ' leo1');
```

### 3.2 构造函数重载

```ts
interface Shape {
  x: number;
  y: number;
  height: number;
  width: number;
}

class Square {
  public x: number;
  public y: number;
  public height: number;
  public width: number;

  constructor();
  constructor(obj: Shape);
  constructor(obj?: any) {
    this.x = obj?.x ?? 0;
    this.y = obj?.y ?? 0;
    this.height = obj?.height ?? 0;
    this.width = obj?.width ?? 0;
  }
}
```
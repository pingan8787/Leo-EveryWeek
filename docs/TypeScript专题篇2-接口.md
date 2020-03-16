## 一、简要介绍 Interface，并说明它与 JS 的 Object 关联和区别。

接口是对行为的抽象，用来定义对象的类型。

Interface 和 Object 区别：
* Interface 声明时一般首字母大写，而 Object 没有规定。
* Interface 用来存储属性及其属性类型，而 Object 则存储属性及其实际值。

## 二、简要介绍接口的可选属性和只读属性，并分别说明只读属性编写时与使用时的特点。

### 2.1 可选属性
定义接口中非必需的属性，使用方式如：
```ts
interface User {
    name: string;
    age?: number;
}
let Leo: User = { name: 'Leo' };
```

特点：可以对可能存在的属性进行预定已，还可以捕获引用了不存在的属性时的错误。

### 2.2 只读属性

定义接口中只能在对象刚创建时修改的值，使用方式如：
```ts
interface User {
    name: string;
    readonly age: number;
}
let Leo: User = { name: 'Leo', age: 18 };
Leo.age = 10000;
// Cannot assign to 'age' because it is a constant or a read-only property.
```


`readonly` 和 `const` 区别：使用时看要把它做为变量使用还是做为一个属性。 做为变量使用的话用 `const`，若做为属性则使用`readonly`。

## 三、下面代码有何问题，尽可能详细。

```ts
interface Rectangle {
  width: number;
  height: number;
}
function area(rect: Rectangle) {
  return rect.width * rect.height;
}
area({ width: 15, height: 20, type: 'square' });
```


### 3.1 问题解析

本段代码的问题在于，将参数对象字面量直接作为 `area` 方法的参数。这样会导致控制台提示如下错误信息：

```ts
/*
Argument of type '{ width: number; height: number; type: string; }' is not assignable to parameter of type 'Rectangle'.
  Object literal may only specify known properties, and 'type' does not exist in type 'Rectangle'.
*/

// 大致意思：
/*
类型“{ width: number; height: number; type: string; }”的参数不能赋给类型“Rectangle”的参数。
对象文字可以只指定已知属性，并且“type”不在类型“Rectangle”中。
*/
```

这个问题可以查看官网的介绍[《excess-property-checks》](https://www.typescriptlang.org/docs/handbook/interfaces.html#excess-property-checks)。

出现这个问题的原因是：

本段代码中，对象字面量在编译阶段就已经确定类型`'{ width: number; height: number; type: string; }'` ，而方法 `area` 的参数类型只有 `'{ width: number; height: number; }'`，并没有包含 `type` ，所以会提示错误。

需要记住的是：当使用字面量属性作为参数时，TypeScript 会对字面量属性进行检查，检查是否传入多余的属性，若存在的话，就会提示错误信息。

### 3.2 代码优化

避免上面出现上面的问题，我们有两种方式避免：

1. 使用变量进行作为参数；

当使用变量作为参数时，不会检查是否传入多余的属性。

```ts
const params = { width: 15, height: 20, type: 'square' }
area(params);
```

2. 使用类型断言指定类型；

类型断言（Type Assertion）可以用来手动指定一个值的类型。详细可阅读[《类型断言》](https://ts.xcatliu.com/basics/type-assertion)。

```ts
area({ width: 15, height: 20, type: 'square' } as Rectangle);
```

## 四、补齐 MappedType 接口声明，并说明使用 TS 接口的哪个特性，及其适用范围。

```ts
interface MappedType {
  // 请补齐此处的类型声明
}
function customMapping(values: string[]): MappedType {
  const result: MappedType = {};
  values.forEach(value => result[value] = parseFloat(value) || 0);
  return result;
}
```

补充结果：
```ts
interface MappedType {
  [x: string]: number;
}
```

这里使用到的特性是**TypeScript**的索引签名。
目前 TypeScript 支持两种索引签名，包括**数值索引**和**字符串索引**。

使用方式如下：

```ts
interface StringArray {
    [index: string]: string; // 字符串索引 -> keyof StringArray => string | number
}

interface StringArray1 {
    [index: number]: string; // 数值索引 -> keyof StringArray1 => number
}
```
为了同时支持两种索引类型，就得要求**数值索引的返回值必须是字符串索引返回值的子类**。
其中的原因就是当使用数值索引时，JavaScript 在执行索引操作时，会**先把数值索引先转换为字符串索引**。

```ts
let leo = { 100: 'hello', 200: 'world'};
leo[100] === leo['100'];  // true
```

在计算 `leo[100]` 索引时，会先执行 `100->'100'` ，然后计算索引 `leo['100']` 索引。

## 五、分别介绍接口的实现（ implements ）与派生（ extends ），并举例说明。

### 5.1 实现 implements

实现（implements）是面向对象中一个重要概念，通常将通用特性提取成接口（interface），然后通过 implements 关键字实现。
这个特性大大提高了面向对象的灵活性。

```ts
interface Phone {
    call(): void;
}

interface Music {
    play(): void;
    pause(): void;
}

class Iphone implements Phone, Music {
    call() {
        console.log('Call up');
    }
    play() {
        console.log('Play music');
    }
    pause() {
        console.log('Pause music');
    }
}
```

### 5.2 派生 extends 
接口和类一样，可以互相继承，将一个接口里的成员复制到另一个接口中，可以使接口更灵活。

```ts
interface Shape {
    color: string;
}

interface Square extends Shape {
    sideLength: number;
}
```
1️⃣、请简述类（ Class ）中 this 与 super 的作用。
2️⃣、什么是类属性/方法的可访问性？有几种类型，作用分别是什么？
3️⃣、什么是类/对象的访问器属性（ Accessors ）？请用简单的例子进行说明。
4️⃣、请简述抽象类的特点与意义。
5️⃣、JavaScript/TypeScript 中的类，实际上是使用对象的什么特性实现的？请尝试将以下的 TypeScript 代码改写为等效的 ES5 代码。

# 题目一

**题目：类（Class）中 this 与 super 的作用；**

## 1. this

`this` 关键字表示当前类实例化的对象。注意构造函数的参数名与字段名相同，`this.name` 表示类的字段。

```ts
class Leo {
    name: string;
    constructor(name: string){
        this.name = name;
    }
}
```

编译为 ES5 代码：

```js
var Leo = /** @class */ (function () {
    function Leo(name) {
        this.name = name;
    }
    return Leo;
}());
```

## 2. super

`super` 方法用来执行基类的构造函数，并且需要在构造函数访问 `this` 的属性之前调用！

```ts
class Animal {
    name: string;
    constructor(theName: string) { this.name = theName; }
    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}

class Snake extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 5) {
        console.log("Slithering...");
        super.move(distanceInMeters); // 相当于调用 Animal.move
    }
}
let sam = new Snake("Sammy the Python");
sam.move();
// Slithering...
// Sammy the Python moved 5m.
```

# 题目二

**题目：什么是类属性/方法的可访问性？有几种类型，作用分别是什么？**

通常情况下，我们需要将类的指定属性或方法进行封装，实现信息隐藏，在 TypeScript 中我们通过类的可访问性来实现。

类的可访问性是指**当前类可被其他类所访问的程度**，通过不同的可访问性修饰符可以体现程序的封装性。

是类属性/方法有公共（`public`），私有（`private`）或保护（`protected`）可访问性。默认为`public`.。

## 2.1 public

默认值，公共属性/方法成员随处**可访问而无任何限制**。

## 2.2 private

私有属性/方法成员**只能被当前类中的元素访问**。例如一个声明在类“C”中的私有成员“M”只能被“C”类体中的元素访问。

## 2.3 protected

保护共属性/方法成员**只能被当前类中的元素和派生于它的类访问**，并且一个保护实例属性成员必须通过一个类的实例或类的子类来访问。例如，一个声明在类“C”中的保护成员“M”只能在“C”的类体中或派生于“C”的类访问。


# 题目三

**题目：什么是类/对象的访问器属性（Accessors）？请用简单的例子进行说明。**

参考文章：[《TypeScript属性访问器》](http://www.srcmini.com/3443.html)。

在TypeScript中，属性访问器提供了访问和设置类成员的方法。它有两个方法，如下所示。

* `getter` 
* `setter` 

## 3.1 getter

`getter` 访问器属性是用于检索变量值的常规方法。在 `object` 字面量中，用“`get`”关键字表示的 `getter` 属性。它可以是公共的、私有的和受保护的。

```ts
class Leo {    
    _name: string = "leo";
    get name():string {    
        return this._name;    
    }
    set name (str: string){
        this._name = 'hello ' + this._name;
    }
}    
let MyLeo = new Leo();
console.log(MyLeo.name);    // leo
```

## 3.2 setter

`setter` 访问器属性是用于更新变量值的常规方法。在 `object` 字面量中，`setter` 属性由“`set`”关键字表示。

```ts
class Leo {    
    _name: string = "leo";
    get name():string {    
        return this._name;    
    }
    set name (str: string){
        this._name = 'hello ' + this._name;
    }
}    
let MyLeo = new Leo();
MyLeo.name = 'pingan';
console.log(MyLeo.name);    // hello leo
```

# 题目四

**题目：简述抽象类的特点与意义**

参考文章：[《Typescript中的抽象类和抽象方法》](https://www.jianshu.com/p/cfce25e6e4a4)。

## 4.1 什么是抽象类

* 使用 `abstract` 关键字定义抽象类和抽象方法；
* 抽象类不允许被实例化；
* 抽象类中的抽象方法必须被实现。

## 4.2 定义抽象类和抽象方法

```ts
// 抽象类
abstract class Person{
  // 抽象方法
  public abstract eat();
}
```

实例化抽象类会怎么样？试试

```ts
let person = new Person();
// 报错: Cannot create an instance of the abstract class 'Person'.
```

## 4.3 实现抽象类中的抽象方法

```ts
abstract class Person{
  // 抽象方法
  public abstract eat();
}
// 我们这里先步实现抽象类中的eat抽象方法
class Student extends Person{
  public study(){
    console.log('study.....');
  }
}
let student = new Student();
// 报错: Non-abstract class 'Student' does not implement inherited abstract member 'eat' from class 'Person'.
```

通过上面报错可知：**抽象类中的抽象方法必须被实现**。

# 题目五

**题目：JavaScript/TypeScript 中的类，实际上是使用对象的什么特性实现的？请尝试将以下的 TypeScript 代码改写为等效的 ES5 代码。**

```ts
class Rectangle {
    constructor(public width: number, public height: number) {

    }

    static formatResult(result: number): number {
        return Math.round(result * 100) / 100;
    }

    area(): number {
        return Rectangle.formatResult(this.width * this.height);
    }
}

class Square extends Rectangle {
    constructor(public sideLength: number) {
        super(sideLength, sideLength);
    }

    area(): number {
        if (this.width !== this.height) {
            return NaN;
        }

        return super.area();
    }
}
```

## 5.1 JavaScript/TypeScript类的实现

在 JavaScript 的 ES6 规范中提供了两个关键字 `class` 和 `extends` ，虽然它们只是语法糖，底层还是再利用 `prototype` 实现继承的，但是不能否认，这中写法确实让代码更清晰，更易读。

TypeScript 也是使用  `prototype` 实现继承。

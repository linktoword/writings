# TypeScript学习笔记

TypeScript：JavaScript的超集。

## 类型注解

一种轻量级的为函数或者变量添加约束的方式；

## 基础类型

ts支持和js几乎相同的数据类型，此外还提供了实用的枚举

### 布尔值`: boolean`

### 数字`: number`

### 字符串 `: string`

### 数组

- 直接在元素类型后面接上`[]`,表示由此类型元素组成的一个数组`let list: number[] = [1,2,3]; `
- 使用数组泛型， `Array<元素类型>`; `let list: Array<number> = [1,2,3];`

### 元组 Tuple

表示一个已知元素数量和类型的数组，各元素的类型不必相同。

```typescript
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error
```

当访问数组一个越界的元素，会使用联合类型代替

### 枚举enum

enum类型是对JavaScript标准数据类型的一个补充。

```
enum Color {Red, Green, Blue}
let c: Color = Color.Green; // 1
```

默认情况下枚举从0开始编号，但是你可以手动指定成员值

```
enum Color {Red = 1, Green = 2, Blue = 4}
let c: Color = Color.Green; // 2
```

枚举类型提供的一个便利就是你可以由枚举的值得到它的名字

### Any

### Void

表示没有任何类型， 只能为此类型赋值null和undefined

### null和undefined

TypeScript里，`undefined`和`null`两者各自有自己的类型分别叫做`undefined`和`null`。

默认情况下`null`和`undefined`是所有类型的子类型。 就是说你可以把 `null`和`undefined`赋值给`number`类型的变量。

然而，当你指定了`--strictNullChecks`标记，`null`和`undefined`只能赋值给`void`和它们各自。

### Never

`never`类型表示的是那些永不存在的值的类型。 例如， `never`类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 `never`类型，当它们被永不为真的类型保护所约束时。

`never`类型是任何类型的子类型，也可以赋值给任何类型；然而，*没有*类型是`never`的子类型或可以赋值给`never`类型（除了`never`本身之外）。 即使 `any`也不可以赋值给`never`。

### Object

`object`表示非原始类型，也就是除`number`，`string`，`boolean`，`symbol`，`null`或`undefined`之外的类型。

### 类型断言

通过*类型断言*这种方式可以告诉编译器，“相信我，我知道自己在干什么”。 类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript会假设你，程序员，已经进行了必须的检查。

## 接口

定义变量应该拥有什么东西， 是一些规范

```typescript
interface Demo {
	laber: string
}
```

#### 接口的属性

##### 可选属性

接口里的属性不全都是必需的。 有些是只在某些条件下存在，或者根本不存在。 

带有可选属性的接口与普通的接口定义差不多，只是在可选属性名字定义的后面加一个`?`符号

```typescript
interface SquareConfig {
  	color?: string;
  	width?: number;
}
```

##### 只读属性

些对象属性只能在对象刚刚创建的时候修改其值。 你可以在属性名前用 `readonly`来指定只读属性:

```typescript
interface Point {
    readonly x: number;
    readonly y: number;
}
```

TypeScript具有`ReadonlyArray<T>`类型，它与`Array<T>`相似，只是把所有可变方法去掉了

最简单判断该用`readonly`还是`const`的方法是看要把它做为变量使用还是做为一个属性。 做为变量使用的话用 `const`，若做为属性则使用`readonly`。

## 泛型

```
function identity<T>(arg: T): T {
    return arg;
}
```

我们给identity添加了类型变量`T`。 `T`帮助我们捕获用户传入的类型（比如：`number`），之后我们就可以使用这个类型。 之后我们再次使用了 `T`当做返回值类型。现在我们可以知道参数类型与返回值类型是相同的了。 这允许我们跟踪函数里使用的类型的信息。

使用`any`类型会导致这个函数可以接收任何类型的`arg`参数，这样就丢失了一些信息：传入的类型与返回的类型应该是相同的。
## hello world

首先新建一个 `app` 文件夹，用作我们的项目目录，然后新建一个 `app.ts` 文件

```js
// app.ts
class Hello {
	public static main(): number {
		console.log(`hello world`)
		return 0
	}

	hello () {
		console.log(`hello`)
	}
}

Hello.main()

new Hello().hello()
```

然后在命令行中运行

```js
tsc app.ts
node app
```

就可以看到 可以看到 `hello world` 和 `hello` 的输出，这就是一个最简单的 `TypeScript` 程序，一些相关知识点

* `TypeScript` 文件的后缀名为 `.ts`
* `public` 表名这个方法是公共方法，同样的可以用 `private` 表示私有方法，`protected` 在子类中可以使用
* 用 `static` 声明这个方法是静态的，可以在通过 `Hello.main()` 直接使用，反则要通过实例化一个对象然后再来调用
* 反回类型通过 `: number` 表示反回结果为数字类型
* `tsc` 命令可以把 `ts` 文件编译成 `JavaScript` 文件，实际上在项目中使用的还是 `JavaScript` 类型的文件






----

----









## Boolean

```js
let isDone: boolean = false
```

## Number

除了支持十进制和十六进制字面量，`TypeScript` 还支持 `ECMAScript 2015` 中引入的二进制和八进制字面量

```js
let height: number = 6
let hexLiteral: number = 0xf00d
let binaryLiteral: number = 0b1010
let octalLiteral: number = 0o744
```

## String

```js
var name: string = 'bob' // 可以使用双引号
name = 'smith'           // 或者使用单引号
```

还可以使用模版字符串，同 `ES6` 中的使用方式是一致的

```js
let name: string = `zhangsan`
let age: number = 18
let sentence: string = `Hello, my name is ${ name }, ${ age } years old.`
```


## Array

```js
var list:number[] = [1, 2, 3]      // 第一种声明方式
var list:Array<number> = [1, 2, 3] // 第二种声明方式
```

## Tuple

元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同，比如可以定义一对值分别为 `string` 和 `number` 类型的元组

```js
let x: [string, number]

// 这样是可以的
x = ['hello', 10] 

// 这样会报错，类型不符合
x = [10, 'hello']
```

当访问一个已知索引的元素，会得到正确的类型

```js
console.log(x[0].substr(1)) // OK

console.log(x[1].substr(1)) // number 没有 substr() 方法
```

当访问一个越界的元素，会使用联合类型替代：

```js
x[3] = 'world' // OK, 字符串可以赋值给（string | number）类型

x[5].toString() // OK, 'string' 和 'number' 都有 toString 方法

x[6] = true // Error, 布尔不是（string | number）类型
```

## Enum

```js
enum Color {
  Red, Green, Blue   // 默认 enum 是从 0 开始
}  

var c: Color = Color.Green
```

但是我们可以改变起始值，让其从 `1` 开始

```js
enum Color {
  Red = 1, Green, Blue // 起始值从 1 开始
} 

var c: Color = Color.Green
```

或者全部都采用手动赋值

```js
enum Color {
  Red = 1, Green = 2, Blue = 4
} 

var c: Color = Color.Green
```

也可以反向推断出这个值对应的 `enum` 名称

```js
enum Color {
  Red = 1, Green, Blue
}
var colorName: string = Color[2] 

alert(colorName)
```


## Any

当我们不确定声明值它对应的类型，想要由程序运行时确定时，就可以事先声明成 `any` 类型

```js
var notSure: any = 4
notSure = 'maybe a string instead' // OK, 声明为 string 类型
notSure = false // OK, 声明为 boolean 类型
```

当你只知道一部分数据的类型时，`any` 类型也是有用的，比如你有一个数组，它包含了不同的类型的数据

```js
let list: any[] = [1, true, 'free']

list[1] = 100
```


## Void

`void` 类型有点像是与 `any` 类型相反，它表示没有任何类型，当一个函数没有返回值时，你通常会见到其返回值类型是 `void`，一般用来定义一个没有返回值的函数，或者定义一个变量而这个变量只能有 `null` 或者 `undefined` 的赋值可能性

```js
function warnUser(): void {
  alert('This is my warning message')
}
```


## Never

`never` 类型表示的是那些永不存在的值的类型，`never` 类型是任何类型的子类型，也可以赋值给任何类型，然而没有类型是 `never` 的子类型或可以赋值给 `never` 类型（除了 `never` 本身之外），即使 `any` 也不可以赋值给 `never`，下面是一些返回 `never` 类型的函数

```js
// 返回 never 的函数必须存在无法达到的终点
function error(message: string): never {
  throw new Error(message)
}

// 推断的返回值类型为 never
function fail() {
  return error('Something failed')
}

// 返回 never 的函数必须存在无法达到的终点
function infiniteLoop(): never {
  while (true) {

  }
}
```







----


----









## 严格的对象赋值检查

对象赋值时必需严格按照声明时指定的属性类型以及属性个数来操作

```js
var x: { foo: number }

x = { foo: 3 }
x = { foo: 1, baz: 2 }  // 错误, 初始化时没有指名有 `baz` 属性

var y: { foo: number, bar: number }
y = { foo: 1, baz: 2 }  // 错误, 超过或者缺少属性 `baz`
```

可以用新对象对原有对象进行赋值, 要求新对象必需包含原对象属性值, 且赋值后多余属性不可以被使用

```js
var x: { foo: number }
var x1 = { foo: 1, bar: 2 }
x = x1 // 只能对共同的属性进行赋值

console.log(x.foo)  // OK
console.log(x.baz)  // 错误, 缺少属性 `baz`

var y: { foo: number, bar: number }
var y1 = { foo: 1, bar: 2 }
y = y1

console.log(y.foo) // OK
console.log(y.bar) // OK

// 表明 x 可以有任意的名字,但必需要是 string 类型的
var x: { foo: number, [x: string]: any } 
x = { foo: 1, bar: 2 }  // 正确
```



## 关于 Duck Typing

也就是所谓的鸭子类型

```js
class Duck {
  quack() {
    console.log('呱呱呱')
  }

  feathers() {
    console.log('是一个有灰色的羽毛的鸭子')
  }
}

class Person {
  quack() {
    console.log('这个人在模仿一只鸭子,呱呱叫')
  }

  feathers() {
    console.log('穿着一个鸭绒背心的人')
  }
}

function inTheForest(duck: any) {
  duck.quack()
  duck.feathers()
}

function game() {
  var duck = new Duck()
  var person = new Person()

  inTheForest(duck)
  inTheForest(person)
}

game()
```


## 隐式/显示 类型转换

`TypeScript` 中可以显示表明对象的类型，转换一个类型到其它类型时可以使用 `<>` 符号, 如 `<T>value`，`any` 类型可以转换成任意对象类型，反之亦然

```ts
class A {
  run() {
    console.log('run run run')
  }

  jump() {
    console.log('jump jump jump')
  }
}

class B {
  run(): void {
    console.log('slow run')
  }

  cry() {
    console.log('cry cry cry')
  }

}

var a = new A()        // 隐式转换 a 的类型
var a1: A = new A()    // 也可以显示的表明 a1 的类型

var b = new B()
a = b                  // 错误，类型 B 不能直接赋值给类型 A，因为在 B 中没有定义 jump 方法
a = <A>b               // 错误，ts 中使用 `<>` 进行强制类型转换，原因同上

var b1: any = new B()  // 声明为 any 类型 any 类型可以转成任意其它类型，反之亦然
a = b1                 // OK
b1 = a                 // OK

var b2: any = new B()
var newA = <A>b2       // 把 b2 的 B 类型转为 A 类型
newA.run()             // OK, 打印 slow run
newA.jump()            // 编译期间不会有问题，但是运行期间会出现异常 `newA.jump is not a function`
```







----

----









## Enums 类型

一个基本的例子

```js
enum Color {
	Red,
	Green,
	Blue
}

var colorValue = Color.Red

// 这里会提示错误, 因为 enum 值为 number 类型，并且是从 0 开始的，所以赋值 string 会得到一个错误
colorValue = '2'
```

## Enums 和 Numbers

`TypeScript` 中的 `enum` 类型是基于 `number` 的，所以可以使用 `number` 类型变量对其赋值

```js
enum Color {
	Red,
	Green,
	Blue
}

var colorValue = Color.Red

// 这样是可以的，因为 colorValue 隐式转为了 number 类型
colorValue = 2
```


## Enums 内部原理

打开上面示例编译后的 `JavaScript` 文件，可以发现结果如下

```js
var Color
(function (Color) {
	Color[Color['Red'] = 0] = 'Red'
	Color[Color['Green'] = 1] = 'Green'
	Color[Color['Blue'] = 2] = 'Blue'
})(Color || (Color = {}))
```

`Color[Color['Red'] = 0] = 'Red'` 其实是等价于 `Color['Red'] = 0` 和 `Color[0] = 'Red'` 的，所以我们可以在 `TypeScript` 中以下面这样的方式来使用 `Enums` 类型

```js
enum Color {
	Red,
	Green,
	Blue
}

console.log(Color['Red'])     // 0
console.log(Color[0])         // 'Red'
console.log(Color[Color.Red]) // 'Red' 因为 `Color.Red == 0`
```

也可以定义两个同名的 `enum` 类型，但是其中一个 `enum` 类型必需设置一个初始标识

```js
enum Color {
	Red,
	Green,
	Blue
}

enum Color {
	DarkRed = 3,
	DarkGreen,
	DarkBlue
}
```

也可以使用 `const` 来修饰 `enum`

```js
const enum Color {
	Red,
	Green,
	Blue
}

var value = Color.Red
```

可以查看编译后的 `JavaScript` 文件，代码量已经大大减少了，如下所示

```js
var value = 0 /* Red */
```

> 扩展阅读：[Enums](https://basarat.gitbooks.io/typescript/content/docs/enums.html)







----

----







## 函数 Function

函数（`function`）分为署名函数（`named function`）和匿名函数（`anonymous function`）

```js
// named function
function sub(x, y) {
  console.log(x - y)
}

// anonymous function
var sub1 = function (x, y) {
  console.log(x - y)
}
```

依据上面的例子来添加类型相关信息

```js
function sub(x: number, y: number): number {
  return x - y
}

var sub1 = function (x: number, y: number): number {
  return x - y
}
```

我们也可以使用 `=>` 符号连接参数类型与反回值类型，如果没有任何反回值时要记得添写 `void`

```js
var sub2: (x: number, y: number) => number = function (x: number, y: number): number {
  return x - y
}

// 只要类型相匹配就可以，名称无关紧要，增加函数类型是为了增加函数的可读性
var sub2: (source: number, dest: number) => number = function (x: number, y: number): number {
  return x - y
}
```

`TypeScript` 可以正确的判断出函数所对应的类型

```js
//右边指定类型,左边没有指定
var sub3 = function (x: number, y: number): number {
  return x - y
}

//左边指定类型,右边没有指定
var sub4: (x: number, y: number) => number = function (x, y): number {
  return x - y
}
```

## 函数的参数

在 `JavaScript` 里函数的参数是可有可无的，但是在 `TypeScript` 中每个参数都是必需的，即声明时需要传入两个参数，在使用时传入的参数个数必需是两个而且类型必需一致，否则在编译时就会抛出错误

```js
function twoParam(one: string, two: string): string {
  return one + two
}

twoParam('a', 'b')

// 下面两个在编写的时候就会提示错误
twoParam('a')             //错误 参数数量不匹配
twoParam('a', 'b', 'c')   //错误 参数数量不匹配
```

如果就想在 `TypeScript` 中使用可选的参数，需要在不确定的参数后面加上 `?` 符号，来说明的是可选参数，但是注意必需放到最后

```js
function buildName(firstName: string, lastName?: string) {
  if (lastName) {
    return firstName + ' ' + lastName
  } else {
    return firstName
  }
}

var result1 = buildName('zhangsan')  // 正确
var result2 = buildName('zhangsan', 'lisi', 'wangwu')  // 错误 参数数量不匹配
var result3 = buildName('zhangsan', 'lisi')  // 正确
```

也可以为可选参数来指定一个默认值

```js
function buildName(firstName: string, lastName = 'zhangsan') {
  // ...
}
```

如果不确定可选参数的个数，可以使用 `ES6` 中的 `...` 运算符

```js
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + ' ' + restOfName.join(' ')
}

var employeeName = buildName('zhangsan', 'lisi', 'wangwu', 'zhaoliu')
```


## this

在 `JavaScript` 中使用 `this` 时需要注意 `this` 所处的上下文，不同的绑定方法会造成不同的结果，比如

```js
var myObj = {
  name: 'zhangsan',

  say: function (str: string) {
    return function () {
      console.log(this.name + ' ' + str)
    }
  }
}

var say = myObj.say('hello')

// undefined hello
say()
```

可以看到 `this.name` 的返回结果为 `undefined`，说明 `this` 没有绑定到 `myObj` 这个上下文中，这时可以使用箭头函数（`Lambda`）写法的话，`TypeScript` 会在编译时自动为我们绑定 `this` 到当前上下文中

```js
var myObj = {
  name: 'zhangsan',

  say: function (str: string) {
    return () => console.log(this.name + ' ' + str)
  }
}

var say = myObj.say('hello')

// zhangsan hello
say()
```


## 函数的重载

重载的好处是在编写代码时由编译器负责检查传入参数类型及反回类型，使用时可以返回与传入参数类型相匹配的返回类型

```js
function add(arg1: string, arg2: string): string     // 重载 1
function add(arg1: number, arg2: number): number     // 重载 2
function add(arg1: boolean, arg2: boolean): boolean  // 重载 3

//上面重载的实现
function add(arg1: any, arg2: any): any {
  return arg1 + arg2
}

// 只能传入之前定义后的 3 种参数类型，如果传入其它组合类型则会得到一个错误
console.log(add(1, 2))              // 返回类型为 number
console.log(add('Hello', 'World'))  // 返回类型为 string
console.log(add(true, false))       // 返回类型为 boolean
```







----

----









## 箭头函数

先来看一个简单的示例

```js
function Arrow(age) {
  this.age = age

  this.add = function () {
    this.age++
    console.log(this.age)
  }
}

var arrow2 = new Arrow(10)
setTimeout(arrow2.add, 1000)  // NaN
```

原因很简单，因为 `setTimeout` 执行的上下文环境为 `window`，使得 `add` 方法中的 `this` 脱离了原上下文而指向了 `window`，现在使用箭头函数来改造一下

```js
function Arrow1(age) {
  this.age = age

  this.add = () => {
    this.age++
    console.log(this.age)
  }
}

var arrow2 = new Arrow1(10)
setTimeout(arrow2.add, 1000) // 11
```

打开编译后的文件可以发现

```js
function Arrow1(age) {
  // 自动创建了一个 _this 变量指向了当前上下文
  var _this = this   
  this.age = age

  this.add = function () {
    // 在这里使用的是之前创建的 _this 中保存的上下文环境
    _this.age++ 
    console.log(_this.age)
  }
}

var arrow2 = new Arrow1(10)
setTimeout(arrow2.add, 1000)
```

上面的例子使用 `ES6` 语法来书写的话

```js
class Arrow {
  constructor(public age: number) { }

  add = () => {
    this.age++
    console.log(this.age)
  }
}

var arrow = new Arrow(10)
setTimeout(arrow.add, 1000) // 11
```

箭头函数使用过程中的几个坑

* 如果你想使用 `this` 为当前调用时的上下文时不要使用箭头函数，因为箭头函数会把 `this` 绑定到定义函数时的上下文环境
* 如果你正使用一些回调函数时请小心使用箭头函数，例如 `jquery`，`underscore` 等，因为这些函数库通常在回调函数中指明了 `this` 的特殊意义

比如 `jquery` 中的 `each` 方法

```js
$('li').each(function () {
  console.log($(this).index())
})
```

上例中的 `this` 实际代表每个 `li` 的 `dom` 对象，如果使用箭头函数则会得到错误结果，但是可以使用一个变量来保存 `this`

```js
let _self = this
$(selector).each(function () {
  console.log(_self) // 定义方法时的上下文
  console.log(this)  // 运行时另种库指定的上下文
}) 
```

另外有些回调函数会在回调方法中放置参数（`arguments`），比如

```js
$(selector).each(function (index, element) {
  // ...
})
```

这种情况下，如果你想使用 `index` 或 `element` 这些参数的话也不要使用箭头函数







----

----







## 联合类型（union types）

联合类型一般用于如果一个变量在声明时可以有几个不同的类型，需要跟据运行时所传入的参数不同而改变相应类型

```js
var command: string | string[]

command = 'strat'  // 正确，string

command = ['strat', 'stop']  // 正确，string[]

command = 123  // 错误，没有声明 number 类型
```

也可以使用 `typeof` 来进行判断

```js
function run(command: string | string[]) {
  // string 类型
  if (typeof command === 'string') { 
    command.substr(0)
  // string[] 类型
  } else { 
    command.join()
    // 这里则会报错，因为方法 substr 在 string[] 中不存在
    command.substr(0) 
  }
}

run('start')

run(['start', 'stop'])
```


## typeof 和 instanceof

可以使用 `typeof` 判断变量类型一般为 `number`，`boolean`，`string`，`function`，`object`，`undefined` 这几种之一 

```js
var x: any = /* ... */

if (typeof x === 'string') {
  /* 这个方法块内 x 会被当作 string 来进行处理 */
}
```

使用 `instanceof` 来推断一个类或接口的类型

```js
class Pig { eat() { } }

class People { say() { } }

var animal: Pig | People = new Pig()

if (animal instanceof Pig) {
  animal.eat()
} else if (animal instanceof People) {
  animal.say()
  // 错误，eat 方法在 People 中不存在
  animal.eat()
}
```


## 智能类型推断（Better Type Inference）

```js
// other 跟据现有值，会被当作一个 Array<string|number>
var other = ['abc', 2]
other[0] = 888    // 正确
other[0] = false  // 错误，不是一个 string 或 number
```


## 类型别名（Type Aliases）

当联合类型过长时，可以使用 `type` 为类型定义一个别名

```js
type StringNumberOrBoolean = string | number | boolean
type PrimitiveArray = Array<string | number | boolean>
type MyNumber = number
type Callback = () => void
type CallbackWithString = (string) => void

// 使用上面的别名
function work(x: StringNumberOrBoolean) {
  // ...
}

function usingCallback(f: CallbackWithString) {
  f('This is a string')
}
```

## 自定义类型推断（User defined type guards）

```js
interface Animal { name: string }
interface Cat extends Animal { meow() }

// 使用 a is Cat，当条件为真时，将会把 a 转为 Cat 类型
function isCat(a: Animal): a is Cat {
  return a.name === 'kitty'
}

var x: Animal

if (isCat(x)) {
  x.meow() // OK，在这个范围内 x 是 Cat 类型
}
```


## 元祖（tuple）

元组是一个 `strict array`（严格的数组），因为他不只声明了一个数组，并且同时还规定数组中每个索引处的元素类型，比如声明一个数组

```js
// 类型为 Array<string | number>
var arr = [1, 2, 3, 'a', 'b', 'c']

// 可以改变指定下标处的值的类型，只要为 string 或 number 之一即可
arr[0] = 'abc' 
```

如果需要一个第一位为 `number` 类型，第二位为 `string` 类型，第三位为 `boolean` 类型的数组的话，可以声明一个元组类型

```js
var tuples: [number, string, boolean]

tuples[0] = 123
tuples[1] = 'abc'
tuples[2] = true

// 类型错误，指定索引处应为 number 类型
tuples[0] = 'type error'  

// 如果超出元组声明的类型时，以后的赋值类型又变为 Array<number | string | boolean>，只要是三者之一即可
tuples[3] = 'sure'
```







----

----






## 类与继承

在 `TypeScript` 中也可以使用类，继承，接口等这些概念来清晰我们的代码结构

## class

使用 `class` 关键字定义一个类，`constructor` 定义这个类的构造函数，使用 `this` 为成员变量赋值

```js
class Car {
  engine: string

  constructor(engine: string) {
    this.engine = engine
  }

  start() { }
}
```

## inheritance

现在我们有了一个汽车类，但汽车分好多种，轿车，`SUV`，跑车等，现在我们继承 `Car` 类，生成一个 `SUV` 类

```js
class SUV extends Car {
  engine: string

  constructor(engine: string) {
    super(engine)
  }

  start() {
    console.log(this.engine + ' started!')
  }
}
```

首先必需使用 `super` 关键字调用父类的构造函数，之后我们为 `SUV` 类重写 `start` 方法

```js
var suv = new SUV('V8')
suv.start() 
```

几个需要注意的地方

* 多态

```js
// su1 指向原对象
var su1: Car = new Car('V6')  

// 这里调用的是原对象的方法
su1.start()  
```

* 强制类型转换

```js
class SuperCar extends Car {

  // public engine:string 的意思为 this.engine = engine
  constructor(public engine: string) {
    super(engine)
  }

  start() {
    console.log(this.engine + ' started!')
  }

  speed(s: number = 300) {
    console.log('speed ' + s + 'km feels like flying!')
  }
}

var superCar = new SuperCar('v12')

var suv = new SUV('v8')
var car = new Car('v4')

suv = car  // 正确 suv 类与 car 类具有相同的属性及方法，可以互相赋值，不需要强制类型转换
car = suv  // 同上

superCar = car              // 错误，因为 car 类中没有实现 SuperCar 中的 speed 方法
superCar = car as SuperCar  // 正确，可以使用强制类型转换
superCar = <SuperCar>car    // 也可以使用这种强制类型转换
```


## 抽象类

面向对象的主要特点是 

1. 封装 
2. 继承 
3. 抽象 
4. 多态

在 `class` 之前加上 `abstract` 关键字表名这个类是一个抽象类，在方法前加上 `abstract` 表名这个方法是抽象方法，抽象方法只需要写出方法的定义，由实现类完成它的具体细节，还是以上面的示例为例

```js
abstract class Car {
  engine: string

  constructor(engine: string) {
    this.engine = engine
  }

  // 抽象方法
  abstract start()
}

class SUV extends Car {
  constructor(engine: string) {
    super(engine)
  }

  start() {
    console.log(this.engine + ' started!')
  }
}

var suv = new SUV('V8')
suv.start()

// 当尝试实例化一个抽象类时，会得到一个错误
var car = new Car('v4')
```









----

----







## private

使用 `private` 标识的成员变量，不允许在本类之外的地方使用

```js
class Car {
  private engine: string

  constructor(engine: string) {
    this.engine = engine
  }
}

// 错误
var engine = new Car('').engine 
```

之前提到过，当两个类有相同的属性和方法时可以互相赋值，比如

```js
class One {
  name: string

  say() {
    // ...
  }
}

class Two {
  name: string

  say() {
    // ...
  }
}

var one = new One()
var two = new Two()

one = two
```

但如果加上 `private` 的话就会得到一个错误

```js
class One {
  private name: string

  say() {
    // ...
  }
}

class Two {
  private name: string

  say() {
    // ...
  }
}

var one = new One()
var two = new Two()

// 错误，不同源 private 之间不可以互相赋值
one = two 

class Three extends One {

}

// 正确
var three = new Three()
one = three 
```



## protected

`protected` 可以在本类及其子类中访问

```js
class ListString {
  private contents: string[]

  constructor() {
    this.contents = []
  }
  
  protected setElement(index: number, item: string) {
    this.contents[index] = item
  }

}

class StackString extends ListString {
  currentIndex: number

  constructor() {
    super()
    this.currentIndex = 0
  }

  public push(item: string) {
    this.setElement(this.currentIndex, item)
    this.currentIndex++
  }
}

var stack = new StackString()

// 错误 setElement 方法是 protected，不可以在外部访问 
stack.setElement(0, 1)
```


## static

使用静态变量或方法可以使用 `类别.方法名` 或 `类别.变量名` 的方式使用，即可以直接当成变量来访问，也可以作为方法来使用

```js
class MyClass{
  static name:string = 'zhangsan'

  static foo(){
    console.log('foo')
  }
}

MyClass.foo()
MyClass.name
```







----

----






## Getters 和 Setters

`typeScript` 支持使用 `get` 和 `set` 在访问这个变量时对其进行拦截

```js
var passcode = 'secret passcode'

class Employee {
  private _fullName: string

  public get fullName(): string {
    return this._fullName
  }

  public set fullName(newName: string) {
    if (passcode && passcode == 'secret passcode') {
      this._fullName = newName
    }
    else {
      alert('Error: Unauthorized update of employee!')
    }
  }
}

var employee = new Employee()
employee.fullName = 'Bob Smith'

if (employee.fullName) {
  console.log(employee.fullName)
}
```

需要注意的一点：`get` 和 `set` 前如果不加修饰符时默认是 `public` 类型的，并且 `typeScript` 要求修饰符要一致，即 `get` 和 `set` 前要不都不加，或都加 `public` 修饰，或都为 `protected` 修饰







----

----






## Interfaces

严格的类型检查是 `TypeScript` 的一个核心原则，而接口正是对它这一原则的体现，接口的功能是为类型命名，或约束实现了同一个接口的不同实现类外型的一致

## 使用接口描述属性

```js
function sayMan(man: { name: string }) {
  console.log(man.name)
}

// 在外部声明时可以附加其它属性，可以正常运行
let man = { name: 'zhangsan', age: '18' }
sayMan(man)

// 但是直接附值时不能附加额外的属性
sayMan({ name: 'zhangsan', age: '18' })
```

当使用函数时，如果先定义好变量再传递给函数时，变量中可以不只一个属性，当在使用函数时才赋值的话，必需有且仅有一个要求的属性，即不管怎么用，函数定义时要求的值必需在，使用 `interface` 改写上面的示例

```js
interface Man {
  name: string
}

function sayMan(man: Man) {
  console.log(man.name)
}

// 在外部声明时可以附加其它属性，可以正常运行
let man = { name: 'zhangsan', age: '18' }
sayMan(man)

// 但是直接附值时不能附加额外的属性
sayMan({ name: 'zhangsan', age: '18' })
```

上面这个例子中 `Man` 是一个接口，但其没有具体的实现类，它的存在的作用只是为了约束属性的名称和其匹配的类型，即只关注属性的外型，只要传入的变量具有 `name` 属性并且为 `string` 类型，都可通过检查


## 接口中的可选属性

可以使用 `?` 来表示参数是不确定的

```js
interface PhoneInfo {
  screen: string
  keyboard?: string
}

function myPhone(phone: PhoneInfo) {
  console.log('screen resolution: ' + phone.screen)

  if (phone.keyboard) {
    console.log('keyboard info: ' + phone.keyboard)
  }
}

// OK
myPhone({ screen: '2k' }) 

// OK
myPhone({ screen: '1k', keyboard: 'T9' })
```

其中 `keyboard?` 后面的问号表示这个参数是不确定的，可能会有也可能没有，要到具体运行时才能确定


## 使用接口描述函数

接口可以描述 `JavaScript` 中的 `Object` 的属性和函数，下面是如何表示一个函数的使用

```js
interface CallFun {
  (name: string, phoneNumber: string): boolean
}

var calFun: CallFun = function (name: string, phoneNumber: string): boolean {
  console.log('call ' + name + ', phone number: ' + phoneNumber)
  return true
}

calFun('zhangsan', '13888888888')
```

与普通函数相比，只是保留了参数声明及其反回值而已，并且其中声明的参数只要类型匹配即可

```js
var calFun: CallFun = function (myName: string, myNumber: string): boolean {
  console.log('call ' + myName + ', phone number: ' + myNumber)
  return true
}

calFun('lisi', '18888888888')
```


## 使用接口表示数组

```js
interface Dictionary {
  [index: number]: string
  length: number
}

var arr: Dictionary = ['1', '2', '3']

// 3
console.log(arr.length) 
```


## 接口的继承

`TypeScript` 的接口的继承是通过 `extends` 关键字实现的，同时从多个接口继承时时用 `','` 分隔

```js
// 键盘
interface Keyboard {
  layout: string
}

// 屏幕
interface Screen {
  resolution: string
}

// 手机一般是由键盘和屏幕组成的
interface Phone extends Keyboard, Screen {
  call(phoneNumber: string) 
}
```


## 接口继承类

当接口继承一个类时，会继承这个类的所有成员，但是会忽略具体的实现

```js
class Point {
  x: number
  y: number
}

interface Point3d extends Point {
  z: number
}

var point3d: Point3d = { x: 1, y: 2, z: 3 }
```







----

----






## 类中的修饰字

|属性|解释|
|-|-|
|public|定义类的公有成员，就是说我们可以自由访问这些成员，如果不写，默认就是 public|
|private|定义类的私有成员，就是说不能在声明它的类的外部访问它，在实例里也不行（比如继承）|
|protected|定义类的保护成员，与上面的区别就是他在实例里可以访问（比如继承的实例中）|
|static|定义类的静态属性，存在于类本身上面而不是类的实例上，所以访问的时候要加该类名|
|abstract|定义抽象类，它们不会被实例化，仅提供继承|

## public

共有成员，默认就是这个

## private

```js
class Animal {
  private name: String
  constructor(theName: String) { this.name = theName }
}

// 会报错，因为属性 name 为私有
new Animal('Dog').name
```

一般在我们比较两种不同的类型时，并不在乎它们从哪儿来的，如果所有成员的类型都是兼容的，我们就认为它们的类型是兼容的，但是当我们比较带有 `private` 或 `protected` 成员的类型的时候，情况就不同了，**如果其中一个类型里包含一个 private 成员，那么只有当另外一个类型中也存在这样一个 private 成员，并且它们是来自同一处声明时，我们才认为这两个类型是兼容的**

```js
class Animal {
  private name: String
  constructor(theName: String) { this.name = theName }
}

class Rhino extends Animal {
  constructor() {
    super('Rhino')
  }
}

class Employee {
  private name: String
  constructor(theName: String) { this.name = theName }
}

let animal = new Animal('Dog')
let rhino = new Rhino()
let employee = new Employee('Bob')

animal = rhino  // OK
animal = employee // 报错
```

因为 `animal` 和 `employee` 不同源，所以不能相互赋值，虽然他们都长得一样，但是 `Animal` 的子类的实例就可以赋值了，因为他们同源，也存在 `name` 属性

* 因为 `Animal` 和 `Rhino` 共享了来自 `Animal` 里的私有成员定义 `private name: string`，因此它们是兼容的
* 然而 `Employee` 却不是这样，当把 `Employee` 赋值给 `Animal` 的时候，得到一个错误，说它们的类型不兼容，尽管 `Employee` 里也有一个私有成员 `name`，但它明显不是 `Animal` 里面定义的那个


## protected

还是上面的例子，稍微做一下调整

```js
class Animal {
  protected name: String
  // 但是如果换成 private name: String 就不行了
  // 因为不允许子类使用，这就是 protected 与 private 的区别
  constructor(theName: String) { this.name = theName }
}

class Rhino extends Animal {
  constructor() { super('Rhino') }
  getName(): String {
    return this.name
  }
}

let animal = new Rhino()

// 这样是 OK 的
console.log(animal.getName())
```

## static

类的静态属性，这些属性存在于类本身上面而不是类的实例上，所以调用的时候需要通过类名

```js
class Grid {
  static origin = { x: 0, y: 0 }
  
  calculateDistanceFromOrigin(point: { x: number, y: number }) {
    let xDist = (point.x - Grid.origin.x)
    let yDist = (point.y - Grid.origin.y)
    return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale
  }

  constructor(public scale: number) { }
}

let grid1 = new Grid(1.0)  // 1x scale
let grid2 = new Grid(5.0)  // 5x scale

console.log(grid1.calculateDistanceFromOrigin({x: 10, y: 10}))
console.log(grid2.calculateDistanceFromOrigin({x: 10, y: 10}))
```


## abstract

`abstract` 关键字是用于定义抽象类和在抽象类内部定义抽象方法，抽象类中的抽象方法不包含具体实现并且必须在派生类中实现，抽象方法必须使用 `abstract` 关键字并且可以包含访问符

```js
abstract class Department {

  constructor(public name: string) {
  }

  printName(): void {
    console.log('Department name: ' + this.name)
  }

  abstract printMeeting(): void // 必须在派生类中实现
}

class AccountingDepartment extends Department {

  constructor() {
    super('Accounting and Auditing') // constructors in derived classes must call super()
  }
  printMeeting(): void {
    console.log('The Accounting Department meets each Monday at 10am.')
  }

  generateReports(): void {
    console.log('Generating accounting reports...')
  }
}

let department: Department // ok to create a reference to an abstract type
department = new Department() // 报错，无法创建抽象类的实例

department = new AccountingDepartment() // ok to create and assign a non-abstract subclass
department.printName()
department.printMeeting()
department.generateReports() // 报错，Department 上不存在 generateReports 属性
```

`AccountingDepartment` 类继承 `Department` 抽象类，`printName` 抽象方法，在子类中重定义了，`generateReports` 方法在抽象类中不存在，被拒绝了，当然，创建抽象类的子类也被拒绝了


## 参数简写

我们经常可以看到以下这样的写法

```js
constructor(public name: string) { }
```

其实本质上是等价于

```js
public name: String 
constructor(name: String) { this.name = name }
```


待续

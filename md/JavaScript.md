# JavaScript

## 值类型，引用类型

- 6大基本类型：`String`,  `Boolean` , `Number`,  `Null`, `Undefined` , `Symbol`, `Symbol` 用的比较少。
    - 基本类型的数据存在栈内存上。
- 引用类型：`Object`
    - 栈内存中保存了变量标识符和指向堆内存中该对象的指针。
    - 堆内存保存了对象的值。

## 深浅拷贝的问题

解决问题的场景：

```JavaScript
let a = {
    age: 1
}

let b = a
a.age = 2
console.log(b.age) // 2
```

从上述例子中我们可以发现，如果给一个变量赋值一个对象，那么两者的值会是同一个引用，其中一方改变，另一方也会相应改变。
通常在开发中我们不希望出现这样的问题，我们可以使用浅拷贝来解决这个问题。[相关链接](https://www.zhihu.com/question/23031215)

### 浅拷贝(shallow copy)

首先`深拷贝`和`浅拷贝`只针对像 `Object`, `Array` 这样的复杂对象的。
简单来说，浅复制只复制一层对象的属性，而深复制则递归复制了所有层级。
`浅拷贝`一般用 `Object.assign` 或者扩展运算符 `...` 来解决。

```JavaScript
let a = {
    age: 1
}

let b = Object.assign({}, a)
a.age = 2
console.log(b.age) // 1
```

或者

```JavaScript
let a = {
    age: 1
}

let b = {...a}
a.age = 2
console.log(b.age) // 1
```
- 浅拷贝数组

```js
var a = b.slice()
var a = b.concat([])
```
[js slice 是不是浅拷贝？](https://www.zhihu.com/question/56690271)

### 深拷贝(deep copy)

深拷贝一般用 JSON.parse(JSON.stringify(object)) 来解决。

```JavaScript
let a = {
    age: 1,
    jobs: {
        first: 'FE'
    }
}

let b = JSON.parse(JSON.stringify(a))
a.jobs.first = 'native'
console.log(b.jobs.first) // FE
```
## 闭包问题

- 闭包的定义：函数 A 返回了一个函数 B，并且函数 B 中使用了函数 A 的变量，函数 A 就被称为闭包。
- 使用闭包实现如下程序: 函数每调用一次，该函数的返回值加 1。

```JavaScript
var a = (function() {
    var i = 0
    return function() {
        i++
        return i
    }
})() // a 就是个闭包

a() // 1
a() // 2
```

## 变量声明提升

```js
console.log(a) // undefined
var a = 1

// 相当于
// var a
// console.log(a)
// a = 1

console.log(b()) // 2
function b() {
    return 2
}

// var b = function() {return 2}
// console.log(b())

console.log(c()) // c is not a function
var c = function() {
    return 3
}

// var c
// console.log(c())
// var c = function() {return 3}

// 注意，let 或者 const 声明的变量不具备声明提升的特性
console.log(d)
let d = 4
```

## this

```js
var x = 0
function test() {
    console.log(this.x)
}

var o = {}
o.x = 1
o.m = test
o.m.apply() // 0 apply 需要指定一个 this，这里没给 apply 没给参数，则 this 指向了 window 所以 this.x 是 0
o.m() // 1 谁调用，指向谁
o['m']() // 1
o.m.apply() //  0  相当于改变 this, 指向 window

复杂的 this

var gua = 'name 001'
var foo = function() {
    console.log(arguments['0']())
}
var o = {}
o.gua = 'name 001'
o.func = foo

o.func(function(){
    return this.gua
}) // undefined  this 指向 func 中的 argumets，但 arguments是个伪数组，其实是 Object, arguments 中没有 gua 这个字段，所以是 undefined

var gua = 'name 001'
var foo = function() {
    console.log(arguments['0']())
}
var o = {}
o.gua = 'name 001'
o.func = foo

o.func(function(){
    console.log(this)
    return this[0]
})
// f (){
// 	console.log(this)
//     return this['0']
// }

// 因为 arguments['0'] 是存在的, 即 func 的第一个参数

o.func2 = function(){
    arguments.gua = 'gua'
    console.log(arguments[0]())
}

o.func2(function(){
    return this.gua
}) // gua  此时，arguments.gua 是存在的
// 问题的关键在于搞清楚 this 的指向

function foo() {
  console.log(this.a)
}
var a = 1
foo() // 1    这里 this 指向 window, 相当于 window.foo()

var obj = {
  a: 2,
  foo: foo,
}
obj.foo() // 2    obj 调用的 foo()，所以 this 指向 a

// 以上两者情况 this 只依赖于调用函数前的对象，优先级是第二个情况大于第一个情况

// 以下情况是优先级最高的，this 只会绑定在 c 上，不会被任何方式修改 this 指向
var c = new foo()
c.a = 3
console.log(c.a)

// 还有种就是利用 call，apply，bind 改变 this，这个优先级仅次于 new
```

## `call`, `apply`, `bind` 区别

`call` 和 `apply` 都是为了解决改变 `this` 的指向, 并**立即调用**。
除了第一个参数外，`call` 可以接收一个参数列表，`apply` 只接受一个参数数组。不传入第一个参数，那么默认为 `window`。
`bind` 方法是 return 一个改写过 `this` 和参数的 `function`，这个 `function` 可以**稍后调用**。

```js
let a = {
    value: 1
}

function getValue(name, age) {
    console.log(name)
    console.log(age)
    console.log(this.value)
}

// call apply
getValue.call(a, 'Kowal$ki', '25')
getValue.apply(a, ['Kowal$ki', '25'])

// Kowal$ki
// 25
// 1

// bind 的用法

const log = console.log.bind(console, '###') // 将 console.log 绑定在 console 上面，返回的函数用 log 表示
log('hello') // ### hello
```

## 简单的构造函数

```js
function Foo() {
    this.name = 'a'
}

var f1 = new Foo()
f1.name = 'b'
console.log(f1.name) // b

var f2 = new Foo()
console.log(f2.name) // a
```

## 原型链 `prototype`

继承时，JavaScript 只有一种结构：对象。
每个对象都有一个私有属性（称之为 [[Prototype]]），它持有一个连接到另一个称为其 `prototype` 对象（原型对象）的链接。
该 `prototype` 对象又具有一个自己的原型，层层向上直到一个对象的原型为 `null`。
根据定义，`null` 没有原型，并作为这个原型链中的最后一个环节。

- [Javascript继承机制的设计思想](http://www.ruanyifeng.com/blog/2011/06/designing_ideas_of_inheritance_mechanism_in_javascript.html)

- [Javascript 面向对象编程（一）：封装](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_encapsulation.html)

- [Javascript面向对象编程（二）：构造函数的继承](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html)

- [Javascript面向对象编程（三）：非构造函数的继承](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance_continued.html)

```js
function Foo() {
    this.name = 'a'
}

Foo.prototype.logName = function() {
    console.log('name is', this.name)
}

var f1 = new Foo()
f1.logName() // name is a  没有修改原型 Foo 的 name

var f2 = new Foo()
f2.logName = function() {
    console.log('name')
}
f2.logName() // name  覆盖了原型链上的 logName() 函数

var f3 = new Foo()
f3.name = 'c'
f3.logName() // name is c  只覆盖了 Foo 上的 name
```

## `arguments` 函数隐含的参数，是所有参数构成的一个类数组

```js
(function() {
    return typeof arguments
})()
```

## `setTimeout` 和 `setInterval` 两者的区别

- `setTimeout` 是过一定时间之后执行 `function，` 然后终止
- `setInterval` 是每过一段时间之后执行 `function`，清除用 `clearInterval(id)` 的方法

`setTimeout` 与循环结合:

```js
for (var i = 0; i < 5; i++) {
    setTimeout(function() {
        console.log(new Date(), i)
    }, 1000)
}
console.log(new Date(), i)

// Sat Dec 01 2018 16:37:48 GMT+0800 (中国标准时间) 5   这是第六行的 console.log()
// Sat Dec 01 2018 16:37:49 GMT+0800 (中国标准时间) 5
// Sat Dec 01 2018 16:37:49 GMT+0800 (中国标准时间) 5
// Sat Dec 01 2018 16:37:49 GMT+0800 (中国标准时间) 5
// Sat Dec 01 2018 16:37:49 GMT+0800 (中国标准时间) 5
// Sat Dec 01 2018 16:37:49 GMT+0800 (中国标准时间) 5
```

## Ajax 过程：
Ajax: 浏览器提供的使用 `HTTP` 协议收发数据的接口

// 0 1 2 3 4 各代表什么含义
// 0 代理被创建，但并未 open()
// 1 已经调用 open()
// 2 已经调用 send() 并且头部和状态已经获得了
// 3 正在下载
// 4 下载完成

```JavaScript
const ajax = (method, path, data, responseCallback) => {
    // 发送登录数据
    let r = new XMLHttpRequest()
    // 设置请求方法和请求地址
    r.open(method, path, true)
    // 设置发送的数据的格式
    r.setRequestHeader('Content-Type', 'application/json')
    // 注册响应函数
    r.onreadystatechange = () => {
        if(r.readyState == 4) {
            const response = JSON.parse(r.response)
            responseCallback(response)
        }
    }
    r.send(data)
}
```

## 事件捕获, 事件冒泡, 事件委托

用 `addEventListener` 讲清楚这三个概念
`addEventListener(event, callback, useCapture)`

- `event` 事件名称 `click`
- `callback` 事件监听函数，监听到事件后执行的函数，第一个参数为事件本身。
- `useCapture` 默认 `false` ，`false` 代表只触发**事件冒泡**，不触发**事件捕获**。
- 可以调用 `event.stopPropagation()` 即可阻止事件继续传播。
- **事件委托**: 如果一个节点中的子节点是动态生成的，那么子节点需要注册事件的话应该注册在父节点上。
  换言之，可以利用冒泡原理，在父元素或者祖先元素上绑定事件监听函数，当事件传递到该元素上之后，callback 查询 e.target.id 或者 dataset 等其他标记，来判断事件并处理函数。优点是不需要给每一个子元素都绑定事件，节省内存。
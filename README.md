
# 40道前端基础题，你能答对几道题？  

### 第一题  

```
if(false) {
  var a = 1;
  let b = 2;
}
console.log(a);
console.log(b);
```

> 解析  

```
// 输出
undefined

ReferenceError: b is not defined
```
`var` 不会产生块级作用域，`let` 会产生块级作用域  
代码相当于  
```
var a;
if (false) {
  a = 1;
  let b = 2;
}
console.log(a);
console.log(b);
```  
---

### 第二题  

```
var a = 1;
if(true) {
  console.log(a);
  let a = 2;
}
```  

> 解析  

```
// 输出
Uncaught ReferenceError: Cannot access 'a' before initialization
```

`let` 声明的变量不会提升，并且会产生暂存死区，在 `let` 声明之前访问变量会抛出异常。  

---  

### 第三题  

```
var a = {n: 1};
var b = a;
a.x = a = {n: 2};

console.log(a.n, b.n);
console.log(a.x, b.x);
```

> 解析  

```
// 输出
2 1
undefined {n: 2}
```

```
var b = a,此时a和b指向同一个对象。

.运算符比 = 运算符高,先计算`a.x`,此时 
b = {
    n:1,
    x:undefined
}

相当于给对象添加了x属性。

a.x = a = {n:2};

计算完a.x,再计算 = ,赋值是从右向左,此时a指向一个新对象。
a = {
    n:2
}

a.x已经执行过了,此时对象的x属性赋值为a,此时

对象 = {
    n:1,
    x:{
        n:2
    }
}

即:
a = {
    n:2
}

b = {
    n:1,
    x:{
        n:2
    }
}
```

---

### 第四题  

```
console.log(c);
var c;
function c(a) {
  console.log(a);
  var a = 3;
  function a() {

  }
}
c(2);
```

> 解析  

```
// 输出 

function c(a){
    console.log(a);
    var a = 3;
    function a(){
    }
}

function a(){
}
```

变量提升也有优先权，函数声明>arguments>变量声明。

---

### 第五题  

```
var c = 1;
function c(c) {
  console.log(c);
  var v = 3;
}
console.log(c);
c(2);
```

> 解析  

```
// 输出
1
Uncaught TypeError: c is not a function
```

由于函数声明会提升,当函数外的 `console.log(c)` 执行时,`c` 已经被赋值为 `1`。因此,执行 `c(2)` 时会抛出 `TypeError`,因为 `1` 不是函数。

---

### 第六题  

```
var name = "xinuo";
(function() {
  if(typeof name == "undefined") {
    var name = "sayno";
    console.log(name);
  } else {
    console.log(name);
  }
})();
```

> 解析  

```
// 输出
sayno
```

自执行函数执行时，会先进行变量的提升，在执行时，代码相当于：  

```
var name = "xinuo";
(function() {
  var name; // 变量name会提升到当前作用于顶部
  if(typeof name == "undefined") {
    name = "sayno";
    console.log(name);
  } else {
    console.log(name);
  }
})();
```

---

### 第七题  

```
var a = 10;
function test() {
  a = 100;
  console.log(a);
  console.log(this.a);
  var a;
  console.log(a);
}
test();
```

> 解析  

```
// 输出
100
10
100
```

`test()` 为函数独立调用，作用域中的 `this` 绑定为全局对象 `window`。  
`test` 函数执行时，`var a` 都被提升到了作用域顶部，因此函数作用域中存在一个变量 `a`，所以在函数中访问的 `a` 都是局部作用域中的 `a`。  

---

### 第八题  

```
if(!('a' in window)) {
  var a = 1;
}
console.log(a);
```

> 解析  

```
// 输出
undefined
```

由于 `if` 后的 `{}` 不会产生块级作用域(不包含let,const时),此时的代码相当于:  

```
var a;
if (!(a in window)) {
    a = 1;
}
console.log(a);
```

`var a` 相当于 `window.a`。因此 `!(a in window)` 转成布尔值为 `false`,不会执行 `a = 1`。所有 `console.log(a)` 输出 `undefined`。  

---  

### 第九题  

```
var a = 2;
function c(a, b) {
  console.log(a);
  a = 2;
  console.log(a);
}
c();
```

> 解析  

```
// 输出
undefined
2
```

跟第四题类似。  

---  

### 第十题  

```
var val = 1;
var obj = {
  val: 2,
  del: function() {
    console.log(this);
    this.val *= 2;
    console.log(val);
  }
}
obj.del();
```

> 解析  

```
// 输出
obj {val: 2, del: ƒ}
1
```

当通过 `obj.del()` 调用 `del` 函数时，`del` 函数作用域中的 `this` 指向为 `obj`。  
在函数作用域中访问 `val`，由于函数中没有变量 `val`,所以实际上访问的是全局作用域中的 `val`，即 `1`。  
这里考察 `this`的指向，一定要熟练掌握。  

---  

### 第十一题  

```
var name = "xinuo";
var object = {
  name: "sayno",
  getNameFunc: function() {
    return function() {
      return this.name;
    }
  }
}
console.log(object.getNameFunc()());
```

> 解析  

```
// 输出
xinuo
```

`object.getNameFunc()()` 先执行 `object.getNameFunc()` 返回一个函数：  

```
function() {
  return this.name;
}
```

返回的函数再执行，相当于：  

```
(
  function() {
    return this.name;
  }
)();
```

此时的 `this` 指向 `window`，因此输出的全局变量 `name` 的值是 `xinuo`。  

---  

### 第十二题  

```
var name = "xinuo"'
var object = {
  name: "sayno",
  getNameFunc: function() {
    var that = this;
    return function() {
      return this.name;
    }
  }
}
console.log(object.getNameFunc()());
```

> 解析  

```
// 输出
sayno
```
`object.getNameFunc()` 执行时,此时 `getNameFunc` 中的 `this` 绑定为 `object`,因此 `that = object`。`object.getNameFunc()` 返回的函数再执行时,产生闭包,因此返回的函数也能访问到外层作用域中的变量 `that`,因此 `object.name` 为 `object.name`,即 `sayno`。  

---

### 第十三题  

```
(function() {
  var a = b = 3;
})();
console.log(typeof a === 'undefined');
console.log(typeof b === 'undefined');
```

> 解析  

```
// 输出
true
false
```

首先要明白 `var a = b = 3` 是怎样执行的，代码相当于：

```
b = 3;
var a = b;
```

因此在自执行函数执行时, `b` 由于未经 `var` 等操作符声明,为全局变量。`a` 为函数作用域中的局部变量。因此在外面访问 `a` 和 `b` 时,其值分别为 `ReferenceError: a is not defined` 和 `3`。但是 `typeof` 检测未声明的变量不会抛出错误,会返回 `'undefined'`。因此 `typeof a` 和 `typeof b` 分别返回 `'undefined'` 和 `'number'`  

---

### 第十四题  

```
var a = 6;
setTimeout(function() {
  a = 666;
}, 0)
console.log(a);
```

> 解析  

```
// 输出
6
```

`setTimeout` 为宏任务。即使设置延迟为 `0ms`，也是等待同步代码执行完才会执行。因此 `console.log(a)` 输出 `6`。  

---  

### 第十五题  

```
function fn1() {
  var a = 2;
  function fn2() {
    a++;
    console.log(a);
  }
  return fn2;
}
var f = fn1();
f();
f();
```

> 解析  

```
// 输出
3
4
```

由于 `fn1` 函数执行后返回函数 `fn2`,此时产生了闭包。因此 `fn2` 中 `a` 访问的是 `fn1` 作用域中的变量 `a`,因此第一次 `a++`,之后 `a` 为 `3`,第二次之后 `a` 为 `4`。  

---  

### 第十六题  

```
var a = (function(foo) {
  return typeof foo.bar;
})({foo: {bar: 1}});
console.log(a);
```

> 解析  

```
// 输出
undefined
```

实参 `foo` 的值为 `{foo:{bar:1}`,因此 `typeof foo.bar` 为 `undefined`。  

`typeof foo.foo.bar` 为 `number`。

---  

### 第十七题  

```
function f() {
  return f;
}
console.log(new f() instanceof f);
```

> 解析  

```
// 输出
false
```

由于构造函数 `f` 的返回值为 `f`。因此 `new f()` 的值为 `f`。所以 `console.log(new f() instanceof f)` 为 `console.log(f instanceof f)`,即 `false`。

---  

### 第十八题  

```
function A() {
}
A.prototype.n = 1;

var b = new A();

A.prototype = {
  n: 2,
  m: 3
}
var c = new A();
console.log(b);
console.log(c);
```

> 解析  

```
// 输出
1 undefined
2 3
```

`var b = new A();` 实例化 `b` 时,`A` 的 `prototype` 为:  

```
A.prototype = {
  constructor:A,
  n:1
}
```

当访问 `b.n` 和 `b.m` 时,通过原型链找到 `A.prototype` 指向的对象上,即 `b.n = 1`，`b.m = undefined`。  
`var c = new A();` 实例化 `c` 时,`A` 的 `prototype` 为：

```
A.prototype = {
  n: 2,
  m: 3
}
```

当访问 `a.n` 和 `a.m` 时,通过原型链找到 `A.prototype` 指向的对象上,此时 `A.prototype` 重写,因此 `a.n = 2`，`b.m = 3`。

---  

### 第十九题  

```
var F = function() {}
var O = {}
Object.prototype.a = function() {
  console.log("a");
}
Function.prototype.b = function() {
  console.log("b");
}
var f = new F();
F.a();
F.b();
O.a();
O.b();
```

> 解析  

```
// 输出
a
b
a
TypeError: O.b is not a function
```

`F` 为函数,它也能访问 `Object` 原型上的方法,`O` 为对象,不能访问 `Function` 原型上的方法。

`F` 的原型链为:  

```
F => F.__proto__ => Function.prototype => Function.prototype.__proto__ => Object.prototype
```

由于 `Object.prototype` 在 `F` 的原型链上,所以 `F` 能访问 `Object.prototype` 上的属性和方法。即: `F.a()`, `F.b()` 能正常访问。  

`O` 的原型链为:  

```
O => O.__proto__ => Object.prototype
```

由于 `Function.prototype` 不在 `O` 的原型链上,因此 `O` 不能访问 `Function.prototype` 上的方法,即 `O.b()` 抛出错误。

如果你对原型和原型链掌握的好,试着理解下面的示例:  

```
console.log(Object instanceof Function);

console.log(Function instanceof Object);

console.log(Function instanceof Function);
```

---  

### 第二十题  

```
function Person() {
    getAge = function () {
        console.log(10)
    }
    return this;
}

Person.getAge = function () {
    console.log(20)
}

Person.prototype.getAge = function () {
    console.log(30)
}

var getAge = function () {
    console.log(40)
}

function getAge() {
    console.log(50)
}

Person.getAge();
getAge();
Person().getAge();
new Person.getAge();
getAge();
new Person().getAge();
```

> 解析  

```
// 输出
20
40
10
20
10
30
```

`Person.getAge();` 此时执行的是 `Person` 函数上 `getAge` 方法。  

```
Person.getAge = function () {
    console.log(20)
}
```

所以输出:20。    

`getAge();` 此时执行的是全局中的 `getAge` 方法。此时全局 `getAge` 方法为:  

```
function () {
    console.log(40)
}
```

所以输出:40。  

`Person().getAge();` 由于 `Person()` 单独执行所以,作用域中的 `this` 绑定为 `window`,相当于window.getAge()。同上,执行的都是全局getAge 方法,但是Person执行时,内部执行了  

```
getAge = function () {
    console.log(10)
}
```

因此全局 `getAge` 方法现在为:  

```
function () {
    console.log(10)
}
```

所以输出:10。  

`new Person.getAge();` 此时相当于实例化 `Person.getAge` 这个函数,伪代码:  

```
var b = Person.getAge;
new b();
```

所以输出:20  

`getAge();` 执行全局 `getAge` 方法,由于在 `Person().getAge()` 执行时把全局 `getAge` 方法赋值为:  

```
function () {
    console.log(10)
}
```

所以输出:10。  

`new Person().getAge();` 此时调用的是 `Person` 原型上的 `getAge` 方法:  

```
Person.prototype.getAge = function () {
    console.log(30)
}
```

所以输出:30。  

这里要注意:  

> 1.变量提升及提升后再赋值。  
> 2.调用构造函数时,带 `()` 和不带 `()` 的区别。  

---

### 第二十一题  

```
console.log(false.toString()); 
console.log([1, 2, 3].toString()); 
console.log(1.toString()); 
console.log(5..toString());
```

> 解析  

```
// 输出
'false'
'1,2,3'
Uncaught SyntaxError: Invalid or unexpected token
'5'
```

当执行 `1.toString();` 时,由于 `1.` 也是有效数字,因此此时变成 `(1.)toString()`。没有用 `.` 调用 `toString` 方法,因此抛出错误。  

正确的应该是:  

```
1..toString();
1 .toString();
(1).toString();
```

---  

### 第二十二题  

```
console.log(typeof NaN === "number")
```

> 解析  

```
// 输出
true
```

`NaN` 为不是数字的数字。虽然它不是数字,但是它也是数字类型。

---

### 第二十三题  

```
console.log(1 + "2" + "2");
console.log(1 + +"2" + "2");
console.log(1 + -"1" + "2");
console.log(+"1" + "1" + "2"); 
console.log( "A" - "B" + "2"); 
console.log( "A" - "B" + 2);
```

> 解析  

```
//输出
'122'
'32'
'02'
'112'
'NaN2'
NaN
```

首先要明白两点:  
> 1、`+a`,会把 `a` 转换为数字。`-a` 会把 `a` 转换成数字的负值(如果能转换为数字的话,否则为 `NaN` )。  
> 2、字符串与任何值相加都是字符串拼接。  

`console.log(1 + "2" + "2");` 简单的字符串拼接,即结果为: `'122'`。  

`console.log(1 + +"2" + "2");` 这里相当于 `console.log(1 + 2 + "2");` ,然后再字符串拼接。即结果为: `'32'`。  

`console.log(1 + -"1" + "2");` 这里相当于 `console.log(1 + -1 + "2");` ,然后再字符串拼接。即结果为: `'02'`。  

`console.log(+"1" + "1" + "2");` 这里相当于 `console.log(1 + "1" + "2");` ,然后再字符串拼接。即结果为: `'112'`。  

`console.log( "A" - "B" + "2");` ,由于 `'A' - 'B' = NaN`,所以相当于 `console.log( NaN + "2");` , 然后再字符串拼接。即结果为: `'NaN2'`。  

`console.log( "A" - "B" + 2);` 同上,相当于 `console.log(NaN + 2)` ,由于 `NaN+` 任何值还是 `NaN`,即结果为: `NaN`。  

---  

### 第二十四题  

```
var a = 666;
console.log(++a);
console.log(a++);
console.log(a);
```

> 解析  

```
// 输出
667
667
668
```

`++a` 先执行 `+1` 操作,再执行取值操作。 此时 `a` 的值为 `667`。因此输出 `667`。  

`a++` 先执行取值操作,再执行 `+1`。 此时输出 `667`,随后 `a` 的值变为 `668`。  

`--a` 和 `a--` 同理。  

使用这类运算符时要注意:  

> 1）这里的 `++`、`--` 不能用作于常量。比如  

```
 1++; // 抛出错误
```

> 2）如果 `a` 不是数字类型,会首先通过 `Number(a)`,将 `a` 转换为数字。再执行 `++` 等运算。

---  

### 第二十五题  

```
console.log(typeof a);
function a() {}
var a;
console.log(typeof a);
```

> 解析  

```
// 输出
'function'
'function'
```

跟第4题类似。函数会优先于变量声明提前。因此会忽略 `var a`。

---  

### 第二十六题  

```
var a;
var b = "undefined";
console.log(typeof a);
console.log(typeof b);
console.log(typeof c);
```

> 解析  

```
// 输出
'undefined'
'string'
'undefined'
```

`a` 为声明未赋值,默认为 `undefined`,`b` 的值为字符串 `'undefined'`, `c` 为未定义。  

`typeof` 一个未定义的变量时,不会抛出错误,会返回 `'undefined'`。注意 `typeof` 返回的都是字符串类型。

---  

### 第二十七题  

```
var a = 1;
if(function f() {}) {
  a += typeof f;
}
console.log(a);
```

> 解析  

```
// 输出
1undefined
```

`function f(){}` 当做 `if` 条件判断,其隐式转换后为 `true`。但是在 `()` 中的函数不会声明提升,因此 `f` 函数在外部是不存在的。因此 `typeof f = 'undefined'`,所以 `x += typeof f`,相当于 `x = x + 'undefined'` 为 `'1undefined'`。

---  

### 第二十八题  

```
var str = "123abc";
console.log(typeof str++);
```

> 解析  

```
// 输出
'number'
```

在24题解析时提到,使用 `++` 运算符时(无论是前置还是后置),如果变量不是数字类型,会首先用 `Number()` 转换为数字。因此 `typeof str++` 相当于 `typeof Number(str)++`。由于后置的 `++` 是先取值后计算,因此相当于 `typeof Number("123abc")`。即 `typeof NaN`,所以输出 `'number'`。

---  

### 第二十九题  

```
console.log('b' + 'a' + +'a'+'a');
```

> 解析  

```
// 输出
baNaNa
```

`'b' + 'a' + +'a'+'a'` 相当于 `'ba' + +'a'+'a',+'a'` 会将 `'a'` 转换为数字类型,即 `+'a' = NaN`。所以最终得到 `'ba' + NaN +'a'`,通过字符串拼接,结果为: `baNaNa`。  

---  

### 第三十题  

```
var obj = {n: 1};
function fn2(a) {
    a.n = 2;
}
fn2(obj);
console.log(obj.n);
```

> 解析  

```
// 输出
2
```

函数传递参数时,如果是基本类型为值传递,如果是引用类型,为引用地址的值传递。其实都是值传递。因此形参 `a` 和 `obj` 引用地址相同,都指向同一个对象。当执行 `a.n`,实际上共同指向的对象修改了,添加了个 `n` 属性,因此 `obj.n` 为 `2`。

---  

### 第三十一题  

```
var x = 10;
function fn() {
  console.log(x);
}
function show(f) {
  var x = 20;
  f();
}
show(fn);
```

> 解析  

```
// 输出
10
```

`JavaScript` 采用的是词法作用域,它规定了函数内访问变量时,查找变量是从函数声明的位置向外层作用域中查找,而不是从调用函数的位置开始向上查找。因此 `fn` 函数内部访问的 `x` 是全局作用域中的 `x`,而不是 `show` 函数作用域中的 `x`。

---  

### 第三十二题  

```
Object.prototype.bar = 1; 
var foo = {
    goo: undefined
};
console.log(foo.bar);
console.log('bar' in foo);
console.log(foo.hasOwnProperty('bar'));
console.log(foo.hasOwnProperty('goo'));
```

> 解析  

```
//输出
1
true
false
true
```

`in` 操作符:检测指定对象(右边)原型链上是否有对应的属性值。 `hasOwnProperty` 方法:检测指定对象自身上是否有对应的属性值。两者的区别在于 `in` 会查找原型链,而 `hasOwnProperty` 不会。  

示例中对象 `foo` 自身上存在 `goo` 属性,而它的原型链上存在 `bar` 属性。  

通过这个例子要注意如果要判断 `foo` 上是否有属性 `goo`,不能简单的通过 `if(foo.goo){}` 判断,因为 `goo` 的值可能为 `undefined` 或者其他可能隐式转换为 `false` 的值。

---  

### 第三十三题  

```
Object.prototype.bar = 1;
var foo = {
  moo: 2
}
for(var i in foo) {
  console.log(i);
}
```

> 解析  

```
// 输出
'moo'
'bar'
```

`for...in...` 遍历对象上除了 `Symbol` 以外的可枚举属性,包括原型链上的属性。

---  

### 第三十四题

```
function foo1() {
  return {
    bar: "hello"
  }
}
function foo2() {
  return
  {
    bar: "hello"
  }
}
console.log(foo1());
console.log(foo2());
```

>解析  

```
// 输出
{ bar: "hello" }
undefined
```

两个函数唯一区别就是 `return` 后面跟的值,一个换行一个不换行。  

当我们书写代码时忘记在结尾书写 `;` 时, `JavaScript` 解析器会根据一定规则自动补上 `;`。  

```
return
{
    bar: "hello"
}
=> 会被解析成
return;
{
    bar: "hello"
};
```

因此函数执行后会返回 `undefined`。

---  

### 第三十五题  

```
console.log((function() { return typeof arguments; })());
```

> 解析  

```
// 输出
'object'
```

`arguments` 为类数组,类型为 `object`。因此 `typeof arguments = 'object'`。  

---  

### 第三十六题

```
console.log(Boolean(false));
console.log(Boolean('0'));
console.log(Boolean(''));
console.log(Boolean(NaN));
```

> 解析  

```
// 输出
false
true
false
false
```

只有下面几种值在转换为布尔值时为 `false`:  

```
+0,-0,NaN,false,'',null,undefined
```

除此之外的值在转换为布尔值的时候全部为 `true`。

---  

### 第三十七题  

```
console.log(Array(3));
console.log(Array(2, 3));
```

> 解析  

```
// 输出
[empty × 3]
[2, 3]
```

使用 `Array()` 创建数组时,要注意传入的值的类型和数量。

---  

### 第三十八题  

```
console.log(0.1 + 0.2 == 0.3);
```

> 解析  

```
// 输出
false
```

---  

### 第三十九题

```
var a = [1, 2, 3];
console.log(a.join("-"));
```

> 解析  

```
// 输出
1-2-3
```

`join` 方法如果省略参数,默认以 `,` 分隔。

---  

### 第四十题  

```
var a = [3];
var b = [1];
console.log(a - b);
```

> 解析  

```
// 输出
2
```

在执行 `a - b` 时, `a` 和 `b` 都要转换为数字。首先 `a` 先转换为字符串,`[3] => [3].toString() => '3'`,然后 `Number(3) => 3`。`b` 同理。因此转换之后为 `3 - 1 = 2`。
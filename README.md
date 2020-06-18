
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


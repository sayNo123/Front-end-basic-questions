
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

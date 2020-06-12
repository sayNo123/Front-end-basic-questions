
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


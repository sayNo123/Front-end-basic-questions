
# 40道前端基础题，你能答对几道题？  

#### 第一题  

```
if (false) {
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
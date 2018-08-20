---
title: JavaScript闭包
date: 2018-04-25 09:42:00
categories:
- 前端
tags:
- JavaScript
- JavaScript闭包
---

### JavaScript闭包

看了两天的的JavaScript闭包，对于闭包有了些浅显的见解。

> 在函数外部调用函数内部的局部变量或者子函数

### 变量作用域

变量的作用域分为：**全局变量、局部变量**

在JavaScript中函数内部可以直接读取外部的全局变量

```javascript
var n = 10;
function f1(){
	console.log(n);
};
f1();  //10
```

但是在外部无法读取函数内部定义的局部变量

```javascript
function f1(){
	var n = 10;
};
f1();
console.log(n);  //n is not defined
```

### 如何在外部读取局部变量

在JavaScript中函数内部的子函数可以读取函数定义的局部变量

```javascript
function f1(){
	var n = 10;
	function f2(){
		console.log(n);  //10
	}
}
```

> 注：在函数内部声明局部变量不能直接写 n = 10，否则将创建一个全局变量

在上面👆的函数中函数f2是被包括在函数f1中的，f2可以读取f1中的局部变量，那么只要把f2作为f1的返回值，我们就可以在f1外部读取它的内部变量

```javascript
function f1(){
	var n = 10;
	function f2(){
		console.log(n);
	}
	return f2;
}
var result = f1();
result();  //10
```

### 闭包的概念

上面👆的函数f2就是一个闭包

f2能让f1中的局部变量n在f1外部被调用，这个就是闭包

> 或：函数f2在函数f1外部被调用，使函数f1始终存在内存中，不被回收机制销毁

由于在JavaScript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成“定义在一个函数内部的函数”

所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁

### 闭包的用途

闭包可以用在许多地方。它的最大用处有两个：一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中（如下👇）。

```javascript
function A() {
    var count = 0;
    function B() {
       count ++;
       console.log(count);
    }
    return B;
}
var C = A();
C();  // 1
C();  // 2
C();  // 3
```

count 是函数A 中的一个变量，它的值在函数B 中被改变，函数 B 每执行一次，count 的值就在原来的基础上累加 1 。因此，函数A中的 count 变量会一直保存在内存中。

当我们需要在模块中定义一些变量，并希望这些变量一直保存在内存中但又不会 “污染” 全局的变量时，就可以用闭包来定义这个模块。

### 注意事项

#### 命名冲突

当同一个闭包作用域下两个参数或者变量同名时，就会产生命名冲突。更近的作用域有更高的优先权，所以最近的优先级最高，最远的优先级最低。这就是作用域链。链的第一个元素就是最里面的作用域，最后一个元素便是最外层的作用域。

看以下的例子：

```javascript
function outside() {
  var x = 5;
  function inside(x) {
    return x * 2;
  }
  return inside;
}

outside()(10);  // 20
```

命名冲突发生在`return x`上，`inside`的参数`x`和`outside`变量`x`发生了冲突。这里的作用链域是{`inside`, `outside`, 全局对象}。因此`inside`的`x`具有最高优先权，返回了20（`inside`的`x`）而不是10（`outside`的`x`）。

#### 内存占用

由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。
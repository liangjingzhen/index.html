---
title: ES6以后各版本新特性
date: 2019-08-24 20:31:31
categories:
- JavaScript语言特性
tags:
- JavaScript
---

## ES2016

1. Exponentiation Operator（幂运算符）

#### 用法

```javascript
// x ** y

let squared = 2 ** 2;
// 等价于: 2 * 2

let cubed = 2 ** 3;
// 等价于: 2 * 2 * 2
```

也可以使用赋值运算符形式：
```javascript
// x **= y

let a = 2;
a **= 2;
// 等价于: a = a * a;

let b = 3;
b **= 3;
// 等价于: b = b * b * b;
```


2. Array.prototype.include

该方法用来判断一个数组中是否包含指定的值，如果包含则返回`true`，否则返回`false`。

#### 语法

> arr.includes(valueToFind[, fromIndex])

参数说明：
- `valueToFind` 要搜索的值。对于字符串和字符类型的输入是大小写敏感的。
- `fromIndex` 起始搜索位置，可选参数。默认值为0。



> 从技术上讲，`includes()`方法使用[`sameValueZero`](../JS内部的比较操作算法-SameValue、SameValueZero和SameValueNonNumber)算法来判断数组是否包含给定值。

#### 用法

```javascript
var array1 = [1, 2, 3];

console.log(array1.includes(2));
// > true

var pets = ['cat', 'dog', 'bat'];

console.log(pets.includes('cat'));
// > true

console.log(pets.includes('at'));
// > false
```

与`indexOf`方法不同的是，`includes`方法使用的是`SameValueZero`算法（前者使用的是`Strict Equality Comparison`算法），因此可以检查数组是否包含`NaN`，同样也不区分`+0`和`-0`。对于稀疏数组，它也不会跳过缺失元素，而是将其作为`undefined`处理。

```javascript
[NaN].includes(NaN)
> true
[NaN].indexOf(NaN)
> -1

[-0].includes(+0)
> true
```


## ES2017



## ES2018



## ES2019


### 参考资料
1. https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes
2. https://www.ecma-international.org/ecma-262/7.0/#sec-array.prototype.includes
3. https://github.com/tc39/proposal-exponentiation-operator
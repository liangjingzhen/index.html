---
title: JS内部的相等比较算法
date: 2019-08-24 22:58:44
categories:
- JavaScript语言特性
tags:
- JavaScript
---

ES标准规范中规定了三种用于比较两个值相等关系的算法：
- Abstract Equality Comparison (==)
- Strict Equality Comparison (===)
- SameValue (Object.is)
- SameValueZero (Array.prototype.includes/Map.prototype.has/Set.prototype.has)
- SameValueNonNumber

1. Abstract Equality Comparison

抽象相等比较算法，也是大家最熟悉的`==`运算。如果两个操作数的类型相同，则应用严格相等比较算法，否则，先进行类型转换，再对转换后的值进行比较。

2. Strict Equality Comparison

严格相等比较算法，类型不同的两个值不相等。如果两个操作数是数字类型，则直接比较两个数值是否相等；这里需要注意的是，`NaN`和`NaN`是不相等的。对于其他类型的操作数，则使用`SameValueNonNumber`来进行比较。
```javascript
NaN === NaN
> false
+0 === -0
> true
```
> 该算法与`SameValue`的区别在于对正负零和`NaN`的处理上。

3. SameValue

该算法与严格相等比较算法类似，主要区别在于对正负零和`NaN`的处理上。`SameValue`算法中，`NaN`与`NaN`是相等的，但`+0`与`-0`不相等。`Object.is()`方法使用了这个算法。
```javascript
Object.is(NaN, NaN)
> true
Object.is(+0, -0)
> false
```

4. SameValueZero

类似于`SameValue`，但该算法将`+0`与`-0`视为相等。`Array.prototype.includes()/Map.prototype.has()/Set.prototype.has()`等方法使用了这个算法。

```javascript
[NaN].includes(NaN)
> true

[+0].includes(-0)
> true

const s = new Set()
s.add(0)
s.add(NaN)
s.has(-0)
> true
s.has(NaN)
> true
```

5. SameValueNonNumber

在严格比较的情形下，对于任何非数值类型的两个值的比较，均使用此算法。


### 参考资料
1. [ECMAScript Language Specification](https://www.ecma-international.org/ecma-262/7.0/)

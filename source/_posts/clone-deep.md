---
title: clone deep
top: false
cover: false
toc: true
mathjax: true
date: 2021-10-16 21:53:30
password:
summary:
tags: 
    - 深浅拷贝
categories:
    - 深浅拷贝
---

**如何实现一个深拷贝函数（注意循环引用对象)**

#### 前置知识
**1.JS基本数据类型有: number、string、boolean、null、undefined、symbol(es6中新增的类型)**
**2.JS引用数据类型有: Object、Array、Function、Date、Regxp**
**3.基本数据类型（存放在栈中）**
**4.引用数据类型（存放在堆内存中)**
**5.typeof 的判断**
```
// 木易杨
typeof null //"object"
typeof {} //"object"
typeof [] //"object"
typeof function foo(){} //"function" (特殊情况)
```
#### 简单实现
**判断属性是否是对象，是的话，需要递归**
```
function cloneShallow(source) {
    var target = {};
    for (var key in source) {
        if (Object.prototype.hasOwnProperty.call(source, key)) {
            target[key] = source[key];
        }
    }
    return target;
}
```

**测试用例**
```
// 测试用例
var a = {
    name: "hmyjy",
    book: {
        title: "You Don't Know JS",
        price: "45"
    },
    a1: undefined,
    a2: null,
    a3: 123
}
var b = cloneShallow(a);

a.name = "高级前端进阶";
a.book.price = "55";

console.log(a, b);
```

```
function cloneDeep(source) {
    var target = {};
    for (var key in source) {
        if (Object.prototype.hasOwnProperty.call(source, key)) {
            console.log(typeof source[key])
            if (typeof source[key] === 'object') {
                target[key] = cloneDeep(source[key]); // 注意这里
            } else {
                target[key] = source[key];
            }
        }
    }
    return target;
}

// 使用上面测试用例测试一下
var b = cloneDeep(a);
```

**深拷贝--改进版**

```
// 木易杨
//把判断类型的函数提取出来，包含对数组 和  对象的判断 
function isObject(obj) {
    return typeof obj === 'object' && obj != null;
}

// 木易杨
function cloneDeep2(source) {

    if (!isObject(source)) return source; // 非对象返回自身

    var target = Array.isArray(source) ? [] : {};
    for (var key in source) {
        if (Object.prototype.hasOwnProperty.call(source, key)) {
            if (isObject(source[key])) {
                target[key] = cloneDeep2(source[key]); // 注意这里
            } else {
                target[key] = source[key];
            }
        }
    }
    return target;
}
```

**循环引用（自己引用自己）,示例：**

```
{
	name: "muyiy",
	a1: undefined,
	a2: null,
	a3: 123,
	book: {title: "You Don't Know JS", price: "45"},
	circleRef: {name: "muyiy", book: {…}, a1: undefined, a2: null, a3: 123, …}
}
```



#### 拷贝数组
#### 循环引用

**声明**
#### [参考链接](https://blog.csdn.net/qq_41846861/article/details/102296436)



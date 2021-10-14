---
description: 'Doc: https://github.com/mqyqingfeng/Blog/issues/6'
---

# this指针问题

## **作用域**链

作用域规定了如何查找变量，也就是确定当前执行代码对变量的访问权限。

查找变量时，会先从当前上线文的变量对象中查找，如果没有找到，就会从父级(词法层面上的父级)执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。

**由多个执行上下文的变量对象构成的链表就叫做作用域链。**

### **词法作用域与动态作用域**

词法作用域\*\*： \*\*函数的作用域在函数定义的时候决定 【JS】

动态作用域: 函数的作用域是在函数调用的时候决定 \[shell]

### **执行上下文栈**

\*\*可执行代码 (executable code): \*\*全局代码、函数代码、eval代码

当 JavaScript 开始要解释执行代码的时候，最先遇到的就是全局代码，所以初始化的时候首先就会向执行上下文栈压入一个全局执行上下文，我们用 globalContext 表示它，并且只有当整个应用程序结束的时候，ECStack 才会被清空，所以程序结束之前， ECStack 最底部永远有个 globalContext

对于每个执行上下文，都有三个重要属性：

* 变量对象(Variable object，VO)
* 作用域链(Scope chain)
* this

```javascript
var scope = "global scope";
function checkscope(){
    var scope2 = 'local scope';
    return scope2;
}
checkscope();
```

## this

[https://github.com/mqyqingfeng/Blog/issues/7](https://github.com/mqyqingfeng/Blog/issues/7)

### Reference

* base value
* reference name
* strict reference

```javascript
// var foo = 1;

var fooReference = {
    base: EnvironmentRecord,
    name: 'foo',
    strict: false
};

GetValue(fooReference) // 1;

```

GetValue 返回对象属性真正的值

1. GetBase   返回reference的base value
2. IsPropertyReference,  判断base value 是否是一个对象，
3. 判断ref 是不是Reference： 

```
如果ref 是Reference, 并且base value 是一个对象, 那么this 的值为GetBase(ref)
如果ref 是Reference, 并且base value 是Environment Record, 
那么this 指针的值为 ImImplicitThisValue(ref)
如果 ref 不是 Reference，那么 this 的值为 undefined
```





### MemberExpression



* PrimaryExpression // 原始表达式 可以参见《JavaScript权威指南第四章》
* FunctionExpression // 函数定义表达式
* MemberExpression \[ Expression ] // 属性访问表达式
* MemberExpression . IdentifierName // 属性访问表达式
* new MemberExpression Arguments // 对象创建表达式

GetValue，所以返回的值不是 Reference 类型，this 为 undefined，非严格模式下，this 的值为 undefined 的时候，其值会被隐式转换为全局对象

```javascript
function foo() {
    console.log(this)
}

foo(); 

```

MemberExpression 是 foo，解析标识符，查看规范 10.3.1 Identifier Resolution，会返回一个 Reference 类型的值：

```javascript
var fooReference = {
    base: EnvironmentRecord,
    name: 'foo',
    strict: false
};
```

接下来进行判断：

> 2.1 如果 ref 是 Reference，并且 IsPropertyReference(ref) 是 true, 那么 this 的值为 GetBase(ref)

因为 base value 是 EnvironmentRecord，并不是一个 Object 类型，还记得前面讲过的 base value 的取值可能吗？ 只可能是 undefined, an Object, a Boolean, a String, a Number, 和 an environment record 中的一种。

IsPropertyReference(ref) 的结果为 false，进入下个判断：

> 2.2 如果 ref 是 Reference，并且 base value 值是 Environment Record, 那么this的值为 ImplicitThisValue(ref)

base value 正是 Environment Record，所以会调用 ImplicitThisValue(ref)

查看规范 10.2.1.1.6，ImplicitThisValue 方法的介绍：该函数始终返回 undefined。

所以最后 this 的值就是 undefined。

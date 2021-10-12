---
description: 'Doc: https://github.com/mqyqingfeng/Blog/issues/6'
---

# this指针问题

## **作用域**链

作用域规定了如何查找变量，也就是确定当前执行代码对变量的访问权限。

查找变量时，会先从当前上线文的变量对象中查找，如果没有找到，就会从父级(词法层面上的父级)执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。

**由多个执行上下文的变量对象构成的链表就叫做作用域链。**

### **词法作用域与动态作用域**

词法作用域**： **函数的作用域在函数定义的时候决定  【JS】

动态作用域:   函数的作用域是在函数调用的时候决定  \[shell]

### **执行上下文栈**

**可执行代码 (executable code): **全局代码、函数代码、eval代码

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

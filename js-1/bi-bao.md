# 闭包



```
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f;
}

var foo = checkscope();
foo();
```

1. 进入全局代码，创建globalContext, 并压入Stack
2. global context 初始化
3. 执行checkscope函数，创建checkscope 函数执行上下文，并压入Stack
4. checkscope context 初始化，创建VO，作用域链，this 等
5. checkscope函数执行完毕，checkscope 执行context 从执行Stack中弹出
6. 执行f函数，创建f函数context, f context 压入Stack
7. f 执行context 初始化，创建VO, **作用率链(注意：闭包函获取了checkscopeContext.AO的值)**、this 等
8. f函数执行完毕，f context 从执行stack 中弹出

注意： 当f函数执行的时候，checkscope stack 已经从stack 中弹出（销毁）了，但是闭包函数f 维护了一个作用率链

```javascript
fContext = {
    Scope: [AO, checkscopeContext.AO, globalContext.VO]
}
```

闭包：

1. 即使创建它的context已经销毁，它依然存在
2. 在代码中引用了自由变量

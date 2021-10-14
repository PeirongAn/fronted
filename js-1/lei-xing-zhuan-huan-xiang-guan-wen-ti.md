---
description: https://github.com/mqyqingfeng/Blog/issues/159
---

# 类型转换相关问题

toString 方法

```javascript
console.log(({}).toString()) // [object Object]

console.log([].toString()) // ""
console.log([0].toString()) // 0
console.log([1, 2, 3].toString()) // 1,2,3
console.log((function(){var a = 1;}).toString()) // function (){var a = 1;}
console.log((/\d+/g).toString()) // /\d+/g
console.log((new Date(2010, 0, 1)).toString()) // Fri Jan 01 2010 00:00:00 GMT+0800 (CST)
```

JSON.stringfy

处理基本类型时，与使用toString基本相同，undefined 是undefined



```javascript
JSON.stringify({x: undefined, y: Object, z: Symbol("")}); 
// "{}"

JSON.stringify([undefined, Object, Symbol("")]);          
// "[null,null,null]" 
```

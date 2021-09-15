---
description: hooks
---

# react hooks

## 基础问题

1. useEffect 和useState

```jsx
$let count = 0;

useEffect(() => {
  // some logic
}, [count]); // Good!
let myObject = {
  prop: 'Value'
};

useEffect(() => {
  // some logic
}, [myObject]); // Not good!

useEffect(() => {
  // some logic
}, [myObject.prop]); // Good!
```

{% hint style="info" %}
 注意使用useEffect 的死循环问题
{% endhint %}

2  父子通信相关

2.1 useCallback

2.2 useImperativeHandle，父组件调用子组件方法，结合forwardRef使用

{% code title="待补充" %}
```jsx

```
{% endcode %}

TODO...

遗留问题

1. 下面是否可以达到预期结果？该如何修改

{% code title="useState 与 useRef 的区别" %}
```jsx
function MyComponent() {
  const [isFirst, setIsFirst] = useState(true);
  const [count, setCount] = useState(0);

  useEffect(() => {
    if (isFirst) {
      setIsFirst(false);
      return;
    }
    console.log('The counter increased!');
  }, [count]);

  return (
    <button onClick={() => setCount(count => count + 1)}>
      Increase
    </button>
  );
}
```
{% endcode %}

{% code title="useEffect 的一些限制" %}
```jsx
function WatchCount() {
  const [count, setCount] = useState(0);

  useEffect(function() {
    setInterval(function log() {
      console.log(`Count is: ${count}`);
    }, 2000);
  }, []);

  const handleClick = () => setCount(count => count + 1);

  return (
    <>
      <button onClick={handleClick}>Increase</button>
      <div>Counter: {count}</div>
    </>
  );
}
```
{% endcode %}

2 为什么？ Don’t call Hooks inside loops, conditions, or nested functions

{% code title="模拟 useState const \[xxx, setXXX\] = useState\(initVal\);" %}
```javascript
let state = [];
let setters = [];
//let firstRun = true;
let cursor = 0;

function createSetter(cursor) {
  return function setterWithCursor(newVal) {
    state[cursor] = newVal;
  };
}

// This is the pseudocode for the useState helper
export function useState(initVal) {
  if (firstRun) {
    state.push(initVal);
    setters.push(createSetter(cursor));
    //firstRun = false;
  }

  const setter = setters[cursor];
  const value = state[cursor];

  cursor++;
  return [value, setter];
}
```
{% endcode %}

{% hint style="info" %}
参考文档：[https://medium.com/@ryardley/react-hooks-not-magic-just-arrays-cd4f1857236e](https://medium.com/@ryardley/react-hooks-not-magic-just-arrays-cd4f1857236e)
{% endhint %}


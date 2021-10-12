---
description: hooks
---

# react hooks

## 基础问题

1. useEffect 和useState XXXX

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
    // 期望仅在按钮点击时才出现该值，然而不是，注意useState 是异步生效的
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

2 如何使用functional way 使得 state 生效， 注意setXXX 的返回值

```javascript

export function Error3() {
 
  const [content, setcontent] = useState<IRespData[]>([]);
  const inputRef = createRef<HTMLInputElement>();
  const [params, setParams] = useState<string[]>([]);
  
  const search = async () => {
    const resp = await getHolidy(params);
    const { code, data } = resp;

    if (code === 1) {
      setcontent(data);
    }
  }
  const clickHandler = () => {
      setParams((params) => {
       const input = inputRef.current?.value;
       const strarr = input ? input.split(',') : [];
       params.push(...strarr);
       return [];
      });
      search();
     
  };
  return (
    <>
      <span>
        <input placeholder="输入多个日期查询，YYYYMMDD, 英文逗号分割" style={{ marginLeft: 20, marginTop: 20, height: 30, width: 400} } ref={inputRef}></input>
        <button onClick={clickHandler}>search</button>
      </span>
      <ul>
      {content.map((c, index) => (<li key={index}>{`类型：${c.typeDes}, 星座：${c.constellation}, 农历：${c.lunarCalendar}`} </li>))}
      </ul>
    </>
  )


}
```

3 为什么？ Don’t call Hooks inside loops, conditions, or nested functions

以useState 为例， 初始化 -> 首次渲染 -> 后续渲染

初始化： setters 数组和 state 数组，外加一个cursor

首次渲染：将组件设置状态变量以cursor 为索引初始化，并设置初始化值

后续渲染：以cursor 为索引，更新对应状态变量

每个 setter 都有对其cursor位置的引用，调用任意`setter`它将对应更改状态数组中该位置的状态值。

{% code title="模拟 useState const [xxx, setXXX] = useState(initVal);" %}
```javascript
let state = [];
let setters = [];
let firstRun = true;
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
    firstRun = false;
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

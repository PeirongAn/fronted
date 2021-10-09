# 泛型

## 基本使用

示例

```typescript
const last = <T>(nums:T[]):t => {return nums[nums.length - 1];}
const updateTitle = <T extends { title: string }>(obj: T) =>{
    return {
        ...obj,
        title: obj.title,
    }
}

interface Tab<T> {
    id: string;
    position: number;
    data: T
}

type HomeTab = Tab<number>;
const homeTabL HomeTab = {
    id: 'home',
    position:1,
    data: 1,
}
```

{% hint style="info" %}
 Super-powers are granted randomly so please submit an issue if you're not happy with yours.
{% endhint %}

Once you're strong enough, save the world:

{% code title="hello.sh" %}
```bash
# Ain't no code for that yet, sorry
echo 'You got to trust me on this, I saved the world'
```
{% endcode %}




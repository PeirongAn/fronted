# interface vs type

## 使用 interface 

示例：

```typescript
interface Tut {
    title: string;
    isComplete: boolean;
}
interface VideoTut extends Tut {
    isPlay: boolean
}

const machineLearningTut: VideoTut {
    title: "machine learning",
    isComplete: true,
    isPlay: false,
}
```

同名类型自动合并

```typescript
// 允许同名的类型存在
interface Tut {
    title: string;
    isComplete: boolean
}
interface Tut {
    isOpen: boolean
}
// 继承时自动合并
const mlTut: Tut = {
    title: 'machine learning',
    isComplete: true,
}
```

使用TypeScript

```typescript
type Tut = {
    title: string,
    isComplete: boolean
} & {
    isPlay: boolean
}
const machineLearningTut: VideoTut {
    title: 'machine learning',
    isComplete: true,
    isPlay: false,
}
// 类型别名
type inputType = 'text' | 'email'
type inputOnchangeType = (newValue: string) => void
type InputProps = {
    type: inputType;
    value: string;
    onChange: inputOnchangeType
}

```

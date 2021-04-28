---
title: repeat
description: ''
position: 7000
category: '实用工具'
---

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/repeat.png)

```ts
repeat<T>(count: number = Infinity): MonoTypeOperatorFunction<T>
```

### 参数

| 参数  | 说明                       |
| ----- | -------------------------- |
| count | 可选，默认值为：`Infinity` |

### 返回

类型： `MonoTypeOperatorFunction<T>:`

<adsbygoogle></adsbygoogle>

## 示例

### 简单示例

```ts
import { of } from 'rxjs';
import { repeat } from 'rxjs/operators';

const source = of('Repeat message');
const example = source.pipe(repeat(3));
example.subscribe(x => console.log(x));

// 输出：
// Repeat message
// Repeat message
// Repeat message
```

### 组合示例

```ts
import { interval } from 'rxjs';
import { repeat, take } from 'rxjs/operators';

const source = interval(1000);
const example = source.pipe(take(3), repeat(2));
example.subscribe(x => console.log(x));

// 每秒输出：
// 0
// 1
// 2
// 0
// 1
// 2
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/repeat.ts>

---
title: take
description: ''
position: 5000
category: '过滤操作符'
---

<alert>

在完成前发出 N 个值(N 由参数决定)。

</alert>

## 说明

为什么使用 `take`？

当只对开头的一组值感兴趣时，你想要的便是 `take` 操作符。也许你想看看当用户第一次进入页面时，用户首先点击的是什么，你想要订阅点击事件并只取首个值。举例来说，你想要观看赛跑，但其实你只对首先冲过终点的人感兴趣。此操作符很清晰明了，你想要取开头 n 个值。

- 如果想基于某个逻辑或另一个 Observable 来取任意数量的值，你可以用 `takeUntil` 或 `takeWhile`！
- `take` 与 `skip` 是相反的，它接收起始的 N 个值，而 `skip` 会跳过起始的 N 个值。

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/take.png)

```ts
take<T>(count: number): MonoTypeOperatorFunction<T>
```

### 参数

| 参数  | 说明           |
| ----- | -------------- |
| count | 最大投射次数。 |

### 返回

类型： `MonoTypeOperatorFunction<T>`

## 示例

```ts
import { interval } from 'rxjs';
import { take } from 'rxjs/operators';

const intervalCount = interval(1000);
const takeFive = intervalCount.pipe(take(5));
takeFive.subscribe(x => console.log(x));

// 输出:
// 0
// 1
// 2
// 3
// 4
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/take.ts>

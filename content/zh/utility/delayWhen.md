---
title: delayWhen
description: ''
position: 7000
category: '实用工具'
---

<alert>

延迟投射值，延迟时间由提供函数决定。

</alert>

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/delayWhen.png)

```ts
delayWhen<T>(
  delayDurationSelector: (value: T, index: number) => Observable<any>,
  subscriptionDelay?: Observable<any>
): MonoTypeOperatorFunction<T>
```

### 参数

| 参数                  | 说明                                  |
| --------------------- | ------------------------------------- |
| delayDurationSelector | 延迟选择器。                          |
| subscriptionDelay     | 可选。默认值：`undefined`。触发订阅。 |

### 返回

类型： `MonoTypeOperatorFunction<T>`

<adsbygoogle></adsbygoogle>

## 示例

```ts
import { interval, timer } from 'rxjs';
import { delayWhen } from 'rxjs/operators';

// 每1秒投射值
const message = interval(1000);
// 5秒后投射值
const delayForFiveSeconds = () => timer(5000);
// 5秒后，开始投射 interval 延迟的值
const delayWhenExample = message.pipe(delayWhen(delayForFiveSeconds));
// 延迟5秒后输出值
// 例如， 输出: 0...1...2...3
const subscribe = delayWhenExample.subscribe(val => console.log(val));
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/delayWhen.ts>

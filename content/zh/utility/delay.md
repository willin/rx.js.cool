---
title: delay
description: ''
position: 7000
category: '实用工具'
---

<alert>

根据给定时间延迟投射值。

</alert>

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/delay.png)

```ts
delay<T>(
  delay: number | Date,
  scheduler: SchedulerLike = async
): MonoTypeOperatorFunction<T>
```

### 参数

| 参数      | 说明                  |
| --------- | --------------------- |
| delay     | 单位：毫秒。          |
| scheduler | 可选。默认值：`async` |

### 返回

类型： `MonoTypeOperatorFunction<T>`

## 示例

```ts
import { of, merge } from 'rxjs';
import { mapTo, delay } from 'rxjs/operators';

const example = of(null);
// 每延迟一次输出便增加1秒延迟时间
const message = merge(
  example.pipe(mapTo('Hello')),
  example.pipe(mapTo('World!'), delay(1000)),
  example.pipe(mapTo('Goodbye'), delay(2000)),
  example.pipe(mapTo('World!'), delay(3000))
);
// 输出: 'Hello'...'World!'...'Goodbye'...'World!'
const subscribe = message.subscribe(val => console.log(val));
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/delay.ts>

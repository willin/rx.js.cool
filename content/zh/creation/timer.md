---
title: timer
description: ''
position: 1001
category: '创建操作符'
---

<alert>

给定持续时间后，再按照指定间隔时间依次投射数字。

</alert>

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/timer.png)

```ts
timer(
  dueTime: number | Date = 0,
  periodOrScheduler?: number | SchedulerLike,
  scheduler?: SchedulerLike
): Observable<number>
```

### 参数

| 参数              | 说明                                                |
| ----------------- | --------------------------------------------------- |
| dueTime           | 可选。默认值：`0`。单位：毫秒。初始延迟时间。       |
| periodOrScheduler | 可选。默认值：`undefined`。后续序列投射的时间间隔。 |
| scheduler         | 可选。默认值：`undefined`。                         |

### 返回

类型： `Observable<T>`

## 示例

### 当 timer 结束时投射一个值

```ts
import { timer } from 'rxjs';

// 1秒后投射0，然后结束，因为没有提供第二个参数
const source = timer(1000);
// 输出: 0
const subscribe = source.subscribe(val => console.log(val));
```

### timer 1 秒后投射值，然后每 2 秒投射值

```ts
import { timer } from 'rxjs';

/*
  timer 接收第二个参数，它决定了投射序列值的频率，在本例中我们在1秒投射第一个值，
  然后每2秒投射序列值
*/
const source = timer(1000, 2000);
// 输出: 0,1,2,3,4,5......
const subscribe = source.subscribe(val => console.log(val));
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/observable/timer.ts>

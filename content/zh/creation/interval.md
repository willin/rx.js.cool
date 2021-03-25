---
title: interval
description: ''
position: 1001
category: '创建操作符'
---

<alert>

基于给定时间间隔投射数字序列。

</alert>

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/interval.png)

```ts
interval(period: number = 0, scheduler: SchedulerLike = async): Observable<number>
```

### 参数

| 参数      | 说明                            |
| --------- | ------------------------------- |
| period    | 可选。默认值：`0`。单位：毫秒。 |
| scheduler | 可选。默认值：`async`           |

### 返回

类型： `Observable<T>`

## 示例

```ts
import { interval } from 'rxjs';

// 每1秒投射数字序列中的值
const source = interval(1000);
// 数字: 0,1,2,3,4,5....
const subscribe = source.subscribe(val => console.log(val));
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/observable/interval.ts>

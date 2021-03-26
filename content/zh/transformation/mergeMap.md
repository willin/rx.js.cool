---
title: mergeMap
description: ''
position: 6000
category: '转化操作符'
---

<alert>

映射成 Observable 序列并投射值。

</alert>

## 说明

- `flatMap` 是 `mergeMap` 的别名！
- 如果同一时间应该只有一个内部 subscription 是有效的，请尝试 `switchMap`！
- 如果内部 observables 发送和订阅的顺序很重要，请尝试 `concatMap`！

为什么使用 `mergeMap`？

当想要打平内部 Observable 并手动控制内部订阅数量时，此操作符是最适合的。

例如，当使用 `switchMap` 时，源 Observable 投射值时，每个内部订阅都是完成的，只允许存在一个活动的内部订阅。与此相反，`mergeMap` 允许同一时间存在多个活动的内部订阅。正因为如此，`mergeMap` 最常见的用例便是不会被取消的请求，可以将其考虑成写，而不是读。注意如果需要考虑顺序的话， concatMap 会是更好的选择。

注意，因为 `mergeMap` 同时维护多个活动的内部订阅，由于这些长期活动的内部订阅，所以是有可能产生内存泄露的。举个例子，如果你将 Observable 映射成内部的定时器或 DOM 事件流。在这些案例中，如果你仍然想用 `mergeMap` 的话，你应该利用另一个操作符来管理内部订阅的完成，比如 take 或 takeUntil。你还可以使用 concurrent 参数来限制活动的内部订阅的数量。

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/mergeMap.png)

```ts
mergeMap<T, R, O extends ObservableInput<any>>(
  project: (value: T, index: number) => O,
  resultSelector?: number | ((outerValue: T, innerValue: ObservedValueOf<O>, outerIndex: number, innerIndex: number) => R),
  concurrent: number = Number.POSITIVE_INFINITY
): OperatorFunction<T, ObservedValueOf<O> | R>

```

### 参数

| 参数           | 说明                                                            |
| -------------- | --------------------------------------------------------------- |
| project        | `value`：源 Observable 投射的每个值。 `index`： 从 0 开始的索引 |
| resultSelector | 可选。默认值：`undefined`。                                     |
| concurrent     | 可选。默认值：`Number.POSITIVE_INFINITY`。最大并发订阅数量。    |

### 返回

类型： `OperatorFunction<T, R>`：从源 Observable 投射值执行 `project` 函数转换后的 Observable 序列。

## 示例

### 遍历，打平

```ts
import { of, interval } from 'rxjs';
import { mergeMap, map } from 'rxjs/operators';

const letters = of('a', 'b', 'c');
const result = letters.pipe(mergeMap(x => interval(1000).pipe(map(i => x + i))));
result.subscribe(x => console.log(x));
// 输出：
// a0
// b0
// c0
// a1
// b1
// c1
// .....
```

### 与 Promise 配合使用

```ts
import { of } from 'rxjs';
import { mergeMap } from 'rxjs/operators';

// 创建 Promise
const myPromise = val => new Promise(resolve => resolve(`${val} World From Promise!`));

const source$ = of('Hello');

source$.pipe(mergeMap(val => myPromise(val))).subscribe(val => console.log(val));
// 输出：
// Hello World From Promise!
```

### 使用 `resultSelector`

```ts
import { of } from 'rxjs';
import { mergeMap } from 'rxjs/operators';

const myPromise = val => new Promise(resolve => resolve(`${val} World From Promise!`));
const source$ = of('Hello');

source$
  .pipe(
    mergeMap(
      val => myPromise(val),
      (valueFromSource, valueFromPromise) => {
        return `Source: ${valueFromSource}, Promise: ${valueFromPromise}`;
      }
    )
  )
  .subscribe(val => console.log(val));
// 输出：
// Source: Hello, Promise: Hello World From Promise!
```

### 设置最大并发数

```ts
import { interval } from 'rxjs';
import { mergeMap, take } from 'rxjs/operators';

// emit value every 1s
const source$ = interval(1000);

source$
  .pipe(
    mergeMap(
      // project
      val => interval(5000).pipe(take(2)),
      // resultSelector
      (oVal, iVal, oIndex, iIndex) => [oIndex, oVal, iIndex, iVal],
      // concurrent
      2
    )
  )
  .subscribe(val => console.log(val));
// 输出：
// [0, 0, 0, 0]
// [1, 1, 0, 0]
// [0, 0, 1, 1]
// [1, 1, 1, 1]
// [2, 2, 0, 0]
// [3, 3, 0, 0]
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/mergeMap.ts>

---
title: concatMap
description: ''
position: 6000
category: '转化操作符'
---

<alert>

将源值投射为一个 Observable 序列，其内部以串行的方式等待前一个完成再合并下一个 Observable。

</alert>

## 说明

<ruby>遍历<rp>（</rp><rt>Map</rt><rp>）</rp></ruby> 每个值为 Observable 序列，然后使用 `concatAll` 展平所有 <ruby>内部<rp>（</rp><rt> Inner</rt><rp>）</rp></ruby> Observable 序列。

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/concatMap.png)

```ts
concatMap<T, R, O extends ObservableInput<any>>(
  project: (value: T, index: number) => O,
  resultSelector?: (outerValue: T, innerValue: ObservedValueOf<O>, outerIndex: number, innerIndex: number) => R
): OperatorFunction<T, ObservedValueOf<O> | R>nction<T, R>
```

### 参数

| 参数           | 说明                                                                                                                          |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| project        | `value`：源 Observable 投射的每个值。 `index`： 从 0 开始的索引                                                               |
| resultSelector | 可选。默认值：`undefined`。类型： `(outerValue: T, innerValue: ObservedValueOf, outerIndex: number, innerIndex: number) => R` |

### 返回

类型： `OperatorFunction<T, ObservedValueOf<O> | R>`

## 示例

### 与 `mergeMap` 区别比较

```ts
import { of } from 'rxjs';
import { concatMap, delay, mergeMap } from 'rxjs/operators';

const source = of(2000, 1000);
// 顺序执行，类似于 Promise.parallels
source
  .pipe(
    // concatMap
    concatMap(val => of(`Delayed by: ${val}ms`).pipe(delay(val)))
  )
  .subscribe(val => console.log(`With concatMap: ${val}`));

// 对比 concatMap 和 mergeMap 的区别
source
  .pipe(
    // mergeMap 延迟 5000ms 执行
    delay(5000),
    mergeMap(val => of(`Delayed by: ${val}ms`).pipe(delay(val)))
  )
  .subscribe(val => console.log(`With mergeMap: ${val}`));
// 输出：
// With concatMap: Delayed by: 2000ms
// With concatMap: Delayed by: 1000ms
// With mergeMap: Delayed by: 1000ms
// With mergeMap: Delayed by: 2000ms
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/concatMap.ts>

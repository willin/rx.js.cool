---
title: combineAll
description: ''
position: 2000
category: '组合操作符'
---

<alert>

即 `combineLatestAll`。当源 Observable 完成时，使用 [combineLatest](/combination/combineLatest)。

</alert>

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/combineAll.png)

```ts
combineAll<T, R>(
  project?: (...values: any[]) => R
): OperatorFunction<T, R>
```

### 参数

| 参数    | 说明                                                       |
| ------- | ---------------------------------------------------------- |
| project | 可选。默认值：`undefined`。 类型 `(...values: any[]) => R` |

### 返回

类型： `OperatorFunction<T, R>`

<adsbygoogle></adsbygoogle>

## 示例

### 投射成内部的 interval observable

```ts
import { take, map, combineAll } from 'rxjs/operators';
import { interval } from 'rxjs';

// 每秒发出值，并只取前2个
const source = interval(1000).pipe(take(2));
// 将 source 发出的每个值映射成取前5个值的 interval observable
const example = source.pipe(
  map(val =>
    interval(1000).pipe(
      map(i => `Result (${val}): ${i}`),
      take(5)
    )
  )
);
/*
  soure 中的2个值会被映射成2个(内部的) interval observables，
  这2个内部 observables 每秒使用 combineLatest 策略来 combineAll，
  每当任意一个内部 observable 发出值，就会发出每个内部 observable 的最新值。
*/
const combined = example.pipe(combineAll());
combined.subscribe(val => console.log(val));
/*
  输出:
  ["Result (0): 0", "Result (1): 0"]
  ["Result (0): 1", "Result (1): 0"]
  ["Result (0): 1", "Result (1): 1"]
  ["Result (0): 2", "Result (1): 1"]
  ["Result (0): 2", "Result (1): 2"]
  ["Result (0): 3", "Result (1): 2"]
  ["Result (0): 3", "Result (1): 3"]
  ["Result (0): 4", "Result (1): 3"]
  ["Result (0): 4", "Result (1): 4"]
*/
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/combineLatestAll.ts>

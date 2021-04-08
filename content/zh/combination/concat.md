---
title: concat
description: ''
position: 2000
category: '组合操作符'
---

<alert>

按照顺序，前一个 Observable 完成了再订阅下一个 Observable 并投射值。

</alert>

## 说明

- 你可以把 concat 想象成 ATM 机前的长队，下一次交易 (subscription) 不能在前一个交易完成前开始！
- 此操作符可以既有静态方法，又有实例方法！
- 如果生产量是首要考虑的，而不需要关心产生值的顺序，那么试试用 [merge](/combination/merge) 来代替！

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/concat.png)

```ts
concat<O extends ObservableInput<any>, R>(
  ...observables: (SchedulerLike | O)[]
): Observable<ObservedValueOf<O> | R>
```

### 参数

| 参数        | 说明            |
| ----------- | --------------- |
| observables | Observable 数组 |

### 返回

类型： `Observable<ObservedValueOf<O> | R>`

<adsbygoogle></adsbygoogle>

## 示例

### concat 2 个基础的 Observables

```ts
import { concat } from 'rxjs/operators';
import { of } from 'rxjs';

// 投射 1,2,3
const sourceOne = of(1, 2, 3);
// 投射 4,5,6
const sourceTwo = of(4, 5, 6);
const example = sourceOne.pipe(
  // 先投射 sourceOne 的值，当完成时订阅 sourceTwo
  concat(sourceTwo)
);

example.subscribe(val => console.log('Example: Basic concat:', val));
// 输出: 1,2,3,4,5,6
```

### concat 作为静态方法

```ts
import { of, concat } from 'rxjs';

// 投射 1,2,3
const sourceOne = of(1, 2, 3);
// 投射 4,5,6
const sourceTwo = of(4, 5, 6);

// 作为静态方法使用
const example = concat(sourceOne, sourceTwo);
example.subscribe(val => console.log(val));
// 输出: 1,2,3,4,5,6
```

### 使用延迟的 souce observable 进行 concat

```ts
import { delay, concat } from 'rxjs/operators';
import { of } from 'rxjs';

// 投射 1,2,3
const sourceOne = of(1, 2, 3);
// 投射 4,5,6
const sourceTwo = of(4, 5, 6);

// 延迟3秒，然后投射
const sourceThree = sourceOne.pipe(delay(3000));
const example = sourceThree.pipe(
  // sourceTwo 要等待 sourceOne 完成才能订阅
  concat(sourceTwo)
);
example.subscribe(val => console.log('Example: Delayed source one:', val));
// 输出: 1,2,3,4,5,6
```

### 使用不完成的 source observable 进行 concat

```ts
import { interval, of, concat } from 'rxjs';

// 当 source 永远不完成时，随后的 observables 永远不会运行
const source = concat(interval(1000), of('This', 'Never', 'Runs'));
// 输出: 0,1,2,3,4....
const subscribe = source.subscribe(val =>
  console.log(
    'Example: Source never completes, second observable never runs:',
    val
  )
source.subscribe(val => console.log('Example: Source never completes, second observable never runs:', val));
// 输出: 0,1,2,3,4....
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/concat.ts>

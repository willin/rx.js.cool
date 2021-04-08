---
title: combineLatest
description: ''
position: 2000
category: '组合操作符'
---

<alert>

当任意 Observable 投射值时，投射每个 Observable 的最新值。

</alert>

## 说明

如果你只需要 Observable 序列投射一个值，或只需要它们完成前的最新值时，[forkJoin](/combination/forkJoin) 会是更好的选择。

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/combineLatest.png)

```ts
combineLatest<O extends ObservableInput<any>, R>(
  ...observables: (SchedulerLike | O | ((...values: ObservedValueOf<O>[]) => R))[]
): Observable<R>
```

### 参数

| 参数        | 说明                                                                   |
| ----------- | ---------------------------------------------------------------------- |
| observables | 类型 `(SchedulerLike \| O \| ((...values: ObservedValueOf[]) => R))[]` |

### 返回

类型： `OperatorFunction<T, R>`

<adsbygoogle></adsbygoogle>

## 示例

### 组合 3 个定时发送的 observables

```ts
import { timer, combineLatest } from 'rxjs';

// timerOne 在1秒时投射第一个值，然后每4秒发送一次
const timerOne = timer(1000, 4000);
// timerTwo 在2秒时投射第一个值，然后每4秒发送一次
const timerTwo = timer(2000, 4000);
// timerThree 在3秒时投射第一个值，然后每4秒发送一次
const timerThree = timer(3000, 4000);

// 当一个 timer 投射值时，将每个 timer 的最新值作为一个数组投射
const combined = combineLatest(timerOne, timerTwo, timerThree);

combined.subscribe(latestValues => {
  // 从 timerValOne、timerValTwo 和 timerValThree 中获取最新投射的值
  const [timerValOne, timerValTwo, timerValThree] = latestValues;
  /*
    示例:
    timerOne first tick: 'Timer One Latest: 1, Timer Two Latest:0, Timer Three Latest: 0
    timerTwo first tick: 'Timer One Latest: 1, Timer Two Latest:1, Timer Three Latest: 0
    timerThree first tick: 'Timer One Latest: 1, Timer Two Latest:1, Timer Three Latest: 1
  */
  console.log(
    `Timer One Latest: ${timerValOne},
     Timer Two Latest: ${timerValTwo},
     Timer Three Latest: ${timerValThree}`
  );
});
```

### 使用 projection 函数的 combineLatest

```ts
import { timer, combineLatest } from 'rxjs';

// timerOne 在1秒时投射第一个值，然后每4秒发送一次
const timerOne = timer(1000, 4000);
// timerTwo 在2秒时投射第一个值，然后每4秒发送一次
const timerTwo = timer(2000, 4000);
// timerThree 在3秒时投射第一个值，然后每4秒发送一次
const timerThree = timer(3000, 4000);

// combineLatest 还接收一个可选的 projection 函数
const combinedProject = combineLatest(timerOne, timerTwo, timerThree, (one, two, three) => {
  return `Timer One (Proj) Latest: ${one},
              Timer Two (Proj) Latest: ${two},
              Timer Three (Proj) Latest: ${three}`;
});
// 输出值
combinedProject.subscribe(latestValuesProject => console.log(latestValuesProject));
```

<!--
### 组合 2 个按钮事件

```ts
import { fromEvent, combineLatest } from 'rxjs';
import { mapTo, startWith, scan, tap, map } from 'rxjs/operators';

// 用来设置 HTML 的辅助函数
const setHtml = id => val => (document.getElementById(id).innerHTML = val);

const addOneClick$ = id =>
  fromEvent(document.getElementById(id), 'click').pipe(
    // 将每次点击映射成1
    mapTo(1),
    startWith(0),
    // 追踪运行中的总数
    scan((acc, curr) => acc + curr),
    // 为适当的元素设置 HTML
    tap(setHtml(`${id}Total`))
  );

const combineTotal$ = combineLatest(addOneClick$('red'), addOneClick$('black'))
  .pipe(map(([val1, val2]) => val1 + val2))
  .subscribe(setHtml('total'));
```
-->

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/combineLatest.ts>

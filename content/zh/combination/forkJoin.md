---
title: forkJoin
description: ''
position: 2000
category: '组合操作符'
---

<alert>

当所有 Observables 完成时，投射每个 Observable 的最新值。

</alert>

## 说明

- 如果你想要多个 Observables 按投射顺序相对应的值的组合，试试 [zip](/combination/zip)！
- 如果内部 Observable 不完成的话，forkJoin 永远不会投射值！

为什么使用 `forkJoin`？

当有一组 Observables，但你只关心每个 Observable 最后投射的值时，此操作符是最适合的。此操作符的一个常见用例是在页面加载(或其他事件)时你希望发起多个请求，并在所有请求都响应后再采取行动。它可能与 `Promise.all` 的使用方式类似。

注意，如果任意作用于 `forkJoin` 的内部 Observable 报错的话，对于那些在内部 Observable 上没有正确 `catch` 错误，从而导致完成的 Observable，你将丢失它们的值。如果你只关心所有内部 Observables 是否成功完成的话，可以在外部捕获错误。

还需要注意的是如果 Observable 投射的项多于一个的话，并且你只关心前一个投射的话，那么 `forkJoin` 并非正确的选择。在这种情况下，应该选择像 `combineLatest` 或 `zip` 这样的操作符。

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/forkJoin.png)

```ts
forkJoin(...sources: any[]): Observable<any>
```

### 参数

| 参数    | 说明                                   |
| ------- | -------------------------------------- |
| sources | 以数组或者参数形式传进 Observable 序列 |

### 返回

类型： `Observable<any>`

<adsbygoogle></adsbygoogle>

## 示例

### 简单示例

```ts
import { delay, take } from 'rxjs/operators';
import { forkJoin, of, interval } from 'rxjs';

const myPromise = val => new Promise(resolve => setTimeout(() => resolve(`Promise Resolved: ${val}`), 5000));

/*
  当所有 observables 完成时，将每个 observable
  的最新值作为数组投射
*/
const example = forkJoin(
  // 立即投射 'Hello'
  of('Hello'),
  // 1秒后投射 'World'
  of('World').pipe(delay(1000)),
  // 1秒后投射0
  interval(1000).pipe(take(1)),
  // 以1秒的时间间隔投射0和1
  interval(1000).pipe(take(2)),
  // 5秒后解析 'Promise Resolved' 的 promise
  myPromise('RESULT')
);

example.subscribe(val => console.log(val));
// 输出:
// ["Hello", "World", 0, 1, "Promise Resolved: RESULT"]
```

### Ajax 请求

```ts
import { ajax } from 'rxjs/ajax';
import { forkJoin } from 'rxjs';

forkJoin({
  google: ajax.getJSON('https://api.github.com/users/google'),
  microsoft: ajax.getJSON('https://api.github.com/users/microsoft'),
  users: ajax.getJSON('https://api.github.com/users')
}).subscribe(console.log);
// 输出：
// { google: object, microsoft: object, users: array }
```

### 在外部处理错误

```ts
import { delay, catchError } from 'rxjs/operators';
import { forkJoin, of, throwError } from 'rxjs';

const example = forkJoin(
  // 立即投射 'Hello'
  of('Hello'),
  // 1秒后投射 'World'
  of('World').pipe(delay(1000)),
  // 抛出错误
  _throw('This will error')
).pipe(catchError(error => of(error)));

example.subscribe(val => console.log(val));
// 输出:
// 'This will Error'
```

### 当某个内部 Observable 报错时得到成功结果

```ts
import { delay, catchError } from 'rxjs/operators';
import { forkJoin, of, throwError } from 'rxjs';

const example = forkJoin(
  of('Hello'),
  of('World').pipe(delay(1000)),
  // 抛出错误
  _throw('This will error').pipe(catchError(error => of(error)))
);

example.subscribe(val => console.log(val));
// 输出:
// ["Hello", "World", "This will error"]
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/observable/forkJoin.ts>

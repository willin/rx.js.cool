---
title: iif
description: ''
position: 3000
category: '判断操作符'
---

<alert>

判断决定订阅的 Observable 对象。

</alert>

## 用法

```ts
iif<T = never, F = never>(
  condition: () => boolean,
  trueResult: SubscribableOrPromise<T> = EMPTY,
  falseResult: SubscribableOrPromise<F> = EMPTY
): Observable<T | F>
```

### 参数

| 参数        | 说明                                                    |
| ----------- | ------------------------------------------------------- |
| condition   | 条件                                                    |
| trueResult  | 可选，默认值： `EMPTY`。 类型： `SubscribableOrPromise` |
| falseResult | 可选，默认值： `EMPTY`。 类型： `SubscribableOrPromise` |

### 返回

类型： `Observable<T>`

## 示例

### 简单示例

```ts
import { iif, of, interval } from 'rxjs';
import { mergeMap } from 'rxjs/operators';

const r$ = of('R');
const x$ = of('X');

interval(1000)
  .pipe(mergeMap(v => iif(() => v % 4 === 0, r$, x$)))
  .subscribe(console.log);
// 输出：
// R ... X ... X ... X ... R ... X ... X ... X ... R ...
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/observable/iif.ts>

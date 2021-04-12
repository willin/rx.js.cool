---
title: delayRetry
description: ''
position: 8800
category: '进阶'
---

## 示例

示例代码：

```ts
import { of } from 'rxjs';
import { tap, map } from 'rxjs/operators';
import { tapAsync } from 'v0';

const s$ = of(1, 2, 3, 4, 5, 6).pipe(
  tapAsync(async x => {
    await Promise.resolve(x);
    console.log('tapAsync: origin', x);
  }),
  map(x => x ** 2),
  tap(x => console.log('tapAsync: calc', x))
);

s$.subscribe({
  complete() {
    console.log('tapAsync: ended');
  }
});
```

输出：

```
origin 1
origin 2
origin 3
origin 4
origin 5
origin 6
calc 1
calc 4
calc 9
calc 16
calc 25
calc 36
ended
```

## 可运行示例

示例运行：[https://stackblitz.com/edit/rxjs-v0](https://stackblitz.com/edit/rxjs-v0?devtoolsheight=100&file=operators/delayRetry.ts)

修改 `index.ts` 注释掉不需要演示的 operator 引入。

## 实现

使用 `mergeMap` 实现：

```ts
import { OperatorFunction } from 'rxjs';
import { mergeMap } from 'rxjs/operators';

/**
 * tapAsync
 * @param fn Async Function
 *
 * const s$ = of(1, 2, 3, 4, 5, 6).pipe(
 * tapAsync(async x => {
 *   await Promise.resolve(x);
 *   console.log(x);
 * }),
 * map(x => x ** 2),
 * tap(x => console.log("calced", x))
 * );
 */
export function tapAsync<T>(fn: (x: T) => Promise<void | void | never>): OperatorFunction<T, T> {
  return mergeMap(async (x: T) => {
    await fn(x);
    return x;
  });
}
```

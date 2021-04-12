---
title: tapAsync
description: ''
position: 8800
category: '进阶'
---

## 示例

示例代码：

```ts
import { range, of, throwError } from 'rxjs';
import { delayRetry } from 'v0';
import { mergeMap, catchError } from 'rxjs/operators';

const s$ = range(1, 5).pipe(
  mergeMap(val => {
    if (val > 3) {
      return throwError(`Errored: ${val}`);
    }
    return of(val);
  }),
  delayRetry({
    maxAttempts: 2, //重试两次，加原本执行一次，最多执行 3 次。
    duration: 200
  }),
  catchError(error => of(error))
);

s$.subscribe({
  next(v) {
    console.log('delayRetry', v);
  },
  complete() {
    console.log('delayRetry ended');
  }
});
```

输出：

```
1
2
3
1
2
3
1
2
3
Errored: 4
ended
```

## 可运行示例

示例运行：[https://stackblitz.com/edit/rxjs-v0](https://stackblitz.com/edit/rxjs-v0?devtoolsheight=100&file=operators/delayRetry.ts)

修改 `index.ts` 注释掉不需要演示的 operator 引入。

## 实现

使用 `retryWhen`、`mergeMap`、`timer` 实现：

```ts
import { MonoTypeOperatorFunction, Observable, throwError, timer } from 'rxjs';
import { mergeMap, retryWhen } from 'rxjs/operators';

export function delayRetry<T>({
  maxAttempts = 3,
  duration = 1000
}: {
  maxAttempts?: number;
  duration?: number;
} = {}): MonoTypeOperatorFunction<T> {
  return retryWhen<T>(
    (attempts: Observable<unknown>): Observable<unknown> =>
      attempts.pipe(
        mergeMap((error, i) => {
          const retryAttempt = i + 1;
          // 如果已经达到最大尝试次数
          if (retryAttempt > maxAttempts) {
            return throwError(error);
          }
          // 1s, 2s, ... 后重试
          return timer(retryAttempt * duration);
        })
      )
  );
}
```

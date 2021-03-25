---
title: retryWhen
description: ''
position: 4000
category: ' 异常处理'
---

<alert>

当发生错误时，基于自定义的标准来重试 Observable 序列。

</alert>

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/retryWhen.png)

```ts
retryWhen<T>(
  notifier: (errors: Observable<any>) => Observable<any>
): MonoTypeOperatorFunction<T>
```

### 参数

| 参数     | 说明                                                           |
| -------- | -------------------------------------------------------------- |
| notifier | 接受一个 Observable 对象，用户可以指定完成、出错或者取消重试。 |

### 返回

类型： `MonoTypeOperatorFunction<T>`

## 示例

### 基础示例

```ts
import { timer, interval } from 'rxjs';
import { map, tap, retryWhen, delayWhen } from 'rxjs/operators';

const source = interval(200);
const example = source.pipe(
  map(val => {
    if (val > 5) {
      // 抛出的异常会被 retryWhen 接收
      throw val;
    }
    return val;
  }),
  retryWhen(errors =>
    errors.pipe(
      tap(val => console.log(`Value ${val} was too high!`)),
      delayWhen(val => timer(1000))
    )
  )
);
example.subscribe(val => console.log(val));
/*
  输出:
  0
  1
  2
  3
  4
  5
  "Value 6 was too high!"
  -- 等待 1s 后重试
*/
```

### 重试策略

<alert>

抽象出的策略方法

</alert>

```ts
import { Observable, throwError, timer } from 'rxjs';
import { mergeMap } from 'rxjs/operators';

const genericRetryStrategy = ({
  maxRetryAttempts = 3,
  scalingDuration = 1000
}: {
  maxRetryAttempts?: number;
  scalingDuration?: number;
} = {}) => (attempts: Observable<any>): Observable<any> =>
  attempts.pipe(
    mergeMap((error, i) => {
      const retryAttempt = i + 1;
      // 如果已经达到最大尝试次数
      if (retryAttempt > maxRetryAttempts) {
        return throwError(error);
      }
      // 1s, 2s, ... 后重试
      return timer(retryAttempt * scalingDuration);
    })
  );
```

下面，给出两种情况的示例。

<code-group>

<code-block label="当失败时重试整个 Observable 序列" active>

1.当失败时重试整个 Observable 序列

```ts
import { Observable, throwError, timer, range, of, from } from 'rxjs';
import { mergeMap, finalize, map, retryWhen, catchError } from 'rxjs/operators';

const genericRetryStrategy = ({
  maxRetryAttempts = 3,
  scalingDuration = 1000
}: {
  maxRetryAttempts?: number;
  scalingDuration?: number;
} = {}) => (attempts: Observable<any>): Observable<any> =>
  attempts.pipe(
    mergeMap((error, i) => {
      const retryAttempt = i + 1;
      // 如果已经达到最大尝试次数
      if (retryAttempt > maxRetryAttempts) {
        return throwError(error);
      }
      console.log(`Attempt ${retryAttempt}: retrying in ${retryAttempt * scalingDuration}ms`);
      // 1s, 2s, ... 后重试
      return timer(retryAttempt * scalingDuration);
    }),
    finalize(() => console.log('We are done!'))
  );

const source = range(1, 10);
const example = source.pipe(
  //
  map(val => val + 3),
  mergeMap(val => {
    if (val > 5 && Math.random() > 0.75) {
      console.log('error:', val);
      return throwError(val);
    }
    return of(val);
  }),
  retryWhen(genericRetryStrategy()),
  catchError(error => of(error))
);
example.subscribe(val => console.log(val));
```

可能的输出类似如下：

```
4..5..6..7..8..9..10..11
error: 12
Attempt 1: retrying in 1000ms
4..5..6..7..8..9..10..11
error: 12
Attempt 2: retrying in 2000ms
4..5..6..7..8..9
error: 10
Attempt 3: retrying in 3000ms
4..5
error: 6
We are done!
6
```

</code-block>

<code-block label="当失败时，只重试前一步出错的步骤">

2.当失败时，只重试前一步出错的步骤

```ts
import { Observable, throwError, timer, range, of, from } from 'rxjs';
import { mergeMap, finalize, map, retryWhen, catchError } from 'rxjs/operators';

const genericRetryStrategy = ({
  maxRetryAttempts = 3,
  scalingDuration = 1000
}: {
  maxRetryAttempts?: number;
  scalingDuration?: number;
} = {}) => (attempts: Observable<any>): Observable<any> =>
  attempts.pipe(
    mergeMap((error, i) => {
      const retryAttempt = i + 1;
      // 如果已经达到最大尝试次数
      if (retryAttempt > maxRetryAttempts) {
        return throwError(error);
      }
      console.log(`Attempt ${retryAttempt}: retrying in ${retryAttempt * scalingDuration}ms`);
      // 1s, 2s, ... 后重试
      return timer(retryAttempt * scalingDuration);
    }),
    finalize(() => console.log('We are done!'))
  );

const source = range(1, 10);
const example = source.pipe(
  //
  map(val => val + 3),
  mergeMap(val =>
    from(
      (function() {
        if (val > 5 && Math.random() > 0.75) {
          console.log('error:', val);
          return throwError(val);
        }
        return of(val);
      })()
    ).pipe(
      //
      retryWhen(genericRetryStrategy()),
      catchError(error => of(error))
    )
  )
);
example.subscribe(val => console.log(val));
```

可能的输出类似这样：

```
4
5
error: 6
Attempt 1: retrying in 1000ms
7
8
error: 9
Attempt 1: retrying in 1000ms
10
error: 11
Attempt 1: retrying in 1000ms
12
13
Attempt 2: retrying in 2000ms
Attempt 2: retrying in 2000ms
Attempt 2: retrying in 2000ms
Attempt 3: retrying in 3000ms
Attempt 3: retrying in 3000ms
Attempt 3: retrying in 3000ms
We are done!
6
We are done!
9
We are done!
11
```

</code-block>

</code-group>

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/retryWhen.ts>

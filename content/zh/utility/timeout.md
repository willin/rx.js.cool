---
title: timeout
description: ''
position: 7000
category: '实用工具'
---

<alert>

在指定时间间隔内不投射值就报错。

</alert>

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/timeout.png)

```ts
timeout<T>(
  due: number | Date,
  scheduler: SchedulerLike = async
): MonoTypeOperatorFunction<T>
```

### 参数

| 参数      | 说明                                            |
| --------- | ----------------------------------------------- |
| due       | 超时时间或日期。                                |
| scheduler | 可选，默认值： `async`。 类型： `SchedulerLike` |

### 返回

类型： `MonoTypeOperatorFunction<T>`

<adsbygoogle></adsbygoogle>

## 示例

### 2.5s 超时

```ts
import { of } from 'rxjs';
import { concatMap, timeout, catchError, delay } from 'rxjs/operators';

// 模拟请求
function makeRequest(timeToDelay) {
  return of('Request Complete!').pipe(delay(timeToDelay));
}

of(4000, 3000, 2000)
  .pipe(
    concatMap(duration =>
      makeRequest(duration).pipe(
        timeout(2500),
        catchError(error => of(`Request timed out after: ${duration}`))
      )
    )
  )
  /*
   * 输出：
   *  "Request timed out after: 4000"
   *  "Request timed out after: 3000"
   *  "Request Complete!"
   */
  .subscribe(val => console.log(val));
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/timeout.ts>

---
title: onErrorResumeNext
description: ''
position: 7000
category: '实用工具'
---

<alert>

如果之前的 Observable 对象投射了完成或者报错通知，立即订阅传入的值。

</alert>

## 说明

该操作符无论成功或失败均会执行，有点类似 `concat` 操作。如果需要异常捕获，参考 [catchError](/error/catchError)。

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/onErrorResumeNext.png)

```ts
onErrorResumeNext<T, R>(
  ...nextSources: any[]
): OperatorFunction<T, R>
```

### 参数

| 参数        | 说明          |
| ----------- | ------------- |
| nextSources | 类型：`any[]` |

### 返回

类型： `OperatorFunction<T, R>`

<adsbygoogle></adsbygoogle>

## 示例

### 基础示例

```ts
import { of } from 'rxjs';
import { onErrorResumeNext, map } from 'rxjs/operators';

of(1, 2, 3, 0)
  .pipe(
    map(x => {
      if (x === 0) {
        throw Error();
      }
      return 10 / x;
    }),
    onErrorResumeNext(of(1, 2, 3))
  )
  .subscribe(
    val => console.log(val),
    err => console.log(err), // Will never be called.
    () => console.log("that's it!")
  );

// 输出:
// 10
// 5
// 3.3333333333333335
// 1
// 2
// 3
// "that's it!"
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/onErrorResumeNext.ts>

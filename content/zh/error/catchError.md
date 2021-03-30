---
title: catchError
description: ''
position: 4000
category: ' 异常处理'
---

<alert>

优雅地处理 Observable 序列中的错误。

</alert>

## 说明

记住要在 catch 函数中返回一个 Observable !

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/catch.png)

```ts
catchError<T, O extends ObservableInput<any>>(
  selector: (err: any, caught: Observable<T>) => O
): OperatorFunction<T, T | ObservedValueOf<O>>
```

### 参数

| 参数     | 说明                                                                                |
| -------- | ----------------------------------------------------------------------------------- |
| selector | 一个抛出错误返回的方法，其源为 Observable 对象，因此你可以使用 `retry` 去进行重试。 |

### 返回

类型： `OperatorFunction<T, T | ObservedValueOf<O>>`

<adsbygoogle></adsbygoogle>

## 示例

### 简单示例

示例 1：

```ts
import { of } from 'rxjs';
import { map, catchError } from 'rxjs/operators';

of(1, 2, 3, 4, 5)
  .pipe(
    map(n => {
      if (n === 4) {
        throw 'four!';
      }
      return n;
    }),
    catchError(err => of('I', 'II', 'III', 'IV', 'V'))
  )
  .subscribe(x => console.log(x));
/*
  输出：
  1, 2, 3, I, II, III, IV, V
*/
```

示例 2：

```ts
import { of } from 'rxjs';
import { map, catchError, take } from 'rxjs/operators';

of(1, 2, 3, 4, 5)
  .pipe(
    map(n => {
      if (n === 4) {
        throw 'four!';
      }
      return n;
    }),
    catchError((err, caught) => caught),
    take(30)
  )
  .subscribe(x => console.log(x));
// 输出：
// 1, 2, 3, 1, 2, 3, ...
```

示例 3：

```ts
import { of } from 'rxjs';
import { map, catchError } from 'rxjs/operators';

of(1, 2, 3, 4, 5)
  .pipe(
    map(n => {
      if (n === 4) {
        throw 'four!';
      }
      return n;
    }),
    catchError(err => {
      throw 'error in source. Details: ' + err;
    })
  )
  .subscribe(
    x => console.log(x),
    err => console.log(err)
  );
// 输出：
// 1, 2, 3, error in source. Details: four!
```

### 捕获 Promise 错误

```ts
import { timer, from, of } from 'rxjs';
import { mergeMap, catchError } from 'rxjs/operators';

//Promise 抛出错误
const myBadPromise = () => new Promise((resolve, reject) => reject('Rejected!'));

const source = timer(1000).pipe(
  mergeMap(_ =>
    from(myBadPromise()).pipe(
      //
      catchError(error => of(`Bad Promise: ${error}`))
    )
  )
);
source.subscribe(val => console.log(val));
//输出: 'Bad Promise: Rejected'
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/catchError.ts>

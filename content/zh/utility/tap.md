---
title: tap
description: ''
position: 7000
category: '实用工具'
---

<alert>

透明执行操作或者 <ruby>边际效应<rp>（</rp><rt>Side-effects</rt><rp>）</rp></ruby>，比如打印日志。

</alert>

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/tap.png)

```ts
tap<T>(
  nextOrObserver?:
    NextObserver<T>
    | ErrorObserver<T>
    | CompletionObserver<T>
    | ((x: T) => void), error?: (e: any) => void, complete?: () => void
): MonoTypeOperatorFunction<T>
```

### 参数

| 参数           | 说明                                                                            |
| -------------- | ------------------------------------------------------------------------------- |
| nextOrObserver | 可选。默认值：`undefined`。一个投射到 `next` 的正常 Observable 对象或者回调函数 |
| error          | 可选。默认值：`undefined`。错误回调。                                           |
| complete       | 可选。默认值：`undefined`。完成回调。                                           |

### 返回

类型： `MonoTypeOperatorFunction<T>`

## 示例

### 打印日志数据

```ts
import { of } from 'rxjs';
import { tap, map } from 'rxjs/operators';

const source = of(1, 2, 3, 4, 5);
const example = source.pipe(
  tap(val => console.log(`BEFORE MAP: ${val}`)),
  map(val => val + 10),
  tap(val => console.log(`AFTER MAP: ${val}`))
);

example.subscribe(val => console.log(val));
// 输出：
// BEFORE MAP: 1
// AFTER MAP: 11
// 11
// BEFORE MAP: 2
// AFTER MAP: 12
// 12
// BEFORE MAP: 3
// AFTER MAP: 13
// 13
// BEFORE MAP: 4
// AFTER MAP: 14
// 14
// BEFORE MAP: 5
// AFTER MAP: 15
// 15
```

### 用于对象

```ts
import { of } from 'rxjs';
import { tap, map } from 'rxjs/operators';

const source = of(1, 2, 3, 4, 5);

const example = source
  .pipe(
    map(val => val + 10),
    tap({
      next: val => {
        // on next 11, etc.
        console.log('on next', val);
      },
      error: error => {
        console.log('on error', error.message);
      },
      complete: () => console.log('on complete')
    })
  )
  .subscribe(val => console.log(val));
// 输出：
// on next 11
// 11
// on next 12
// 12
// on next 13
// 13
// on next 14
// 14
// on next 15
// 15
// on complete
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/tap.ts>

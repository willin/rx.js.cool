---
title: retry
description: ''
position: 4000
category: ' 异常处理'
---

<alert>

如果发生错误，以指定次数重试 Observable 序列。

</alert>

## 说明

- 如果你需要在特定的场景下使用，参考 [retryWhen](/error/retryWhen)
- 如果没有错误的场景中使用，参考 [repeat](/utility/repeat)

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/retry.png)

```ts
retry<T>(count: number = -1): MonoTypeOperatorFunction<T>
```

### 参数

| 参数  | 说明                             |
| ----- | -------------------------------- |
| count | 可选。默认值：`-1`。重试的次数。 |

### 返回

类型： `MonoTypeOperatorFunction<T>`

## 示例

### 简单示例

```ts
import { interval, of, throwError } from 'rxjs';
import { mergeMap, retry } from 'rxjs/operators';

const source = interval(200);
const example = source.pipe(
  mergeMap(val => {
    if (val > 5) {
      return throwError('Error!');
    }
    return of(val);
  }),
  retry(2)
);

example.subscribe({
  next: val => console.log(val),
  error: val => console.log(`${val}: Retried 2 times then quit!`)
});
/*
  输出：
  0..1..2..3..4..5..
  0..1..2..3..4..5..
  0..1..2..3..4..5..
  "Error!: Retried 2 times then quit!"
*/
```

### 补充说明示例

<alert>

- retry 仅当管道中上一个 Observable 对象出错时重试
- 不能与 delay 同时使用
- 出错后重试整个 Observable 序列

</alert>

```ts
import { interval, of, throwError } from 'rxjs';
import { mergeMap, retry, delay } from 'rxjs/operators';

const source = interval(200);
const example = source.pipe(
  mergeMap(val => {
    if (val > 5) {
      return throwError('Error!');
    }
    return of(val);
  }),
  delay(2000),
  retry(2)
);

example.subscribe({
  next: val => console.log(val),
  error: val => console.log(`${val}: Retried 2 times then quit!`)
});

// 输出：
// Error!: Retried 2 times then quit!
```

如果将该例子中的 Delay 换成 200，输出：

```
0..1..2..3..4
0..1..2..3..4
0..1..2..3..4
"Error!: Retried 2 times then quit!"
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/retry.ts>

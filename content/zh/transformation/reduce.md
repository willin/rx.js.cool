---
title: reduce
description: ''
position: 6000
category: '转化操作符'
---

<alert>

将源 Observalbe 序列的值归并为单个值，当源 Observable 完成时将投射这个值。

</alert>

## 说明

- 与众所周知的 `Array.prototype. reduce` 函数相似。
- `reduce` 等同于 `scan` 后紧跟 `last` 一同使用。

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/reduce.png)

```ts
reduce<T, R>(
  accumulator: (acc: T | R, value: T, index?: number) => T | R,
  seed?: T | R
): OperatorFunction<T, T | R>
```

### 参数

| 参数        | 说明                                        |
| ----------- | ------------------------------------------- |
| accumulator | 累加器函数                                  |
| seed        | 可选。默认值：`undefined`，指定初始的累加值 |

### 返回

类型： `OperatorFunction<T, T | R>`

## 示例

### 数字流的加和

```ts
import { of } from 'rxjs';
import { reduce } from 'rxjs/operators';

const source = of(1, 2, 3, 4);
const example = source.pipe(reduce((acc, val) => acc + val));

example.subscribe(val => console.log('Sum:', val));
// 输出:
// Sum: 10'
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/reduce.ts>

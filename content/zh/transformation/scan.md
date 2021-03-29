---
title: map
description: ''
position: 6000
category: '转化操作符'
---

<alert>

随着时间的推移进行归并。

</alert>

## 说明

累加器操作符。此操作符是许多基于 `Redux` 实现的 RxJS 的核心！

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/scan.png)

```ts
scan<T, R>(
  accumulator: (acc: R, value: T, index: number) => R,
  seed?: T | R
): OperatorFunction<T, R>
```

### 参数

| 参数        | 说明                                        |
| ----------- | ------------------------------------------- |
| accumulator | 累加器函数                                  |
| seed        | 可选。默认值：`undefined`，指定初始的累加值 |

### 返回

类型： `OperatorFunction<T, R>`

## 示例

### 总数累加

```ts
import { of } from 'rxjs';
import { scan } from 'rxjs/operators';

const source = of(1, 2, 3);
const example = source.pipe(scan((acc, curr) => acc + curr, 0));
// 输出累加值
example.subscribe(val => console.log(val));
// 输出:
// 1,3,6
```

### 对对象进行累加

```ts
import { Subject } from 'rxjs';
import { scan } from 'rxjs/operators';

const subject = new Subject();
// scan 示例，随着时间的推移构建对象
const example = subject.pipe(scan((acc, curr) => Object.assign({}, acc, curr), {}));

// 输出累加值
example.subscribe(val => console.log('Accumulated object:', val));
// subject 投射的值会被添加成对象的属性
// {name: 'Joe'}
subject.next({ name: 'Joe' });
// {name: 'Joe', age: 30}
subject.next({ age: 30 });
// {name: 'Joe', age: 30, favoriteLanguage: 'JavaScript'}
subject.next({ favoriteLanguage: 'JavaScript' });
```

### 随机投射累加数组中的值

```ts
import { interval } from 'rxjs';
import { scan, map, distinctUntilChanged } from 'rxjs/operators';

// 累加数组中的值，并随机发出此数组中的值
const scanObs = interval(1000)
  .pipe(
    scan((a, c) => [...a, c], []),
    map(r => r[Math.floor(Math.random() * r.length)]),
    distinctUntilChanged()
  )
  .subscribe(console.log);
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/scan.ts>

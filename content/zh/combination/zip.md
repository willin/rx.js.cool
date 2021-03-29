---
title: zip
description: ''
position: 2000
category: '组合操作符'
---

<alert>

组合多个 Observable 的序列来创建一个 Observable 对象，其值是根据每个输入的 Observable 按顺序计算所投射。

</alert>

## 说明

`zip` 操作符会订阅所有内部 Observables，然后等待每个投射一个值。一旦发生这种情况，将投射具有相应索引的所有值。这会持续进行，直到至少一个内部 Observable 完成。

与 `interval` 或 `timer` 进行组合, `zip` 可以用来根据另一个 Observable 进行定时输出！

## 用法

```ts
zip<O extends ObservableInput<any>, R>(
  ...observables: (O | ((...values: ObservedValueOf<O>[]) => R))[]
): Observable<ObservedValueOf<O>[] | R>
```

### 参数

| 参数        | 说明                                                   |
| ----------- | ------------------------------------------------------ |
| observables | 类型：`(O \| ((...values: ObservedValueOf[]) => R))[]` |

### 返回

类型： `Observable<ObservedValueOf<O>[] | R>`

## 示例

### 以交替的时间间隔对多个 Observables 进行 zip

```ts
import { delay } from 'rxjs/operators';
import { of, zip } from 'rxjs';

const sourceOne = of('Hello');
const sourceTwo = of('World!');
const sourceThree = of('Goodbye');
const sourceFour = of('World!');
// 一直等到所有 Observables 都投射一个值，才将所有值作为数组投射
const example = zip(sourceOne, sourceTwo.pipe(delay(1000)), sourceThree.pipe(delay(2000)), sourceFour.pipe(delay(3000)));

example.subscribe(val => console.log(val));
// 输出:
// ["Hello", "World!", "Goodbye", "World!"]
```

### 当一个 Observable 完成时进行 zip

```ts
import { take } from 'rxjs/operators';
import { interval, zip } from 'rxjs';

// 每1秒投射值
const source = interval(1000);
// 当一个 observable 完成时，便不会再投射更多的值了
const example = zip(source, source.pipe(take(2)));

example.subscribe(val => console.log(val));
// 输出:
// [0,0]...[1,1]
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/zip.ts>

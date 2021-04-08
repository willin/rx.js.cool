---
title: merge
description: ''
position: 2000
category: '组合操作符'
---

<alert>

将多个 Observables 转换成单个 Observable。

</alert>

## 说明

- 此操作符可以既有静态方法，又有实例方法！
- 如果产生值的顺序是首要考虑的，那么试试用 [concat](/combination/concat) 来代替！

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/merge.png)

```ts
merge<T, R>(
  ...observables: any[]
): Observable<R>
```

### 参数

| 参数        | 说明            |
| ----------- | --------------- |
| observables | Observable 数组 |

### 返回

类型： `Observable<R>`

<adsbygoogle></adsbygoogle>

## 示例

### 使用静态方法合并多个 Observables

```ts
import { mapTo } from 'rxjs/operators';
import { interval, merge } from 'rxjs';

// 每2.5秒投射值
const first = interval(2500);
// 每2秒投射值
const second = interval(2000);
// 每1.5秒投射值
const third = interval(1500);
// 每1秒投射值
const fourth = interval(1000);

const example = merge(
  // 从一个 observable 中投射输出值
  first.pipe(mapTo('FIRST!')),
  second.pipe(mapTo('SECOND!')),
  third.pipe(mapTo('THIRD')),
  fourth.pipe(mapTo('FOURTH'))
);
example.subscribe(val => console.log(val));
// 输出: "FOURTH", "THIRD", "SECOND!", "FOURTH", "FIRST!", "THIRD", "FOURTH"
```

### 使用实例方法合并 2 个 Observables

```ts
import { merge } from 'rxjs/operators';
import { interval } from 'rxjs';

// 每2.5秒投射值
const first = interval(2500);
// 每1秒投射值
const second = interval(1000);
// 作为实例方法使用
const example = first.pipe(merge(second));
example.subscribe(val => console.log(val));
// 输出: 0,1,0,2....
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/merge.ts>

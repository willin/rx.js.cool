---
title: map
description: ''
position: 6000
category: '转化操作符'
---

<alert>

将源 Observable 投射的每个值应用到给出的 `project` 方法，并将结果值投射为一个 `Observable` 对象。

</alert>

## 说明

与众所周知的 `Array.prototype.map` 函数相似。

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/map.png)

```ts
map<T, R>(
  project: (value: T, index: number) => R, thisArg?: any
): OperatorFunction<T, R>
```

### 参数

| 参数    | 说明                                                            |
| ------- | --------------------------------------------------------------- |
| project | `value`：源 Observable 投射的每个值。 `index`： 从 0 开始的索引 |
| thisArg | 可选。默认值：`undefined`，指定 `project` 方法的作用域          |

### 返回

类型： `OperatorFunction<T, R>`：从源 Observable 投射值执行 `project` 函数转换后的 Observable 序列。

<adsbygoogle></adsbygoogle>

## 示例

### 给每个数值加 10

```ts
import { from } from 'rxjs';
import { map } from 'rxjs/operators';

const source = from([1, 2, 3, 4, 5]);
const example = source.pipe(map(val => val + 10));
example.subscribe(val => console.log(val));
// 输出：
// 11
// 12
// 13
// 14
// 15
```

### 批量转化

```ts
import { from } from 'rxjs';
import { map } from 'rxjs/operators';

const source = from([
  { name: 'Joe', age: 30 },
  { name: 'Frank', age: 20 },
  { name: 'Ryan', age: 50 }
]);
// 只返回原有对象的 name 字段
const example = source.pipe(map(({ name }) => name));
example.subscribe(val => console.log(val));
// 输出：
// Joe
// Frank
// Ryan
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/map.ts>

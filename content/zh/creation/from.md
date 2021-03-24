---
title: from
description: ''
position: 1001
category: '创建操作符'
---

<alert>

从一个 `数组`、`类数组对象`、`Promise 对象`、`可迭代（Iterable）对象`或`类可观察对象`创建一个 `Observable` 对象。

</alert>

## 说明

- 将 Promise 转化为 Observable 对象
- 将数组或迭代器 <ruby>发射<rp>（</rp><rt>Emit</rt><rp>）</rp></ruby> 为一个序列
- 将一个字符串发射为一个字符序列

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/from.png)

```ts
from<T>(input: any, scheduler?: SchedulerLike): Observable<T>
```

### 参数

| 参数      | 说明                                                |
| --------- | --------------------------------------------------- |
| input     | 类型：`any`                                         |
| scheduler | 可选，默认值： `undefined`。 类型： `SchedulerLike` |

### 返回

类型： `Observable<T>`

## 示例

### 数组

将数组转化为 Observable：

```ts
import { from } from 'rxjs';

const arraySource = from([1, 2, 3, 4, 5]);

arraySource.subscribe(val => console.log(val));
```

输出的结果为：

```
1
2
3
4
5
```

### Promise

将 Promise 转化为 Observable：

```ts
import { from } from 'rxjs';

const promiseSource = from(new Promise(resolve => resolve('Hello World!')));

promiseSource.subscribe(val => console.log(val));
```

输出结果为：

```
Hello World!
```

### Generator

将 Generator 转化为 Observable：

```ts
import { from } from 'rxjs';
import { take } from 'rxjs/operators';

function* generateDoubles(seed) {
  let i = seed;
  while (true) {
    yield i;
    i = 2 * i; // double it
  }
}

const iterator = generateDoubles(3);
const result = from(iterator).pipe(take(10));

result.subscribe(x => console.log(x));
```

输出结果为：

```
3
6
12
24
48
96
192
384
768
1536
```

### Map Collection

将 Map 转化为 Observable：

```ts
import { from } from 'rxjs';

const map = new Map();
map.set(1, 'Hi');
map.set(2, 'Bye');

const mapSource = from(map);
mapSource.subscribe(val => console.log(val));
```

输出结果为：

```
[ 1, 'Hi' ]
[ 2, 'Bye' ]
```

### 字符串

将 String 转化为 Observable

```ts
import { from } from 'rxjs';

const source = from('Hello World');
source.subscribe(val => console.log(val));
```

输出结果为：

```
H
e
l
l
o

W
o
r
l
d
```

### 配合 async scheduler

```ts
import { from, asyncScheduler } from 'rxjs';

console.log('start');

const array = [10, 20, 30];
const result = from(array, asyncScheduler);

result.subscribe(x => console.log(x));

console.log('end');
```

输出结果为：

```
start
end
10
20
30
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/observable/from.ts>

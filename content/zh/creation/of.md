---
title: of
description: ''
position: 1001
category: '创建操作符'
---

<alert>

将参数转化成一个 `Observable` 对象。

</alert>

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/of.png)

```ts
of<T>(...args: (SchedulerLike | T)[]): Observable<T>
```

### 参数

| 参数 | 说明                            |
| ---- | ------------------------------- |
| args | 类型： `(SchedulerLike \| T)[]` |

### 返回

类型： `Observable<T>`

## 示例

### 投射一个序列

```ts
import { of } from 'rxjs';

of(10, 20, 30).subscribe(
  next => console.log('next:', next),
  err => console.log('error:', err),
  () => console.log('the end')
);
// 输出:
// next: 10
// next: 20
// next: 30
// the end
```

### 投射一个数组

```ts
import { of } from 'rxjs';

of([1, 2, 3]).subscribe(
  next => console.log('next:', next),
  err => console.log('error:', err),
  () => console.log('the end')
);
// 输出:
// next: [1, 2, 3]
// the end
```

### 混合

```ts
import { of } from 'rxjs';
const source = of({ name: 'Brian' }, [1, 2, 3], Promise.resolve('test'), function hello() {
  return 'Hello';
});
source.subscribe(val => console.log(val));
// 输出:
// { name: "Brain" }
// [1, 2, 3]
// Promise { 'test' }
// [Function: hello]
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/observable/of.ts>

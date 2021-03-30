---
title: expand
description: ''
position: 6000
category: '转化操作符'
---

<alert>

递归调用提供的函数。

</alert>

## 用法

![](https://rxjs.dev/assets/images/marble-diagrams/expand.png)

```ts
expand<T, R>(
  project: (value: T, index: number) => any,
  concurrent: number = Number.POSITIVE_INFINITY,
  scheduler: SchedulerLike = undefined
): OperatorFunction<T, R>
```

### 参数

| 参数       | 说明                                                            |
| ---------- | --------------------------------------------------------------- |
| project    | `value`：源 Observable 投射的每个值。 `index`： 从 0 开始的索引 |
| concurrent | 可选。默认值：`Number.POSITIVE_INFINITY`。最大并发订阅数量。    |
| scheduler  | 可选，默认值： `undefined`。 类型： `SchedulerLike`             |

### 返回

类型： `OperatorFunction<T, R>`

<adsbygoogle></adsbygoogle>

## 示例

### 每次调用加 1

```ts
import { interval, of } from 'rxjs';
import { expand, take } from 'rxjs/operators';

// 发出 2
const source = of(2);
const example = source.pipe(
  // 递归调用提供的函数
  expand(val => {
    // 2,3,4,5,6
    console.log(`Passed value: ${val}`);
    // 3,4,5,6
    return of(1 + val);
  }),
  // 用5次
  take(5)
);

example.subscribe(val => console.log(`RESULT: ${val}`));
// 输出:
// RESULT: 2
// Passed value: 2
// RESULT: 3
// Passed value: 3
// RESULT: 4
// Passed value: 4
// RESULT: 5
// Passed value: 5
// RESULT: 6
// Passed value: 6
```

### Github 分页查询所有的数据

Github RESTful 接口调用[查询仓库](https://developer.github.com/v3/repos/#list-user-repositories)

```
GET /users/willin/repos
```

[接口响应](https://developer.github.com/v3/repos/#response)的头信息里包含

```
Link: <https://api.github.com/user/1890238/repos?page=2>; rel="next",
      <https://api.github.com/user/1890238/repos?page=3>; rel="last"
```

封装 Ajax 调用方法：

```ts [get.ts]
import { Observable } from 'rxjs';
import { ajax, AjaxResponse } from 'rxjs/ajax';
import { map } from 'rxjs/operators';

export function get(
  url: string
): Observable<{
  content: object[];
  next: string | null;
}> {
  return ajax.get(url).pipe(
    map(response => ({
      content: response.response,
      next: next(response)
    }))
  );
}

function next(response: AjaxResponse): string | null {
  let url: string | null = null;
  const link = response.xhr.getResponseHeader('Link');
  if (link) {
    const match = link.match(/<([^>]+)>;\s*rel="next"/);
    if (match) {
      [, url] = match;
    }
  }
  return url;
}
```

Expand 使用示例：

```ts
import { empty } from 'rxjs';
import { concatMap, expand } from 'rxjs/operators';
import { get } from './get';

const url = 'https://api.github.com/users/willin/repos';
const repos = get(url).pipe(
  expand(({ next }) => (next ? get(next) : empty())),
  concatMap(({ content }) => content)
);
repos.subscribe(repo => console.log(repo));
```

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/expand.ts>

---
title: ajax
description: ''
position: 1001
category: '创建操作符'
---

```ts
const ajax: AjaxCreationMethod;
```

一个 ajax 请求的创建操作符，还可以参考 `fetchFrom`。

<adsbygoogle></adsbygoogle>

## 示例

```js
import { ajax } from 'rxjs/ajax';
import { map, catchError } from 'rxjs/operators';
import { of } from 'rxjs';

const obs$ = ajax.getJSON(`https://api.github.com/users?per_page=5`).pipe(
  map(userResponse => console.log(userResponse)),
  catchError(error => {
    console.log('error: ', error);
    return of(error);
  })
);

obs$.subscribe();
```

在线运行： <https://stackblitz.com/edit/rddrz1?devtoolsheight=100&file=index.ts>

## 源码

<https://github.com/ReactiveX/rxjs/blob/master/src/internal/ajax/ajax.ts>

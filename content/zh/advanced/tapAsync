---
title: tapAsync
description: ''
position: 8800
category: '进阶'
---

示例运行：<https://stackblitz.com/edit/rxjs-tap-async?devtoolsheight=100&file=index.ts>

使用 `mergeMap` 实现：

```ts
function tapAsync(fn) {
  return mergeMap(async x => {
    await fn(x);
    return x;
  });
}
```

示例代码：

```ts
import { of } from "rxjs";
import { tap, map, mergeMap } from "rxjs/operators";

function tapAsync(fn) {
  return mergeMap(async x => {
    await fn(x);
    return x;
  });
}

const s$ = of(1, 2, 3, 4, 5, 6).pipe(
  tapAsync(async x => {
    console.log(x);
    await Promise.resolve(x);
  }),
  map(x => x ** 2),
  tap(x => console.log("calced", x))
);

s$.subscribe({
  complete() {
    console.log("ended");
  }
});
```

输出：

```
1
2
3
4
5
6
calced 1
calced 4
calced 9
calced 16
calced 25
calced 36
ended
```

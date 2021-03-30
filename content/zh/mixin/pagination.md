---
title: Pagination 分页
description: ''
position: 9000
category: '项目中使用'
---

<alert>

一般情况下，可用数据库操作进行分页。本篇仅以实现分页为演示，来进行 RxJS 操作说明。

</alert>

一些无关紧要的类型定义：

```ts
export class Cat {
  id: number;
  name: string;
  age: number;
  breed: string;
}

export class PaginatorQueryDto {
  readonly limit?: number = 10;
  readonly offset?: number = 0;
}

export class CatPaginatorDto extends PaginatorQueryDto {
  readonly name?: string;
  readonly age?: number;
  readonly breed?: string;
}
```

<adsbygoogle></adsbygoogle>

实现部分代码：

```ts
import { from, iif, of, forkJoin, Observable } from 'rxjs';
import { mergeMap } from 'rxjs/operators';

pagination({ limit = 10, offset = 0, age, name = '', breed = '' }: CatPaginatorDto): Observable<{items: Cat[]}> {
  const items = from(Promise.resolve([
    // 模拟数据
    { id: 1, name: 'Kitty1', age: 1, breed: 'string' },
    { id: 2, name: 'Kitty2', age: 1, breed: 'string' },
    { id: 3, name: 'Kitty3', age: 1, breed: 'string' },
    { id: 4, name: 'Kitty4', age: 2, breed: 'string' },
    { id: 5, name: 'Kitty5', age: 2, breed: 'string' },
    { id: 6, name: 'Kitty6', age: 2, breed: 'string2' }
  ])).pipe(
    // 此处也仅用于作为 rxjs 的操作示例
    // Filter by Age
    mergeMap((cats: Cat[]) =>
      iif(
        //
        () => age >= 0,
        // Query 里的数值类型，如果没有默认值，实际为字符串类型
        of(cats.filter((cat) => `${cat.age}` === `${age}`)),
        of(cats)
      )
    ),
    // Filter by Name
    mergeMap((cats: Cat[]) =>
      iif(
        //
        () => name !== '',
        of(cats.filter((cat) => cat.name === name)),
        of(cats)
      )
    ),
    // Filter by Breed
    mergeMap((cats: Cat[]) =>
      iif(
        //
        () => breed !== '',
        of(cats.filter((cat) => cat.breed === breed)),
        of(cats)
      )
    ),
    // Pagination
    mergeMap((cats: Cat[]) => of(cats.slice(offset, offset + limit)))
  );
  return forkJoin({
    items
  });
}
```

其中，每个 `mergeMap` 都是一个独立的、特定的过滤操作，除了最后一步将结果进行分页，其他的步骤（如按类型、按年龄查询）均可以调换顺序，或者直接去除，也可以很方便地添加新的过滤字段。

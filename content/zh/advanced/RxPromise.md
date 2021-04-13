---
title: RxPromise
description: ''
position: 8880
category: '进阶'
---

```ts
// RxPromise<T, R = Error> extends Observable<T>
const mockedPromise = new RxPromise(resolver);
```

其中 `resolver` 的类型为：

```ts
(resolve: (r: T) => void, reject: (r: R) => void) => void
```

<adsbygoogle></adsbygoogle>

## Mongoose 查询改为 Observable 序列

```ts
import mongoose from 'mongoose';
import { RxPromise } from 'v0';

mongoose.Promise = RxPromise;

mongoose.connect('mongodb://localhost/test', { useNewUrlParser: true, useUnifiedTopology: true });
const kittySchema = new mongoose.Schema({
  name: String
});
const Kitten = mongoose.model('Kitten', kittySchema);

// 类型转化是必要的，目前我也不知道该怎么优化 Typescript 的返回类型重写。
// 如果您有合适的方法，请联系我 willin(at)willin.org
const s$ = <Observable<Record<string, unknown>[]>>(<any>Kitten.find().exec());
// or
// const s$ = (Kitten.find().exec()) as any) as Observable<Record<string, unknown>[]>;

s$.subscribe({
  next(v) {
    console.log(v);
  },
  complete() {
    console.log('ended');
  }
});
```

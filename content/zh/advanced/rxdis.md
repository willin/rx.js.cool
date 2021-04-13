---
title: rxdis
description: ''
position: 8888
category: '进阶'
---

# Rxdis: Redis Adapter for RxJS

<code-group>
  <code-block label="Yarn" active>

```bash
yarn add rxdis
```

  </code-block>
  <code-block label="NPM">

```bash
npm install --save rxdis
```

  </code-block>
</code-group>

## IORedis Adapter

```ts
import { Rxdis } from 'rxdis';
import IORedis from 'ioredis';

const client = new IORedis();
// options
const rxdis = Rxdis(client);

rxdis
  .set('test', 'hello')
  // Output: OK
  .subscribe(console.log);
```

<adsbygoogle></adsbygoogle>

### Pipeline

```ts
import { Rxdis, Pipeline } from 'rxdis';
import IORedis from 'ioredis';

const redis = new IORedis();
const rxdis = Rxdis(redis);

const source$ = rxdis
  .pipeline()
  .set('test1', 'hello')
  .set('test2', 'world')
  // 类型转换是必要的
  .get('test1') as Pipeline;

source$.exec().subscribe({
  next: console.log,
  complete() {
    rxdis.disconnect();
  },
  error: console.error
});
```

### Multi

```ts
import { Rxdis, Pipeline } from 'rxdis';
import IORedis from 'ioredis';

const redis = new IORedis();
const rxdis = Rxdis(redis);

const source$ = rxdis
  .multi()
  .set('test1', 'hello')
  .set('test2', 'world')
  // 类型转换是必要的
  .get('test1') as Pipeline; // Same with pipeline()

source$.exec().subscribe({
  next: console.log,
  complete() {
    rxdis.disconnect();
  },
  error: console.error
});
```

## Node-Redis Adatper

```ts
import { Rxdis } from 'rxdis';
import Redis from 'redis';

const client = Redis
  .createClient
  // options
  ();
const rxdis = Rxdis(client);

rxdis
  .set('test', 'hello')
  // Output: OK
  .subscribe(console.log);
```

## 支持的命令列表

参考项目仓库说明： <https://github.com/willin/rxdis#typescript-defination>

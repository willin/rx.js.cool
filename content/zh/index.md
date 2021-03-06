---
title: 《RxJS从入门到精通》
description: ''
menuTitle: '简介'
position: 1
category: ''
---

## 介绍

RxJS 是一个使用 <ruby>可观察<rp>（</rp><rt>Observable</rt><rp>）</rp></ruby> 序列来编写异步和 <ruby>基于事件<rp>（</rp><rt>Event-based</rt><rp>）</rp></ruby> 程序的库。

它提供了一种核心类型，即 [Observable](#)，附属类型（Observer，Scheduler，Subject）和受 `Array`（还有 map、filter、reduce、every 等） 启发的 <ruby>运算符<rp>（</rp><rt>Operators</rt><rp>）</rp></ruby>，以允许将异步事件作为集合进行处理。

## 安装

<alert>

本系列文章更新时， RxJS 版本为： <badge>v7.3.0</badge>

</alert>

<code-group>
  <code-block label="Yarn" active>

```bash
yarn add rxjs reflect-metadata v0
```

  </code-block>
  <code-block label="NPM">

```bash
npm install --save rxjs reflect-metadata v0
```

  </code-block>
</code-group>

## 使用

导入整个核心功能集：

```ts
import * as rxjs from 'rxjs';

rxjs.of(1, 2, 3);
```

通过打补丁的方式只导入所需要的(这对于减少 bundling 的体积是十分有用的)：

```ts
import { of } from 'rxjs';
import { map } from 'rxjs/operators';

of(1, 2, 3).pipe(map(x => x + '!!!')); // etc
```

## 进阶

后续章节中会结合项目详细介绍操作符、工具类等的进阶使用。

只要这么一本书， RxJS 从入门到精通。

---

欢迎关注和在 Github 上 Follow 我（[@willin](https://github.com/willin)）。

<adsbygoogle></adsbygoogle>

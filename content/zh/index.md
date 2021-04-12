---
title: 《RxJS从入门到精通》
description: ''
menuTitle: '简介'
position: 1
category: ''
---

# 介绍

RxJS 是一个使用 <ruby>可观察<rp>（</rp><rt>Observable</rt><rp>）</rp></ruby> 序列来编写异步和 <ruby>基于事件<rp>（</rp><rt>Event-based</rt><rp>）</rp></ruby> 程序的库。

它提供了一种核心类型，即 [Observable](#)，附属类型（Observer，Scheduler，Subject）和受 `Array`（还有 map、filter、reduce、every 等） 启发的 <ruby>运算符<rp>（</rp><rt>Operators</rt><rp>）</rp></ruby>，以允许将异步事件作为集合进行处理。

# 安装

<alert>

本系列文章更新时， RxJS 版本为： <badge>v6.6.7</badge> 和 <badge>v7.0.0-beta15</badge>

</alert>

<code-group>
  <code-block label="Yarn" active>

```bash
yarn add v0 rxjs
```

  </code-block>
  <code-block label="NPM">

```bash
npm install --save v0 rxjs
```

  </code-block>
</code-group>

# 使用

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

# 进阶操作符

项目源码： <https://github.com/willin/v0> ，欢迎 Star。

后续章节中会结合项目详细介绍这些操作符的进阶使用。

---

欢迎关注和在 Github 上 Follow 我（[@willin](https://github.com/willin)）。

<adsbygoogle></adsbygoogle>

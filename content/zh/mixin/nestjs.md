---
title: 在 Nest.js 中使用
menuTitle: 'Nest.js'
description: ''
position: 9010
category: '项目中使用'
---

# 在 Controllers / Services 中使用

可以与 Promise 配合使用。

```ts [app.controller.ts]
import { Controller, Get } from '@nestjs/common';
import { from, Observable } from 'rxjs';

import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): Observable<number> {
    return from(Promise.resolve(10));
  }
}
```

# Refs

- 官方文档： https://docs.nestjs.com/interceptors
- 中文文档： https://docs.nestjs.cn/7/interceptors
- Nest Demo： https://github.com/ThomasOliver545/Blog-with-NestJS-and-Angular

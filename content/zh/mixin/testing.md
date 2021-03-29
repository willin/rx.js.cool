---
title: Testing 测试
description: ''
position: 9001
category: '项目中使用'
---

## 返回 Observable 的函数

以上一篇 [Pagination](/mixin/pagination) 为例：

```ts
describe('CatsService', () => {
  it('pagination', complete => {
    pagination({
      // 在这里定义请求参数
      limit: 10,
      offset: 0
    })
      // 直接订阅
      .subscribe({
        next(cats) {
          // 在 next 里进行期望检测
          expect(cats.items).toContainEqual(
            expect.objectContaining({
              name: 'kitty',
              age: 1,
              breed: 'sth'
            })
          );
        },
        // 直接传递 complete 方法（也就是一般命名为 next 或者 done 的方法）
        complete
      });
  });
});
```

## 直接调用 Observable 的函数

示例的函数代码：

```ts
export default function DemoTask(): void {
  //  多线程任务执行器
  const task = from(Promise.resolve('DemoTask')).pipe(
    // 打印参数
    tap(x => console.log(x, 'Task Info'))
    // 处理任务
    // 忽略业务代码
  );

  task.subscribe({
    complete() {
      console.log('Task Done');
    },
    error(err) {
      console.error('Task Failed', err);
    }
  });
}
```

该函数返回类型为 void，直接在其内部进行了 Observable 的订阅和处理。

测试用例写法：

```ts
import { from, timer } from 'rxjs';
import { mergeMap, tap } from 'rxjs/operators';
// other imports

describe('Task Demo', () => {
  it('create task', complete => {
    // 同步方式执行
    DemoTask();
    // 设置一个延迟执行
    const test = timer(300).pipe(
      // 进行期望检测
      mergeMap(() => Promise.resolve(1)),
      tap(val => expect(val).toEqual(1))
      // 多个期望的话可以使用 forkJoin 等方式灵活组排
    );
    test.subscribe({ complete });
  });
});
```

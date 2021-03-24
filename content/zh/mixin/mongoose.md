---
title: 在 Mongoose 中使用
menuTitle: 'Mongoose'
description: ''
position: 9020
category: '项目中使用'
---

Observable 可以从 MongoDB 中按游标顺序提取数据，并能很好地控制并发性。

```ts
import { Observable } from 'rxjs';

// This took me way too long to figure out. Hope this helps someone.
// <3 Well Caffeinated
function fromCursor( cursor ){
  return new Observable((obs) => {
    // is the connection closed
    var closed = false

    // get the next document
    function getNext(){
      return cursor.next((err, doc) => {
        if ( err ){
          return obs.error( err )
        }

        if ( !doc ){
          // no document so we're done
          return obs.complete()
        }

        // call next, however we'll pass it an observable
        // that way we delay fetching the next document until
        // the current one is observed
        obs.next(Rx.Observable.defer( () => {
          if ( !closed ){ getNext() }
          return Rx.Observable.of(doc)
        }))
      })
    }

    // start
    getNext()

    // cleanup
    return () => {
      closed = true;
      cursor.close();
    }
  })
}
```

示例代码：

```ts
const Model = require('some/mongoose/model')
const taskFn = require('some/task/fn')

function performMaintenance( concurrency = 1 ){
  let query = Model.find({ needsMaintenance: true })

  // this will return an observable stream of the result of taskFn
  return fromCursor( query.cursor() ).mergeMap( obs => {
    // we use mergeMap because we want to perform async tasks on each document
    // we need to use flatMap because we're receiving an observable, not the document!

    // taskFn could return an observable, or a promise
    return obs.flatMap( doc => taskFn(doc) )
  }, concurrency)
}

// do stuff
performMaintenance( 2 ).subscribe({
  next: ( result ) => console.log('updated document', result)
  , error: ( err ) => console.error( err )
  , complete: () => console.log('done')
})
```

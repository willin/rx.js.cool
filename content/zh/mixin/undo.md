---
title: undo
description: ''
position: 9020
category: '项目中使用'
---

示例运行：[https://stackblitz.com/edit/rxjs-undo-operator-dnuced](https://stackblitz.com/edit/rxjs-undo-operator-dnuced?devtoolsheight=100&file=index.ts)

```ts
import { of, fromEvent, merge, Observable, EMPTY } from 'rxjs';
import { map, scan, startWith, filter } from 'rxjs/operators';
console.clear();

// Setup
const add1 = document.querySelector('#add1');
const add2 = document.querySelector('#add2');
const undoBtn = document.querySelector('#undo');
const redoBtn = document.querySelector('#redo');
const output = document.querySelector('output');
// A static reference we can use to identify "undo" emissions below
const UNDO_TOKEN = {};
const REDO_TOKEN = {};

const INITIAL_VALUE = 0;

const add1Click$ = fromEvent(add1, 'click');
const add2Click$ = fromEvent(add2, 'click');
const undoClick$ = fromEvent(undoBtn, 'click');
const redoClick$ = fromEvent(redoBtn, 'click');

// Just a source that increments a number over time when
// you click buttons.
const source$ = merge(
  // when we click "+ 1" it adds `1`
  add1Click$.pipe(map(() => 1)),
  // when we click "+ 2" it adds `2`
  add2Click$.pipe(map(() => 2))
).pipe(
  // A reducer to accumlate our number
  scan((acc, n) => acc + n, 0),
  // Start with 0, so we have an initial value to display
  startWith(INITIAL_VALUE)
);

// Here we're adding our custom `undo` and subscribing to it.
source$
  .pipe(
    // "Undo" whenever we click the "Undo" button.
    undo(undoClick$, redoClick$)
  )
  .subscribe(value => (output.textContent = '' + value));

/**
 * Subscribes to the `undoNotifier`, and emits all values from
 * `source`. When `undoNotifier` emits, it will emit previously
 * emitted values back through time.
 *
 * If a `redoNotifier` is passed, it's subscribed to, and when
 * it emits, will "redo" anything that was "undone", unless new
 * values have come from the source.
 *
 * TODO: Add an upper-bounds to the undo state collected.
 */
function undo<T>(undoNotifier: Observable<any>, redoNotifier: Observable<any> = EMPTY) {
  return (source: Observable<T>) =>
    merge(undoNotifier.pipe(map(() => UNDO_TOKEN)), redoNotifier.pipe(map(() => REDO_TOKEN)), source).pipe(
      scan<T, { state: T[]; redos: T[] | null }>(
        (d, x) => {
          let { state, redos } = d;
          if (x === UNDO_TOKEN) {
            // We were notified of an "undo". pop state.
            if (state && state.length > 1) {
              redos = redos || (d.redos = []);
              redos.push(state.pop());
            }
          } else if (x === REDO_TOKEN) {
            if (redos && redos.length > 0) {
              state.push(redos.pop());
            }
          } else {
            if (redos) {
              // clear our redos as new history is written
              redos.length = 0;
            }
            state = state || (d.state = []);
            // It's not an "undo", push state
            state.push(x);
          }
          return d;
        },
        { state: null, redos: null }
      ),
      // we only care about state past here
      map(x => x.state),
      // Don't emit if we don't have state
      filter(x => x !== null),
      // Take the last value from state
      map(state => state[state.length - 1])
    );
}
```

```html
<h1>Very, very simple "undo"</h1>

<p>Click the buttons below to increment the value</p>
<button id="add1">+ 1</button>
<button id="add2">+ 2</button>

<p>Click this button to "undo" previous changes to the value</p>
<button id="undo">Undo</button>

<p>Click this button to "redo" previous changes to the value</p>
<button id="redo">Redo</button>

<p>Value:</p>
<output style="font-size:3rem"></output>
```

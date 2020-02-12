# Examples

I recommend trying these out on [stackblitz](https://stackblitz.com/).

## Imperative

```ts
// imperative example
const doStuff = new Promise((resolve, reject) => {
  try {
    resolve('success');
  } catch (exception) {
    reject('error');
  }
});

doStuff.then(message => {
  console.log(message);
});
```

## Declarative

```ts
// declarative example
import { Observable } from 'rxjs';

const firstObservable = new Observable(subscriber => {
  subscriber.next('success');
  subscriber.complete();
});

firstObservable.subscribe({
  next(x) {
    console.log(x);
  },
  error(err) {
    console.error('error');
  },
  complete() {
    console.log('done');
  }
});
```

## Creation Operator

```ts
// creation operator
// copied from rxjs.dev at https://rxjs.dev/api/index/function/of
import { of } from 'rxjs';

of(10, 20, 30).subscribe(
  next => console.log('next:', next),
  err => console.log('error:', err),
  () => console.log('the end')
);
// console outputs
// next: 10
// next: 20
// next: 30
// the end
```

## Pipeable Operator

```ts
// pipeable operator
// copied from rxjs.dev at https://rxjs.dev/api/operators/mergeMap
import { of, interval } from 'rxjs';
import { mergeMap, map } from 'rxjs/operators';

const letters = of('a', 'b', 'c');
const result = letters.pipe(
  mergeMap(x => interval(1000).pipe(map(i => x + i)))
);
result.subscribe(x => console.log(x));

// Results in the following:
// a0
// b0
// c0
// a1
// b1
// c1
// continues to list a,b,c with respective ascending integers
```

## Handling Subscriptions Memory Leak

```ts
// incorrect
import { interval } from 'rxjs';

const observable = interval(1000);
const subscription = observable.subscribe(() => console.log('Hello!'));
```

## Handling Subscriptions with Memory Leak Fix

```ts
// correct
import { interval } from 'rxjs';

const observable = interval(1000);
const subscription = observable.subscribe(() => console.log('Hello!'));
setTimeout(() => {
  subscription.unsubscribe();
  console.log('unsubscribed');
}, 1000);
```

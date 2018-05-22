@title[let]

## Let's me introduce you to  the __let__ operator

__let me have the whole observable.__

It takes a function that takes an observable and returns an observable. 

```
const filterUndefined = (sourceObservable) => 
  new Observable((observable) => {
    return sourceObservable.subscribe({
      next(value) {
        if (!_.isUndefined(value)) {
          observable.next(fn(value)) 
        }
      },
      error(err) { observable.error(err) }
    })

sourceStream
  .let(filterUndefined())
  .subscribe((value) => console.log(value)))

```

---

@title[let_example]

## Another example, a custom version of map.

```
myMap => (fn) => (sourceObservable) => 
  new Observable((observable) => {
    return sourceObservable.subscribe({
      next(value) { observable.next(fn(value)) },
      error(err) { observable.error(err) }
    })

sourceStream
  .filter((value) => value > 10)))
  .let(myMap((value) => value + 1))
  .subscribe((value) => console.log(value)))

```

---

@title[let_recap]

## Recap

* It's a functional way to use operators through high order functions.
* **It allows you to extend functionality without having to extend the Observable class**.

---

@title[why_is_important]

## ... without having to extend the Observable class

We are doing this...

```
import 'rxjs/add/operators/map';
import 'rxjs/add/operators/filter';
```

---

@title[it_was_bad]

## And that was bad because:


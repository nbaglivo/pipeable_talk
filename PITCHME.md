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
  .let(filterUndefined)
  .subscribe((value) => console.log(value)))

```

---

@title[let_example]

## Another example, a custom version of map.

```
const myMap => (fn) => (sourceObservable) => 
  new Observable((observable) => {
    return sourceObservable.subscribe({
      next(value) { observable.next(fn(value)) },
      error(err) { observable.error(err) }
    })

sourceStream
  .filter((value) => value > 10))
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

We are patching the Observable class.

```
import 'rxjs/add/operators/map';
import 'rxjs/add/operators/filter';

sourceStream
  .filter((value) => value > 10))
  .map(value) => value + 1)
  .subscribe((value) => console.log(value)))
```

---

@title[it_was_bad]

## And that is bad because:

* It looks weird.
* Those lines could be in any file in the projects and the code would still work.
  * So if you stops using map or filter and you forget to remove the import line, the linter or the compiler are not gonna complain.
  * Anyone that uses your library now they have map and filter in their observables, they might depend on it and if you later remove it you could break things.
  * If you remove the import but don't remove the actual function, it might still working if you are importing it from other places, which is confusing.
  
 ---

@title[lettable_operators]

## That's why we habe lettable operators

 ---

@title[lettable_operators]

## That's why we have lettable operators that are pure functions

```
import { map, filter } from 'rxjs/operators/map';

sourceStream
  .let(filter((value) => value > 10)))
  .let(map(value) => value + 1))
  .subscribe((value) => console.log(value)))
```

---

@title[lettable_operators_bad_parts]

## It was a bit too verbose, so we can maybe offer a better way of doing composition

```
import { map, filter } from 'rxjs/operators/map';

sourceStream.pipe(
  filter((value) => value > 10)),
  map(value) => value + 1))
).subscribe((value) => console.log(value)))
```

---

@title[lettable_not_happening]

![pipeable_now](pipable.png)

---

@title[easy_to_reuse]

## Easy to reuse

```
import { map, filter } from 'rxjs/operators/filter';

const filterUndefined = filter(_.negate(_.isUndefined))

sourceStream.pipe(
  filterUndefined,
  map(value) => value + 1))
).subscribe((value) => console.log(value)))
```



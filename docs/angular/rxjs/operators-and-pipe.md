# Операторы и метод pipe()

Для предварительного преобразования отправляемых объектом `Observable` данных или преобразования и управления самими `Observable` используются специальные функции - **операторы**.

```ts
from([7, 21, 10])
  .pipe(map(num => (num <= 10 ? 1 : 0)))
  .subscribe(vl => console.log(vl))
```

Здесь в [`map()`](https://rxjs-dev.firebaseapp.com/api/operators/map) каждое число равное или меньше 10 заменяется на `1`, если больше - на `0`.

Все RxJS операторы подразделяются на категории. Так, различают операторы:

- Создания ([`of`](https://rxjs-dev.firebaseapp.com/api/index/function/of), [`from`](https://rxjs-dev.firebaseapp.com/api/index/function/from), [`fromEvent`](https://rxjs-dev.firebaseapp.com/api/index/function/fromEvent), [`interval`](https://rxjs-dev.firebaseapp.com/api/index/function/interval));
- Преобразования ([`map`](https://rxjs-dev.firebaseapp.com/api/operators/map), [`scan`](https://rxjs-dev.firebaseapp.com/api/operators/scan), [`buffer`](https://rxjs-dev.firebaseapp.com/api/operators/buffer));
- Фильтрации ([`filter`](https://rxjs-dev.firebaseapp.com/api/operators/filter), [`take`](https://rxjs-dev.firebaseapp.com/api/operators/take), [`skip`](https://rxjs-dev.firebaseapp.com/api/operators/skip), [`distinct`](https://rxjs-dev.firebaseapp.com/api/operators/distinct));
- Обработки ошибок ([`catchError`](https://rxjs-dev.firebaseapp.com/api/operators/catchError), [`retry`](https://rxjs-dev.firebaseapp.com/api/operators/retry), [`onErrorResumeNext`](https://rxjs-dev.firebaseapp.com/api/index/function/onErrorResumeNext));
- Условия ([`skipUntil`](https://rxjs-dev.firebaseapp.com/api/operators/skipUntil), [`skipWhile`](https://rxjs-dev.firebaseapp.com/api/operators/skipWhile), [`takeUntil`](https://rxjs-dev.firebaseapp.com/api/operators/takeUntil), [`takeWhile`](https://rxjs-dev.firebaseapp.com/api/operators/takeWhile));
- Математические ([`min`](https://rxjs-dev.firebaseapp.com/api/operators/min), [`max`](https://rxjs-dev.firebaseapp.com/api/operators/max), [`count`](https://rxjs-dev.firebaseapp.com/api/operators/count));
- Утилиты ([`tap`](https://rxjs-dev.firebaseapp.com/api/operators/tap), [`delay`](https://rxjs-dev.firebaseapp.com/api/operators/delay));
- Для Connectable Observable ([`share`](https://rxjs-dev.firebaseapp.com/api/operators/share), [`shareReplay`](https://rxjs-dev.firebaseapp.com/api/operators/shareReplay), [`publish`](https://rxjs-dev.firebaseapp.com/api/operators/publish)).

## pipe()

Начиная с версии 5.5 в библиотеке RxJS большинство операторов, которые применяются к `Observable`, объединяются в методе `pipe()`. Метод вызывается у объекта, а операторы передаются ему в качестве аргументов через запятую в порядке их вызова.

```ts
from([30, 41, 60])
  .pipe(
    filter(num => num % 10 === 0),
    map(num => num * 2)
  )
  .subscribe(vl => console.log(vl))
```

!!! note ""

    Методу `pipe()` передаются только те операторы, которые принимают `Observable` и возвращают `Observable`. Они находятся в `rxjs/operators` и называются pipeable operators (англ.).

В RxJS также есть одноименная утилита `pipe()`, используемая для создания переиспользуемых композиций операторов.

```ts
const doubleFilter = <T, R>(fn: (value: T, index: number) => R) =>
  pipe(
    filter(fn),
    filter(fn)
  )
```

Пример приведен только для наглядности. Практическое использование такого оператора не эффективно.

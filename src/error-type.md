# Типи помилок

Існує кілька варіантів оголошення помилок.
Перш ніж вибрати варіант, який найкраще підходить для вашого випадку, врахуйте наступне.

- Чи потрібно користувачеві зіставляти помилку з іншим типом помилки, щоб обробити її?
  Якщо так, нам потрібно підтримувати функції [`errors.Is`] або [`errors.As`],
  оголошуючи змінну помилки вищого рівня або власного (кастомного) типу.
- Повідомлення про помилку це статичний рядок чи динамічний, для якого потрібна
  контекстна інформація?
  Для першого ми можемо використовувати [`errors.New`], але для останнього ми повинні
  використовувати [`fmt.Errorf`] або власний тип помилки.
- Передаєте помилку з функцій, яка розташована нижче по стеку викликів?
  Тоді перегляньте [розділ про обгортання помилок](error-wrap.md).

  [`errors.Is`]: https://golang.org/pkg/errors/#Is
  [`errors.As`]: https://golang.org/pkg/errors/#As

| Зіставлення помилки? | Повідомлення про помилку | Рекомендація                           |
|----------------------|--------------------------|----------------------------------------|
| ні                   | статична                 | [`errors.New`]                         |
| ні                   | динамічна                | [`fmt.Errorf`]                         |
| так                  | статична                 | верхнього рівня `var` з [`errors.New`] |
| так                  | динамічна                | власний тип `error`                    |

  [`errors.New`]: https://golang.org/pkg/errors/#New
  [`fmt.Errorf`]: https://golang.org/pkg/fmt/#Errorf

Наприклад, використовуйте [`errors.New`] для помилки зі статичним рядком.
Якщо користувачеві потрібно знайти відповідність і обробити помилку, експортуйте цю помилку
як змінну, щоб була підтримка зіставлення з `errors.Is`.

<table>
<thead><tr><th>Немає зіставлення помилки</th><th>Зіставлення помилки</th></tr></thead>
<tbody>
<tr><td>

```go
// package foo

func Open() error {
  return errors.New("could not open")
}

// package bar

if err := foo.Open(); err != nil {
  // Не можливо обробити помилку
  panic("unknown error")
}
```

</td><td>

```go
// package foo

var ErrCouldNotOpen = errors.New("could not open")

func Open() error {
  return ErrCouldNotOpen
}

// package bar

if err := foo.Open(); err != nil {
  if errors.Is(err, foo.ErrCouldNotOpen) {
    // Обробка помилки
  } else {
    panic("unknown error")
  }
}
```

</td></tr>
</tbody></table>

Для помилки з динамічним рядком, використовуйте [`fmt.Errorf`], якщо користувачеві не потрібно виконувати зіставлення помилки з іншим типом помилки або власний `error`, якщо потрібно.

<table>
<thead><tr><th>Немає зіставлення помилки</th><th>Зіставлення помилки</th></tr></thead>
<tbody>
<tr><td>

```go
// package foo

func Open(file string) error {
  return fmt.Errorf("file %q not found", file)
}

// package bar

if err := foo.Open("testfile.txt"); err != nil {
  // Не можливо обробити помилку
  panic("unknown error")
}
```

</td><td>

```go
// package foo

type NotFoundError struct {
  File string
}

func (e *NotFoundError) Error() string {
  return fmt.Sprintf("file %q not found", e.File)
}

func Open(file string) error {
  return &NotFoundError{File: file}
}


// package bar

if err := foo.Open("testfile.txt"); err != nil {
  var notFound *NotFoundError
  if errors.As(err, &notFound) {
    // Обробка помилки
  } else {
    panic("unknown error")
  }
}
```

</td></tr>
</tbody></table>

Врахуйте, якщо ви експортуєте змінні або типи помилок із пакета,
вони стануть частиною загальнодоступного API пакета.

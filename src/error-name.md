# Іменування помилок

Для значень помилок, які зберігаються як глобальні змінні,
використовуйте префікс `Err` або `err` залежно від того, експортовані вони чи ні.
Ці рекомендації замінюють наступні рекомендації – [використовуйте префікс `_` для не експортованих глобальних змінних](global-name.md).
```go
var (
  // Наступні дві помилки експортовано, щоб
  // користувачі даного пакету могли порівняти їх з errors.Is.

  ErrBrokenLink = errors.New("link is broken")
  ErrCouldNotOpen = errors.New("could not open")

  // Ця помилка не експортується, оскільки ми не хочемо
  // робити її частиною нашого загальнодоступного API.
  // Ми все ще можемо використовувати її в середині пакету з errors.Is.

  errNotFound = errors.New("not found")
)
```

Для власних типів помилок використовуйте суфікс `Error`.

```go
// Подібним чином ця помилка експортується, щоб
// користувачі даного пакету могли зіставити її з errors.As.

type NotFoundError struct {
  File string
}

func (e *NotFoundError) Error() string {
  return fmt.Sprintf("file %q not found", e.File)
}

// Ця помилка не експортується, тому що ми не хочемо
// робити її частиною публічного API.
// Ми все ще можемо використовувати її в середині пакету з errors.As.

type resolveError struct {
  Path string
}

func (e *resolveError) Error() string {
  return fmt.Sprintf("resolve %q", e.Path)
}
```

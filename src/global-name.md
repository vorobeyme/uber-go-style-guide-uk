# Використовуйте префікс `_` для не експортованих глобальних змінних

До не експортованих змінних `var` та констант `const` верхнього рівня додайте префікс `_`,
щоб під час їх використання було зрозуміло, що вони є глобальними символами.

Пояснення: змінні та константи верхнього рівня мають область видимості всього пакету.
Використання загальних імен дозволяє випадкове використання неправильного значення
в іншому файлі того ж пакету.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
// foo.go

const (
  defaultPort = 8080
  defaultUser = "user"
)

// bar.go

func Bar() {
  defaultPort := 9090
  ...
  fmt.Println("Default port", defaultPort)

  // Ми не побачимо помилку компіляції, якщо перший рядок
  // Bar() буде видалено.
}
```

</td><td>

```go
// foo.go

const (
  _defaultPort = 8080
  _defaultUser = "user"
)
```

</td></tr>
</tbody></table>

**Виняток**: не експортовані значення помилок повинні мати префікс `err` без підкреслення.
Див. [іменування помилок](error-name.md).

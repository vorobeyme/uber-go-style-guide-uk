# Уникайте паніки

Код, який працює у виробничому середовищі (production), повинен уникати паніки.
Паніка є основним джерелом [каскадних збоїв]. Якщо виникає помилка,
функція повинна повернути помилку та дозволити користувачу вирішити, як її обробити.

  [каскадних збоїв]: https://en.wikipedia.org/wiki/Cascading_failure

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
func run(args []string) {
  if len(args) == 0 {
    panic("an argument is required")
  }
  // ...
}

func main() {
  run(os.Args[1:])
}
```

</td><td>

```go
func run(args []string) error {
  if len(args) == 0 {
    return errors.New("an argument is required")
  }
  // ...
  return nil
}

func main() {
  if err := run(os.Args[1:]); err != nil {
    fmt.Fprintln(os.Stderr, err)
    os.Exit(1)
  }
}
```

</td></tr>
</tbody></table>

Panic/recover не є стратегією обробки помилок. Програма повинна панікувати лише тоді,
коли трапляється щось непоправне, наприклад, nil розіменування (nil dereference).
Винятком є ініціалізація програми: помилки під час запуску програми, які мають
порушити роботу програми, можуть викликати паніку.

```go
var _statusTemplate = template.Must(template.New("name").Parse("_statusHTML"))
```

Навіть у тестах віддавайте перевагу `t.Fatal` або `t.FailNow` замість паніки,
щоб гарантувати, що тест буде позначено як невдалий.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
// func TestFoo(t *testing.T)

f, err := os.CreateTemp("", "test")
if err != nil {
  panic("failed to set up test")
}
```

</td><td>

```go
// func TestFoo(t *testing.T)

f, err := os.CreateTemp("", "test")
if err != nil {
  t.Fatal("failed to set up test")
}
```

</td></tr>
</tbody></table>

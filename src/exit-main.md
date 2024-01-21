# Вихід в Main

Програми Go використовують [`os.Exit`] або [`log.Fatal*`] для негайного виходу.
Паніка не є хорошим способом виходу з програм, будь ласка, [уникайте паніки](panic.md).

  [`os.Exit`]: https://golang.org/pkg/os/#Exit
  [`log.Fatal*`]: https://golang.org/pkg/log/#Fatal

Викликайте `os.Exit` або `log.Fatal*` **лише в `main()`**.
Усі інші функції повинні повертати помилки, щоб повідомляти про збій.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
func main() {
  body := readFile(path)
  fmt.Println(body)
}

func readFile(path string) string {
  f, err := os.Open(path)
  if err != nil {
    log.Fatal(err)
  }

  b, err := io.ReadAll(f)
  if err != nil {
    log.Fatal(err)
  }

  return string(b)
}
```

</td><td>

```go
func main() {
  body, err := readFile(path)
  if err != nil {
    log.Fatal(err)
  }
  fmt.Println(body)
}

func readFile(path string) (string, error) {
  f, err := os.Open(path)
  if err != nil {
    return "", err
  }

  b, err := io.ReadAll(f)
  if err != nil {
    return "", err
  }

  return string(b), nil
}
```

</td></tr>
</tbody></table>

Обґрунтування: програми з кількома функціями, які містять вихід, створюють кілька проблем:

- Неочевидний потік керування: будь-яка функція може вийти з програми,
  тому стає важко міркувати про потік керування.
- Важко тестувати: функція, яка виходить з програми, також вийде із тесту, який її викликає.
  Це ускладнює тестування функції та створює ризик пропуску інших тестів,
  які ще не були запущені `go test`.
- Пропущене звільнення ресурсів: коли функція виходить з програми, вона пропускає виклики функцій,
  поставлених у чергу з операторами `defer`. Це збільшує ризик пропуску важливих завдань очищення
  (звільнення ресурсів).

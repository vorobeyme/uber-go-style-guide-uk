# Використовуйте імена полів для ініціалізації структур

Ви майже завжди повинні вказувати імена полів під час ініціалізації структур.
Це обов'язково при використанні [`go vet`].

  [`go vet`]: https://golang.org/cmd/vet/

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
k := User{"John", "Doe", true}
```

</td><td>

```go
k := User{
    FirstName: "John",
    LastName: "Doe",
    Admin: true,
}
```

</td></tr>
</tbody></table>

Виняток: назви полів *можна* опускати в тестових таблицях, якщо є 3 або менше полів.

```go
tests := []struct{
  op Operation
  want string
}{
  {Add, "add"},
  {Subtract, "subtract"},
}
```

# Пропускайте поля з нульовими значеннями в структурах

Під час ініціалізації структур з іменами полів, не додавайте поля,
які мають нульові значення, якщо вони не несуть змістовний контекст.
В іншому випадку дозвольте Go автоматично встановити ці поля з нульовими значеннями.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
user := User{
  FirstName: "John",
  LastName: "Doe",
  MiddleName: "",
  Admin: false,
}
```

</td><td>

```go
user := User{
  FirstName: "John",
  LastName: "Doe",
}
```

</td></tr>
</tbody></table>

Це допомагає зменшити шум для користувачів, пропускаючи значення за замовчуванням у цьому контексті.
Вказуються лише поля, які мають значення.

Додайте нульові значення, якщо назви полів надають змістовний контекст.
Наприклад, тестові кейси в [тестових таблицях](test-table.md) можуть бути корисними з іменами полів,
навіть якщо вони мають нульове значення.

```go
tests := []struct{
  give string
  want int
}{
  {give: "0", want: 0},
  // ...
}
```

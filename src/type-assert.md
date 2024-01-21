# Обробка помилок підтвердження типу

Форма [підтвердження типу] з єдиним значенням, що повертається, викличе паніку
через неправильний тип. Тому завжди використовуйте ідіому "кома добре" ("comma ok").

  [підтвердження типу]: https://golang.org/ref/spec#Type_assertions

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
t := i.(string)
```

</td><td>

```go
t, ok := i.(string)
if !ok {
  // коректно обробити помилку
}
```

</td></tr>
</tbody></table>

<!-- TODO: There are a few situations where the single assignment form is
fine. -->

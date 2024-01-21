# Форматування рядків за межами Printf

Якщо ви оголошуєте форматування рядків для функцій у стилі `Printf`
поза рядковим літералом, винесіть їх значення в `const`.

Це допомагає `go vet` виконувати статичний аналіз форматування рядка.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
msg := "unexpected values %v, %v\n"
fmt.Printf(msg, 1, 2)
```

</td><td>

```go
const msg = "unexpected values %v, %v\n"
fmt.Printf(msg, 1, 2)
```

</td></tr>
</tbody></table>

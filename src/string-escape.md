# Використовуйте необроблені рядкові літерали, щоб уникнути екранування

Go підтримує [необроблені рядкові літерали (raw string literals)](https://golang.org/ref/spec#raw_string_lit),
які можуть охоплювати кілька рядків та містити лапки.
Використовуйте їх, щоб уникнути ручного додавання екранованих символів, яке значно погіршує читабельність.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
wantError := "unknown name:\"test\""
```

</td><td>

```go
wantError := `unknown error:"test"`
```

</td></tr>
</tbody></table>

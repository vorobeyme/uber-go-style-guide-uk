# Ініціалізація посилань на структури

Використовуйте `&T{}` замість `new(T)` під час ініціалізації посилань на структуру,
щоб це було відповідно до ініціалізації структури.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
sval := T{Name: "foo"}

// непослідовність
sptr := new(T)
sptr.Name = "bar"
```

</td><td>

```go
sval := T{Name: "foo"}

sptr := &T{Name: "bar"}
```

</td></tr>
</tbody></table>

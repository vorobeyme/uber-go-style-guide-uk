# Оголошення змінних верхнього рівня

На верхньому рівні використовуйте стандартне ключове слово `var`.
Не вказуйте тип, за винятком випадків, коли вираз не збігається з типом.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
var _s string = F()

func F() string { return "A" }
```

</td><td>

```go
var _s = F()
// Оскільки ф-ція F вже вказує, що повертає рядок,
// нам не потрібно вказувати тип знову.

func F() string { return "A" }
```

</td></tr>
</tbody></table>

Вкажіть тип, якщо тип виразу не відповідає бажаному типу.


```go
type myError struct{}

func (myError) Error() string { return "error" }

func F() myError { return myError{} }

var _e error = F()
// F повертає об'єкт типу myError, але ми хочемо повернути error.
```

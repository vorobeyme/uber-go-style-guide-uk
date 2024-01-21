# Групування та впорядкування функцій

- Функції мають бути відсортовані в приблизному порядку викликів.
- Функції у файлі повинні бути згруповані відповідно до отримувача.

Таким чином, експортовані функції повинні з'явитися у файлі першими, одразу після визначень
`struct`, `const` та `var`.

Функції `newXYZ()`/`NewXYZ()` можуть з'явитися після визначення типу,
але перед рештою методів одержувача.

Оскільки функції згруповані за одержувачами, звичайні службові функції мають з'являтися
в кінці файлу.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
func (s *something) Cost() {
  return calcCost(s.weights)
}

type something struct{ ... }

func calcCost(n []int) int {...}

func (s *something) Stop() {...}

func newSomething() *something {
    return &something{}
}
```

</td><td>

```go
type something struct{ ... }

func newSomething() *something {
    return &something{}
}

func (s *something) Cost() {
  return calcCost(s.weights)
}

func (s *something) Stop() {...}

func calcCost(n []int) int {...}
```

</td></tr>
</tbody></table>

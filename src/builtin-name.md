# Уникайте використання вбудованих імен

У [специфікації мови] Go описано декілька вбудованих,
[попередньо визначених ідентифікаторів], які не слід використовувати як імена в програмах Go.

Залежно від контексту, повторне використання цих ідентифікаторів як імен
або приховає оригінал у межах поточної лексичної області (і будь-яких вкладених областях),
або зробить код заплутаним.
У кращому випадку можна очікувати попереджень від компілятора, у гіршому - такий
код може створити приховані помилки, які важко помітити.

  [специфікації мови]: https://golang.org/ref/spec
  [попередньо визначених ідентифікаторів]: https://golang.org/ref/spec#Predeclared_identifiers

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
var error string
// `error` приховує таке ж саме вбудоване ім'я 

// або

func handleErrorMessage(error string) {
    // `error` приховує вбудоване ім'я
}
```

</td><td>

```go
var errorMessage string
// `error` відноситься до вбудованого імені

// або

func handleErrorMessage(msg string) {
    // `error` відноситься до вбудованого імені
}
```

</td></tr>
<tr><td>

```go
type Foo struct {
    // Хоча ці поля технічно не приховують
    // вбудовані імена, пошук для рядків `error` або` string`
    // дає неоднозначні результатти.
    error  error
    string string
}

func (f Foo) Error() error {
    // `error` та `f.error` візуально схожі
    return f.error
}

func (f Foo) String() string {
    // `error` та `f.error` візуально схожі
    return f.string
}
```

</td><td>

```go
type Foo struct {
    // Рядки `error` and `string` тепер однозначні.
    err error
    str string
}

func (f Foo) Error() error {
    return f.err
}

func (f Foo) String() string {
    return f.str
}
```

</td></tr>
</tbody></table>

Зауважте, що компілятор не генеруватиме помилок під час використання попередньо оголошених
ідентифікаторів, але такі інструменти, як `go vet`, мають правильно вказувати на ці та інші
випадки приховування вбудованих імен.

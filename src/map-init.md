# Ініціалізація карт

Надавайте перевагу функції `make(..)` для порожніх карт і карт, що заповнені програмно.
Це робить ініціалізацію карт візуально відмінним від оголошення,
а також дозволяє простіше вказувати розмір карти пізніше, якщо доступно.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
var (
  // m1 безпечний для читання та запису;
  // m2 буде паніка під час запису.
  m1 = map[T1]T2{}
  m2 map[T1]T2
)
```

</td><td>

```go
var (
  // m1 безпечний для читання та запису;
  // m2 буде паніка під час запису.
  m1 = make(map[T1]T2)
  m2 map[T1]T2
)
```

</td></tr>
<tr><td>

Оголошення та ініціалізація візуально схожі.

</td><td>

Оголошення та ініціалізація візуально відрізняються.

</td></tr>
</tbody></table>

Якщо це можливо, вказуйте місткість під час ініціалізації карт за допомогою функції `make()`.
Додаткову інформацію дивіться в розділі [вказуйте місткість для карт](#вказуйте-місткість-для-карт).

З іншого боку, якщо карта містить фіксований список елементів,
використовуйте літерали карти, щоб ініціалізувати карту.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
m := make(map[T1]T2, 3)
m[k1] = v1
m[k2] = v2
m[k3] = v3
```

</td><td>

```go
m := map[T1]T2{
  k1: v1,
  k2: v2,
  k3: v3,
}
```

</td></tr>
</tbody></table>

Основне правило полягає в тому, щоб використовувати літерали карт під час
додавання фіксованого набору елементів при ініціалізації, інакше використовуйте `make` та вкажіть розмір, якщо це можливо.

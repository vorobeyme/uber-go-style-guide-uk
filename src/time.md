# Використовуйте пакет `"time"` для обробки часу

Управління часом є складною темою. Неправильні припущення, які часто пов'язанні з "часом",
припускають наступне.

1. Тривалість доби становить 24 години
2. Година має 60 хвилин
3. Тиждень має 7 днів
4. Рік має 365 днів
5. [Та багато іншого](https://infiniteundo.com/post/25326999628/falsehoods-programmers-believe-about-time)

Наприклад, *1* означає, що додавання 24 годин до певного моменту часу не гарантує,
що ви отримаєте інший календарний день.

Тому завжди використовуйте пакет [`"time"`], коли маєте справу з часом, оскільки він
допомагає впоратися з цими неправильними припущеннями у безпечніший та точніший спосіб.

  [`"time"`]: https://golang.org/pkg/time/

## Використовуйте `time.Time` для моментів з часом

Використовуйте [`time.Time`] коли маєте справу з моментами часу, а також методи `time.Time` для
порівняння, додавання або віднімання часу.

  [`time.Time`]: https://golang.org/pkg/time/#Time

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
func isActive(now, start, stop int) bool {
  return start <= now && now < stop
}
```

</td><td>

```go
func isActive(now, start, stop time.Time) bool {
  return (start.Before(now) || start.Equal(now)) && now.Before(stop)
}
```

</td></tr>
</tbody></table>

## Використовуйте `time.Duration` для проміжків часу

Використовуйте [`time.Duration`] коли маєте справу з часовими проміжками.

  [`time.Duration`]: https://golang.org/pkg/time/#Duration

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
func poll(delay int) {
  for {
    // ...
    time.Sleep(time.Duration(delay) * time.Millisecond)
  }
}

poll(10) // це були секунди чи мілісекунди?
```

</td><td>

```go
func poll(delay time.Duration) {
  for {
    // ...
    time.Sleep(delay)
  }
}

poll(10*time.Second)
```

</td></tr>
</tbody></table>

Повертаючись до прикладу з додаванням 24 годин до певного моменту часу,
метод, який ми використовуємо для додавання часу, залежить від наших намірів.
Якщо ми хочемо отримати той самий час доби, але наступного календарного дня, ми повинні
використовувати [`Time.AddDate`]. Однак, якщо ми хочемо гарантувати, що отримаємо момент часу,
зміщений на 24 години після попереднього часу, то слід використовувати [`Time.Add`].

  [`Time.AddDate`]: https://golang.org/pkg/time/#Time.AddDate
  [`Time.Add`]: https://golang.org/pkg/time/#Time.Add

```go
newDay := t.AddDate(0 /* years */, 0 /* months */, 1 /* days */)
maybeNewDay := t.Add(24 * time.Hour)
```

## Використовуйте `time.Time` та `time.Duration` із зовнішніми системами

Використовуйте `time.Duration` та `time.Time` у взаємодії із зовнішніми системами, коли це можливо.
Наприклад:

- Параметри командного рядка: [`flag`] підтримують `time.Duration` через
  [`time.ParseDuration`]
- JSON: [`encoding/json`] підтримує кодування `time.Time` як [RFC 3339]
  рядок через його [`UnmarshalJSON` метод]
- SQL: [`database/sql`] підтримує перетворення `DATETIME` або `TIMESTAMP` стовпців
  в `time.Time` і назад, якщо це підтримує базовий драйвер
- YAML: [`gopkg.in/yaml.v2`] підтримує `time.Time` як [RFC 3339] рядок, та
  `time.Duration` через [`time.ParseDuration`].

  [`flag`]: https://golang.org/pkg/flag/
  [`time.ParseDuration`]: https://golang.org/pkg/time/#ParseDuration
  [`encoding/json`]: https://golang.org/pkg/encoding/json/
  [RFC 3339]: https://tools.ietf.org/html/rfc3339
  [`UnmarshalJSON` метод]: https://golang.org/pkg/time/#Time.UnmarshalJSON
  [`database/sql`]: https://golang.org/pkg/database/sql/
  [`gopkg.in/yaml.v2`]: https://godoc.org/gopkg.in/yaml.v2

Якщо неможливо використати `time.Duration` у цих взаємодіях, використовуйте
`int` або `float64` та включіть одиницю часу в назву поля.

Наприклад, оскільки `encoding/json` не підтримує `time.Duration`, одиниця часу включається в назву поля.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
// {"interval": 2}
type Config struct {
  Interval int `json:"interval"`
}
```

</td><td>

```go
// {"intervalMillis": 2000}
type Config struct {
  IntervalMillis int `json:"intervalMillis"`
}
```

</td></tr>
</tbody></table>

Якщо неможливо використовувати `time.Time` у цих взаємодіях та якщо інше не погоджено,
використовуйте `string` і форматуйте мітки часу (timestamps), як визначено в [RFC 3339].
Цей формат використовується за замовчуванням  [`Time.UnmarshalText`] та доступний
для використання в `Time.Format` і `time.Parse` через [`time.RFC3339`].

  [`Time.UnmarshalText`]: https://golang.org/pkg/time/#Time.UnmarshalText
  [`time.RFC3339`]: https://golang.org/pkg/time/#RFC3339

Хоча на практиці це не є проблемою, майте на увазі, що пакет `"time"` не підтримує
розбір міток часу (timestamps) з високосними (додатковими) секундами ([8728]),
а також не враховує високосні секунди в обчисленнях ([15190]). Якщо ви порівнюєте два моменти часу,
різниця не включатиме високосні секунди, які могли відбутися між цими двома моментами.

  [8728]: https://github.com/golang/go/issues/8728
  [15190]: https://github.com/golang/go/issues/15190

# Перевірка відповідності інтерфейсу

Якщо необхідно, перевірте відповідність інтерфейсу під час компіляції. Це включає:

- Експортовані типи, які необхідні для реалізації певних інтерфейсів згідно з контрактом API
- Експортовані або не експортовані типи, які є частиною групи типів, що реалізують той самий інтерфейс
- Інші випадки, коли недотримання інтерфейсу може спричинити проблеми для користувачів


<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
type Handler struct {
  // ...
}



func (h *Handler) ServeHTTP(
  w http.ResponseWriter,
  r *http.Request,
) {
  // ...
}
```

</td><td>

```go
type Handler struct {
  // ...
}

var _ http.Handler = (*Handler)(nil)

func (h *Handler) ServeHTTP(
  w http.ResponseWriter,
  r *http.Request,
) {
  // ...
}
```

</td></tr>
</tbody></table>

Оператор `var _ http.Handler = (*Handler)(nil)` не вдасться скомпілювати, якщо
`*Handler` перестане відповідати інтерфейсу `http.Handler`.

Права сторона призначення має бути нульовим значенням (zero-value) заявленого типу.
Це `nil` для вказівників (як `*Handler`), зрізів (slices) і карт (maps), а також
порожня структура для типів структур.

```go
type LogHandler struct {
  h   http.Handler
  log *zap.Logger
}

var _ http.Handler = LogHandler{}

func (h LogHandler) ServeHTTP(
  w http.ResponseWriter,
  r *http.Request,
) {
  // ...
}
```

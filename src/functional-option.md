# Функціональні параметри

Функціональні параметри – це шаблон, у якому ви оголошуєте непрозорий тип `Option`,
який записує інформацію в деяку внутрішню структуру. Ви можете приймати різну
кількість цих параметрів і діяти відповідно до повної інформації,
записаної параметрами у внутрішній структурі.

Використовуйте цей шаблон для необов'язкових аргументів у конструкторах та інших
публічних API, які ви передбачаєте розширювати, особливо якщо ви вже маєте три або більше аргументів у цих функціях..

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
// package db

func Open(
  addr string,
  cache bool,
  logger *zap.Logger
) (*Connection, error) {
  // ...
}
```

</td><td>

```go
// package db

type Option interface {
  // ...
}

func WithCache(c bool) Option {
  // ...
}

func WithLogger(log *zap.Logger) Option {
  // ...
}

// Open створює з'єднання.
func Open(
  addr string,
  opts ...Option,
) (*Connection, error) {
  // ...
}
```

</td></tr>
<tr><td>

Параметри cache та logger мають бути надані завжди,
навіть якщо користувач хоче використовувати параметри за замовчуванням.

```go
db.Open(addr, db.DefaultCache, zap.NewNop())
db.Open(addr, db.DefaultCache, log)
db.Open(addr, false /* cache */, zap.NewNop())
db.Open(addr, false /* cache */, log)
```

</td><td>

Параметри надаються лише за потреби.

```go
db.Open(addr)
db.Open(addr, db.WithLogger(log))
db.Open(addr, db.WithCache(false))
db.Open(
  addr,
  db.WithCache(false),
  db.WithLogger(log),
)
```

</td></tr>
</tbody></table>

Ми пропонуємо реалізувати цей шаблон за допомогою інтерфейсу `Option`,
який містить не експортований метод, що записує параметри в не експортовану структуру `options`.

```go
type options struct {
  cache  bool
  logger *zap.Logger
}

type Option interface {
  apply(*options)
}

type cacheOption bool

func (c cacheOption) apply(opts *options) {
  opts.cache = bool(c)
}

func WithCache(c bool) Option {
  return cacheOption(c)
}

type loggerOption struct {
  Log *zap.Logger
}

func (l loggerOption) apply(opts *options) {
  opts.logger = l.Log
}

func WithLogger(log *zap.Logger) Option {
  return loggerOption{Log: log}
}

// Open створює з'єднання.
func Open(
  addr string,
  opts ...Option,
) (*Connection, error) {
  options := options{
    cache:  defaultCache,
    logger: zap.NewNop(),
  }

  for _, o := range opts {
    o.apply(&options)
  }

  // ...
}
```

Зауважте, що існує метод реалізації цього шаблону за допомогою замикань, але ми вважаємо,
що наведений вище шаблон забезпечує більшу гнучкість для авторів і є більш простим у налагодженні та тестуванні для користувачів. Зокрема, він дозволяє порівнювати варіанти між собою у тестах та макетах (mocks), проти замикань, де це неможливо. Крім того, це дозволяє варіантам реалізовувати
інші інтерфейси, зокрема `fmt.Stringer`, який дозволяє зрозумілі для користувача рядкові
представлення параметрів.

Дивіться також,

- [Самореференційні функції та дизайн опцій]
- [Функціональні параметри для зручних API]

  [Самореференційні функції та дизайн опцій]: https://commandcenter.blogspot.com/2014/01/self-referential-functions-and-design.html
  [Функціональні параметри для зручних API]: https://dave.cheney.net/2014/10/17/functional-options-for-friendly-apis

<!-- TODO: replace this with parameter structs and functional options, when to
use one vs other -->

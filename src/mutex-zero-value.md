# Дозволене використання м'ютексів (mutex) з нульовими значеннями

Нульові значення (zero-value) `sync.Mutex` та `sync.RWMutex` є правильними, тому вам майже ніколи
не потрібно використовувати вказівник на м'ютекс.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
mu := new(sync.Mutex)
mu.Lock()
```

</td><td>

```go
var mu sync.Mutex
mu.Lock()
```

</td></tr>
</tbody></table>

Якщо ви використовуєте структуру за вказівником, тоді м'ютекс має бути полем без вказівника.
Не вбудовуйте м'ютекс у структуру, навіть якщо структуру не було експортовано.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
type SMap struct {
  sync.Mutex

  data map[string]string
}

func NewSMap() *SMap {
  return &SMap{
    data: make(map[string]string),
  }
}

func (m *SMap) Get(k string) string {
  m.Lock()
  defer m.Unlock()

  return m.data[k]
}
```

</td><td>

```go
type SMap struct {
  mu sync.Mutex

  data map[string]string
}

func NewSMap() *SMap {
  return &SMap{
    data: make(map[string]string),
  }
}

func (m *SMap) Get(k string) string {
  m.mu.Lock()
  defer m.mu.Unlock()

  return m.data[k]
}
```

</td></tr>

<tr><td>

Поле `Mutex` та методи `Lock` і `Unlock` ненавмисно є частиною експортованого API `SMap`.

</td><td>

М'ютекс та його методи є деталями реалізації `SMap`, прихованими від тих, хто їх викликає.

</td></tr>
</tbody></table>

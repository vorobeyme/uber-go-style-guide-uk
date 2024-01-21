# Використовуйте go.uber.org/atomic

Атомарні операції з пакетом [sync/atomic] працюють із необробленими типами
(`int32`, `int64` тощо), тому легко забути використовувати атомарну операцію
для читання або модифікації змінних.

[go.uber.org/atomic] додає цим операціям захист типу, приховуючи базовий тип.
Крім того, він містить зручний тип `atomic.Bool`.

  [go.uber.org/atomic]: https://godoc.org/go.uber.org/atomic
  [sync/atomic]: https://golang.org/pkg/sync/atomic/

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
type foo struct {
  running int32  // atomic
}

func (f* foo) start() {
  if atomic.SwapInt32(&f.running, 1) == 1 {
     // вже працює…
     return
  }
  // запустіть Foo
}

func (f *foo) isRunning() bool {
  return f.running == 1  // race!
}
```

</td><td>

```go
type foo struct {
  running atomic.Bool
}

func (f *foo) start() {
  if f.running.Swap(true) {
     // вже працює…
     return
  }
  // запустіть Foo
}

func (f *foo) isRunning() bool {
  return f.running.Load()
}
```

</td></tr>
</tbody></table>

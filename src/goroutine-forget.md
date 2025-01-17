# Не використовуйте горутини як "вистрілив та забув"

Горутини легкі, але вони не безплатні: як мінімум, вони потребують пам'яті для стеку та процесора, який потрібно планувати. Хоча ці витрати невеликі для типового використання горутин, вони можуть спричинити значні проблеми з продуктивністю, якщо їх створювати у великій кількості та не контролювати час їх життя. Горутини з некерованим терміном життя також можуть викликати інші проблеми, такі як заважання збірці сміття для об'єктів, які не використовуються та утримання ресурсів, які більше не використовуються.

Отже, не допускайте витоку горутин в продакшн коді. Використовуйте [go.uber.org/goleak](https://pkg.go.dev/go.uber.org/goleak), щоб перевірити витоки горутин в середині пакетів, які можуть створювати горутини.


Загалом, кожна горутина:

- повинна мати передбачуваний час, в який вона припиняє виконання; або
- повинен існувати спосіб сигналізувати про те, що вона повинна зупинитися

В обох випадках повинен існувати код, який блокує і чекає на завершення роботи горутини.

Наприклад:

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
go func() {
  for {
    flush()
    time.Sleep(delay)
  }
}()
```

</td><td>

```go
var (
  stop = make(chan struct{}) // повідомляє горутину, що вона повинна зупинитися
  done = make(chan struct{}) // повідомляє нас, що горутину завершено
)
go func() {
  defer close(done)

  ticker := time.NewTicker(delay)
  defer ticker.Stop()
  for {
    select {
    case <-ticker.C:
      flush()
    case <-stop:
      return
    }
  }
}()

// В іншому місці...
close(stop)  // сигналізувати горутині про зупинку
<-done       // і почекати, поки вона завершиться
```

</td></tr>
<tr><td>

Зупинити цю горутину неможливо.
Вона виконуватиметься до завершення роботи програми.

</td><td>

Цю горутину можна зупинити за допомогою `close(stop)`,
а можна дочекатися її завершення за допомогою `<-done`.

</td></tr>
</tbody></table>

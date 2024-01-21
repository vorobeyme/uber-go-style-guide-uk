# Уникайте відкритих параметрів

Відкриті параметри у викликах функцій можуть погіршити читабельність.
Додайте коментарі у стилі мови C (`/* ... */`) для імен параметрів, якщо їхнє значення неочевидне.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
// func printInfo(name string, isLocal, done bool)

printInfo("foo", true, true)
```

</td><td>

```go
// func printInfo(name string, isLocal, done bool)

printInfo("foo", true /* isLocal */, true /* done */)
```

</td></tr>
</tbody></table>

А ще краще, замініть відкриті типи `bool` на власні типи для більш читабельного та безпечного коду.
Також це дозволить більше ніж два стани (true/false) для цього параметра в майбутньому.

```go
type Region int

const (
  UnknownRegion Region = iota
  Local
)

type Status int

const (
  StatusReady Status = iota + 1
  StatusDone
  // Можливо в майбутньому ми матимемо StatusInProgress.
)

func printInfo(name string, region Region, status Status)
```

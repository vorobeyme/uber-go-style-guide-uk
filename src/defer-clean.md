# Defer для звільнення ресурсів

Використовуйте `defer` для звільнення ресурсів, таких як файли та блокування.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
p.Lock()
if p.count < 10 {
  p.Unlock()
  return p.count
}

p.count++
newCount := p.count
p.Unlock()

return newCount

// легко пропустити розблокування через багаторазове використання return
```

</td><td>

```go
p.Lock()
defer p.Unlock()

if p.count < 10 {
  return p.count
}

p.count++
return p.count

// виглядає більше читабельно
```

</td></tr>
</tbody></table>

Defer має надзвичайно низькі витрати ресурсів і тому його слід уникати в тих випадках, якщо ви можете довести,
що час виконання вашої функції становить наносекунди. Перевага оператора `defer` щодо зручності
читання вашого коду вартує тих мізерних витрат ресурсів на його використання.
Це особливо має відношення до більших методів, які мають більше, ніж простий доступ до пам'яті,
де інші обчислення важливіші ніж `defer`.

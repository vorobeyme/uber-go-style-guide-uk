# Обмеження копіювання зрізів (slices) та карт (maps)

Зрізи та карти містять вказівники на основні дані, тому будьте обережні зі сценаріями,
коли їх потрібно скопіювати.

## Отримання зрізів і карт

Майте на увазі, що користувачі можуть змінювати карту або зріз, які ви отримали як аргумент,
якщо ви зберігаєте посилання на них.

<table>
<thead><tr><th>Не рекомендовано</th> <th>Рекомендовано</th></tr></thead>
<tbody>
<tr>
<td>

```go
func (d *Driver) SetTrips(trips []Trip) {
  d.trips = trips
}

trips := ...
d1.SetTrips(trips)

// Ви мали на увазі змінити d1.trips?
trips[0] = ...
```

</td>
<td>

```go
func (d *Driver) SetTrips(trips []Trip) {
  d.trips = make([]Trip, len(trips))
  copy(d.trips, trips)
}

trips := ...
d1.SetTrips(trips)

// Тепер ми можемо змінювати trips[0], не впливаючи на d1.trips.
trips[0] = ...
```

</td>
</tr>

</tbody>
</table>

## Повернення зрізів і карт

Так само остерігайтеся користувацьких модифікацій карт або зрізів,
що розкривають внутрішній стан.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
type Stats struct {
  mu sync.Mutex
  counters map[string]int
}

// Snapshot повертає поточну статистику.
func (s *Stats) Snapshot() map[string]int {
  s.mu.Lock()
  defer s.mu.Unlock()

  return s.counters
}

// знімок більше не захищений м'ютексом, тому будь-який доступ
// до знімка може призвести до гонки даних (data race).
snapshot := stats.Snapshot()
```

</td><td>

```go
type Stats struct {
  mu sync.Mutex
  counters map[string]int
}

func (s *Stats) Snapshot() map[string]int {
  s.mu.Lock()
  defer s.mu.Unlock()

  result := make(map[string]int, len(s.counters))
  for k, v := range s.counters {
    result[k] = v
  }
  return result
}

// Знімок тепер є копією.
snapshot := stats.Snapshot()
```

</td></tr>
</tbody></table>



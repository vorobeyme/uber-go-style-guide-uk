# nil є допустимим зрізом

`nil` є допустимим зрізом, що має довжину та ємність 0. Це означає, що,

- Не слід явно повертати зріз нульової довжини. Натомість повертайте `nil`.

  <table>
  <thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
  <tbody>
  <tr><td>

  ```go
  if x == "" {
    return []int{}
  }
  ```

  </td><td>

  ```go
  if x == "" {
    return nil
  }
  ```

  </td></tr>
  </tbody></table>

- Щоб перевірити, чи порожній зріз, завжди використовуйте `len(s) == 0`. Не перевіряйте його на `nil`.

  <table>
  <thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
  <tbody>
  <tr><td>

  ```go
  func isEmpty(s []string) bool {
    return s == nil
  }
  ```

  </td><td>

  ```go
  func isEmpty(s []string) bool {
    return len(s) == 0
  }
  ```

  </td></tr>
  </tbody></table>

- Зріз, який оголошений за допомогою `var` (нульове значення), можна одразу використовувати (без використання `make()`).

  <table>
  <thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
  <tbody>
  <tr><td>

  ```go
  nums := []int{}
  // or, nums := make([]int)

  if add1 {
    nums = append(nums, 1)
  }

  if add2 {
    nums = append(nums, 2)
  }
  ```

  </td><td>

  ```go
  var nums []int

  if add1 {
    nums = append(nums, 1)
  }

  if add2 {
    nums = append(nums, 2)
  }
  ```

  </td></tr>
  </tbody></table>

Пам'ятайте, що, хоча nil є допустимим зрізом, він не еквівалентний зрізу, якому задано довжину 0.
Перший зріз nil, а другий ні – тому в різних ситуаціях вони можуть оброблятися по-різному
(наприклад, серіалізація).

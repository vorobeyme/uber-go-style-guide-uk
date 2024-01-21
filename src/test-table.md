# Тестові таблиці

Використовуйте тести на основі таблиць з [субтестами], щоб уникнути дублювання коду,
коли основна логіка тестування повторюється.

Якщо система, що тестується, повинна бути протестована за різних умов, коли певні частини вхідних і вихідних даних змінюються, слід використовувати табличний тест, щоб зменшити дублювання та поліпшити читабельність.

  [субтестами]: https://blog.golang.org/subtests

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
// func TestSplitHostPort(t *testing.T)

host, port, err := net.SplitHostPort("192.0.2.0:8000")
require.NoError(t, err)
assert.Equal(t, "192.0.2.0", host)
assert.Equal(t, "8000", port)

host, port, err = net.SplitHostPort("192.0.2.0:http")
require.NoError(t, err)
assert.Equal(t, "192.0.2.0", host)
assert.Equal(t, "http", port)

host, port, err = net.SplitHostPort(":8000")
require.NoError(t, err)
assert.Equal(t, "", host)
assert.Equal(t, "8000", port)

host, port, err = net.SplitHostPort("1:8")
require.NoError(t, err)
assert.Equal(t, "1", host)
assert.Equal(t, "8", port)
```

</td><td>

```go
// func TestSplitHostPort(t *testing.T)

tests := []struct{
  give     string
  wantHost string
  wantPort string
}{
  {
    give:     "192.0.2.0:8000",
    wantHost: "192.0.2.0",
    wantPort: "8000",
  },
  {
    give:     "192.0.2.0:http",
    wantHost: "192.0.2.0",
    wantPort: "http",
  },
  {
    give:     ":8000",
    wantHost: "",
    wantPort: "8000",
  },
  {
    give:     "1:8",
    wantHost: "1",
    wantPort: "8",
  },
}

for _, tt := range tests {
  t.Run(tt.give, func(t *testing.T) {
    host, port, err := net.SplitHostPort(tt.give)
    require.NoError(t, err)
    assert.Equal(t, tt.wantHost, host)
    assert.Equal(t, tt.wantPort, port)
  })
}
```

</td></tr>
</tbody></table>

Тестові таблиці спрощують додавання контексту до повідомлень про помилки,
зменшують дублювання логіки та додають нові тестові випадки.

Ми дотримуємося домовленості, згідно з якою зріз структур називається `tests`,
а кожен тестовий випадок — `tt`. Крім того, ми заохочуємо пояснювати вхідні та вихідні
значення для кожного тесту з префіксами `give` та `want`.

```go
tests := []struct{
  give     string
  wantHost string
  wantPort string
}{
  // ...
}

for _, tt := range tests {
  // ...
}
```

## Уникайте зайвої складності в табличних тестах

Табличні тести може бути складно читати та супроводжувати, якщо субтести містять умовні твердження або іншу логіку розділення. Табличні тести **НЕ** слід використовувати, якщо всередині субтестів є складна логіка або логіка з умовами (наприклад, складна логіка всередині циклу `for`).

Великі, складні табличні тести шкодять читабельності та підтримуваності, оскільки користувачі тестів можуть мати труднощі з відладкою помилок тестів, що можуть виникати.

Такі табличні тести слід розбивати на кілька таблиць або на кілька окремих функцій `Test...`.

Деякі ідеали, до яких варто прагнути:

* Зосередьтеся на найвужчій частині поведінки
* Мінімізуйте "глибину тесту" (test depth) та уникайте умовних тверджень (див. нижче)
* Переконайтеся, що всі поля таблиці використовуються у всіх тестах
* Переконайтеся, що вся логіка тесту виконується для всіх випадків таблиць

У цьому контексті "глибина тесту" означає "кількість послідовних тверджень у тесті, які вимагають, щоб попередні твердження були правильними" (подібно до цикломатичної складності). "Дрібніші" тести означають, що між твердженнями менше зв'язків і, що важливіше, що ці твердження з меншою ймовірністю будуть умовними за замовчуванням.

Зокрема, табличні тести можуть стати заплутаними і важкими для читання, якщо вони використовують декілька шляхів розгалуження, (наприклад, `shouldError`, `expectCall` і тд.), використовують багато операторів `if` для конкретних імітаційних очікуваних сценаріїв (наприклад, `shouldCallFoo`), або розміщують функції всередині таблиці (наприклад, `setupMocks func(*FooMock)`).

Однак при тестуванні поведінки, де зміни відбуваються лише в залежності від вхідних даних, може бути більш доцільним згрупувати подібні випадки разом у табличному тесті. Це дозволяє наочно відобразити, як змінюється поведінка відповідно до всіх вхідних даних, у порівнянні з розбивкою подібних випадків на окремі тести. Такий підхід полегшує порівняння та робить їх більш зрозумілими.

Якщо тіло тесту коротке і зрозуміле, цілком допустимо мати єдиний розгалужений шлях для успішних та неуспішних випадків з полем таблиці типу `shouldErr`, для вказівки очікуваних помилок.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
func TestComplicatedTable(t *testing.T) {
  tests := []struct {
    give          string
    want          string
    wantErr       error
    shouldCallX   bool
    shouldCallY   bool
    giveXResponse string
    giveXErr      error
    giveYResponse string
    giveYErr      error
  }{
    // ...
  }

  for _, tt := range tests {
    t.Run(tt.give, func(t *testing.T) {
      // налаштування моків
      ctrl := gomock.NewController(t)
      xMock := xmock.NewMockX(ctrl)
      if tt.shouldCallX {
        xMock.EXPECT().Call().Return(
          tt.giveXResponse, tt.giveXErr,
        )
      }
      yMock := ymock.NewMockY(ctrl)
      if tt.shouldCallY {
        yMock.EXPECT().Call().Return(
          tt.giveYResponse, tt.giveYErr,
        )
      }

      got, err := DoComplexThing(tt.give, xMock, yMock)

      // перевірка результатів
      if tt.wantErr != nil {
        require.EqualError(t, err, tt.wantErr)
        return
      }
      require.NoError(t, err)
      assert.Equal(t, want, got)
    })
  }
}
```

</td><td>

```go
func TestShouldCallX(t *testing.T) {
  // налаштування моків
  ctrl := gomock.NewController(t)
  xMock := xmock.NewMockX(ctrl)
  xMock.EXPECT().Call().Return("XResponse", nil)

  yMock := ymock.NewMockY(ctrl)

  got, err := DoComplexThing("inputX", xMock, yMock)

  require.NoError(t, err)
  assert.Equal(t, "want", got)
}

func TestShouldCallYAndFail(t *testing.T) {
  // налаштування моків
  ctrl := gomock.NewController(t)
  xMock := xmock.NewMockX(ctrl)

  yMock := ymock.NewMockY(ctrl)
  yMock.EXPECT().Call().Return("YResponse", nil)

  _, err := DoComplexThing("inputY", xMock, yMock)
  assert.EqualError(t, err, "Y failed")
}
```
</td></tr>
</tbody></table>

Така складність призводить до того, що складніше змінювати, розуміти та підтверджувати правильність тесту.

Хоча чітких рекомендацій не існує, читабельність та підтримуваність завжди повинні бути в пріоритеті при виборі між табличними тестами та окремими тестами для декількох входів/виходів системи.

## Паралельні тести

Паралельні тести, як і деякі спеціалізовані цикли (наприклад, ті, що породжують
горутини або захоплюють посилання як частину тіла циклу), мають подбати про те,
щоб явно призначити змінні циклу в межах циклу, щоб гарантувати, що вони зберігають
очікувані значення.

Паралельні тести, як і деякі спеціалізовані цикли (наприклад, ті, що породжують горутини або перехоплюють посилання як частину тіла циклу),
повинні подбати про явне присвоювання змінних циклу в межах його області видимості, щоб гарантувати, що вони отримують очікувані значеня.

```go
tests := []struct{
  give string
  // ...
}{
  // ...
}

for _, tt := range tests {
  tt := tt // для t.Parallel
  t.Run(tt.give, func(t *testing.T) {
    t.Parallel()
    // ...
  })
}
```

У наведеному вище прикладі ми повинні оголосити змінну `tt`, яка буде видимою для ітерації циклу через використання функції `t.Parallel()` нижче. Якщо ми цього не зробимо, більшість або всі тести отримають неочікуване значення `tt` або значення, яке змінюватиметься під час виконання.

<!-- TODO: Explain how to use _test packages. -->

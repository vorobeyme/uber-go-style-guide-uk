<!--

Editing this document:

- Discuss all changes in GitHub issues first.
- Update the table of contents as new sections are added or removed.
- Use tables for side-by-side code samples. See below.

Code Samples:

Use 2 spaces to indent. Horizontal real estate is important in side-by-side
samples.

For side-by-side code samples, use the following snippet.

~~~
<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
BAD CODE GOES HERE
```

</td><td>

```go
GOOD CODE GOES HERE
```

</td></tr>
</tbody></table>
~~~

(You need the empty lines between the <td> and code samples for it to be
treated as Markdown.)

If you need to add labels or descriptions below the code samples, add another
row before the </tbody></table> line.

~~~
<tr>
<td>DESCRIBE BAD CODE</td>
<td>DESCRIBE GOOD CODE</td>
</tr>
~~~

-->

<<<<<<< HEAD
# Посібник зі стилю Go від Uber

## Зміст

- [Вступ](#вступ)
- [Настанови](#настанови)
  - [Вказівники на інтерфейси](#вказівники-на-інтерфейси)
  - [Перевірка відповідності інтерфейсу](#перевірка-відповідності-інтерфейсу)
  - [Одержувачі (receivers) та інтерфейси](#одержувачі-receivers-та-інтерфейси)
  - [Дозволене використання м'ютексів (mutex) з нульовими значеннями](#дозволене-використання-мютексів-mutex-з-нульовими-значеннями)
  - [Обмеження копіювання зрізів (slices) та карт (maps)](#обмеження-копіювання-зрізів-slices-та-карт-maps)
  - [Defer для звільнення ресурсів](#defer-для-звільнення-ресурсів)
  - [Розмір каналу (channel) дорівнює одиниці або не вказано](#розмір-каналу-channel-дорівнює-одиниці-або-не-вказано)
  - [Починайте перерахування (enums) з одиниці](#починайте-перерахування-enums-з-одиниці)
  - [Використовуйте пакет `"time"` для обробки часу](#використовуйте-пакет-time-для-обробки-часу)
  - [Помилки](#помилки)
    - [Типи помилок](#типи-помилок)
    - [Упакування помилок (wrapping)](#упакування-помилок-wrapping)
    - [Іменування помилок](#іменування-помилок)
  - [Обробка помилок підтвердження типу](#обробка-помилок-підтвердження-типу)
  - [Уникайте паніки](#уникайте-паніки)
  - [Використовуйте go.uber.org/atomic](#використовуйте-gouberorgatomic)
  - [Уникайте непостійних (mutable) глобальних змінних](#уникайте-непостійних-mutable-глобальних-змінних)
  - [Уникайте вбудовування типів (type embedding) у публічні структури](#уникайте-вбудовування-типів-type-embedding-у-публічні-структури)
  - [Уникайте використання вбудованих імен](#уникайте-використання-вбудованих-імен)
  - [Уникайте `init()`](#уникайте-init)
  - [Вихід в Main](#вихід-в-main)
    - [Виходьте один раз](#виходьте-один-раз)
  - [Використовуйте теги полів у серіалізованих структурах](#використовуйте-теги-полів-у-серіалізованих-структурах)
- [Продуктивність](#продуктивність)
  - [Надавайте перевагу `strconv` замість `fmt`](#надавайте-перевагу-strconv-замість-fmt)
  - [Уникайте конвертації string-to-byte](#уникайте-конвертації-string-to-byte)
  - [Намагайтесь вказувати місткість (capacity) контейнера](#намагайтесь-вказувати-місткість-capacity-контейнера)
    - [Вказуйте місткість для карт](#вказуйте-місткість-для-карт)
    - [Вказуйте місткість для зрізів](#вказуйте-місткість-для-зрізів)
- [Стиль](#стиль)
  - [Уникайте надто довгих рядків](#уникайте-надто-довгих-рядків)
  - [Будьте послідовними](#будьте-послідовними)
  - [Групуйте схожі оголошення](#групуйте-схожі-оголошення)
  - [Порядок імпортування бібліотек](#порядок-імпортування-бібліотек)
  - [Назви пакетів](#назви-пакетів)
  - [Назви функцій](#назви-функцій)
  - [Імпорт псевдонімів](#імпорт-псевдонімів)
  - [Групування та впорядкування функцій](#групування-та-впорядкування-функцій)
  - [Зменште вкладеність](#зменште-вкладеність)
  - [Зайвий оператор else](#зайвий-оператор-else)
  - [Оголошення змінних верхнього рівня](#оголошення-змінних-верхнього-рівня)
  - [Використовуйте префікс `_` для не експортованих глобальних змінних](#використовуйте-префікс-_-для-не-експортованих-глобальних-змінних)
  - [Вбудовування в структури (embedding)](#вбудовування-в-структури-embedding)
  - [Оголошення локальних змінних](#оголошення-локальних-змінних)
  - [nil це повноцінний зріз](#nil-це-повноцінний-зріз)
  - [Зменште область видимості змінних](#зменште-область-видимості-змінних)
  - [Уникайте відкритих параметрів](#уникайте-відкритих-параметрів)
  - [Використовуйте необроблені рядкові літерали, щоб уникнути екранування](#використовуйте-необроблені-рядкові-літерали-щоб-уникнути-екранування)
  - [Ініціалізація структур](#ініціалізація-структур)
    - [Використовуйте імена полів для ініціалізації структур](#використовуйте-імена-полів-для-ініціалізації-структур)
    - [Пропускайте поля з нульовими значеннями в структурах](#пропускайте-поля-з-нульовими-значеннями-в-структурах)
    - [Використовуйте `var` для структур з нульовим значенням](#використовуйте-var-для-структур-з-нульовим-значенням)
    - [Ініціалізація структурних посилань](#ініціалізація-структурних-посилань)
  - [Ініціалізація карт (maps)](#ініціалізація-карт-maps)
  - [Форматування рядків поза Printf](#форматування-рядків-поза-printf)
  - [Назви функцій у стилі Printf](#назви-функцій-у-стилі-printf)
- [Шаблони](#шаблони)
  - [Тестові таблиці](#тестові-таблиці)
  - [Функціональні параметри](#функціональні-параметри)
=======
# Uber Go Style Guide

## Table of Contents

- [Introduction](#introduction)
- [Guidelines](#guidelines)
  - [Pointers to Interfaces](#pointers-to-interfaces)
  - [Verify Interface Compliance](#verify-interface-compliance)
  - [Receivers and Interfaces](#receivers-and-interfaces)
  - [Zero-value Mutexes are Valid](#zero-value-mutexes-are-valid)
  - [Copy Slices and Maps at Boundaries](#copy-slices-and-maps-at-boundaries)
  - [Defer to Clean Up](#defer-to-clean-up)
  - [Channel Size is One or None](#channel-size-is-one-or-none)
  - [Start Enums at One](#start-enums-at-one)
  - [Use `"time"` to handle time](#use-time-to-handle-time)
  - [Errors](#errors)
    - [Error Types](#error-types)
    - [Error Wrapping](#error-wrapping)
    - [Error Naming](#error-naming)
  - [Handle Type Assertion Failures](#handle-type-assertion-failures)
  - [Don't Panic](#dont-panic)
  - [Use go.uber.org/atomic](#use-gouberorgatomic)
  - [Avoid Mutable Globals](#avoid-mutable-globals)
  - [Avoid Embedding Types in Public Structs](#avoid-embedding-types-in-public-structs)
  - [Avoid Using Built-In Names](#avoid-using-built-in-names)
  - [Avoid `init()`](#avoid-init)
  - [Exit in Main](#exit-in-main)
    - [Exit Once](#exit-once)
  - [Use field tags in marshaled structs](#use-field-tags-in-marshaled-structs)
  - [Don't fire-and-forget goroutines](#dont-fire-and-forget-goroutines)
    - [Wait for goroutines to exit](#wait-for-goroutines-to-exit)
    - [No goroutines in `init()`](#no-goroutines-in-init)
- [Performance](#performance)
  - [Prefer strconv over fmt](#prefer-strconv-over-fmt)
  - [Avoid string-to-byte conversion](#avoid-string-to-byte-conversion)
  - [Prefer Specifying Container Capacity](#prefer-specifying-container-capacity)
      - [Specifying Map Capacity Hints](#specifying-map-capacity-hints)
      - [Specifying Slice Capacity](#specifying-slice-capacity)
- [Style](#style)
  - [Avoid overly long lines](#avoid-overly-long-lines)
  - [Be Consistent](#be-consistent)
  - [Group Similar Declarations](#group-similar-declarations)
  - [Import Group Ordering](#import-group-ordering)
  - [Package Names](#package-names)
  - [Function Names](#function-names)
  - [Import Aliasing](#import-aliasing)
  - [Function Grouping and Ordering](#function-grouping-and-ordering)
  - [Reduce Nesting](#reduce-nesting)
  - [Unnecessary Else](#unnecessary-else)
  - [Top-level Variable Declarations](#top-level-variable-declarations)
  - [Prefix Unexported Globals with _](#prefix-unexported-globals-with-_)
  - [Embedding in Structs](#embedding-in-structs)
  - [Local Variable Declarations](#local-variable-declarations)
  - [nil is a valid slice](#nil-is-a-valid-slice)
  - [Reduce Scope of Variables](#reduce-scope-of-variables)
  - [Avoid Naked Parameters](#avoid-naked-parameters)
  - [Use Raw String Literals to Avoid Escaping](#use-raw-string-literals-to-avoid-escaping)
  - [Initializing Structs](#initializing-structs)
      - [Use Field Names to Initialize Structs](#use-field-names-to-initialize-structs)
      - [Omit Zero Value Fields in Structs](#omit-zero-value-fields-in-structs)
      - [Use `var` for Zero Value Structs](#use-var-for-zero-value-structs)
      - [Initializing Struct References](#initializing-struct-references)
  - [Initializing Maps](#initializing-maps)
  - [Format Strings outside Printf](#format-strings-outside-printf)
  - [Naming Printf-style Functions](#naming-printf-style-functions)
- [Patterns](#patterns)
  - [Test Tables](#test-tables)
  - [Functional Options](#functional-options)
>>>>>>> e0b30cab6a22cb59465ecacde311f360820652d9
- [Linting](#linting)

## Вступ

Стилі — це домовленості щодо керування кодом. Термін "стиль" тут дещо вводить в оману,
оскільки описані тут угоди охоплюють набагато більше, ніж просто форматування вихідного файлу,
з яким і так чудово справляється `gofmt`.

Мета цього посібника — структурувати дані домовленості шляхом детального опису того,
як потрібно, а також як не потрібно писати код на Go в Uber.
Ці правила існують для того, щоб зберегти кодову базу керованою і при цьому дозволити
інженерам продуктивно використовувати можливості мови Go.

Даний посібник був створений [Prashant Varanasi] та [Simon Newton], щоб ознайомити
колег із використанням мови Go. Протягом багатьох років він змінювався та вдосконалювався на основі
отриманих відгуків.

[Prashant Varanasi]: https://github.com/prashantv
[Simon Newton]: https://github.com/nomis52

Ця документація містить багаті на ідіоми правила коду Go, яких дотримуються в Uber.
Багато з них є загальними рекомендаціями для Go, в той час, як інші походять із зовнішніх джерел:

1. [Effective Go](https://golang.org/doc/effective_go.html)
2. [Go Common Mistakes](https://github.com/golang/go/wiki/CommonMistakes)
3. [Go Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments)

Ми прагнемо, щоб приклади коду були точними для двох останніх проміжних [версій]((https://go.dev/doc/devel/release)) Go.

Під час запуску через `golint` та `go vet`, ваш код не повинен містити помилок.
Рекомендуємо налаштувати ваш редактор наступним чином:

- Запускати `goimports` під час збереження
- Запускати `golint` та `go vet` для перевірки на наявність помилок

Інформацію про підтримку вашим редактором Go інструментів ви можете знайти тут:
<https://github.com/golang/go/wiki/IDEsAndTextEditorPlugins>

## Настанови

### Вказівники на інтерфейси
Вам майже ніколи не знадобиться вказівник на інтерфейс. Ви повинні передавати інтерфейси за значенням,
оскільки дані, що лежать в основі, завжди можуть бути вказівником.

Інтерфейс складається з двох полів:

1. Вказівник на певну інформацію про тип. Він представлений як "тип".
2. Вказівник на дані. Якщо дані містять вказівник, вони зберігаються напряму.
   Якщо дані містять значення, то зберігається вказівник на це значення.

Якщо ви хочете, щоб методи інтерфейсу могли б змінювати базові дані, то вам слід використовувати вказівник.

### Перевірка відповідності інтерфейсу
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

### Одержувачі (receivers) та інтерфейси

Методи з одержувачами за значенням, можуть бути викликані як за вказівниками, так і за значеннями.
Методи з одержувачами вказівника, можуть бути викликані лише через вказівник або [адресовані значення].

[адресовані значення]: https://golang.org/ref/spec#Method_values

Наприклад,

```go
type S struct {
  data string
}

func (s S) Read() string {
  return s.data
}

func (s *S) Write(str string) {
  s.data = str
}

sVals := map[int]S{1: {"A"}}

// Read можна викликати лише за значенням
sVals[1].Read()

// Це не скомпілюється:
// sVals[1].Write("test")

sPtrs := map[int]*S{1: {"A"}}

// Read та Write можна викликати за допомогою вказівника
sPtrs[1].Read()
sPtrs[1].Write("test")
```

Подібним чином інтерфейс може бути реалізований як вказівник, навіть якщо одержувач методу переданий як значення.

```go
type F interface {
  f()
}

type S1 struct{}

func (s S1) f() {}

type S2 struct{}

func (s *S2) f() {}

s1Val := S1{}
s1Ptr := &S1{}
s2Val := S2{}
s2Ptr := &S2{}

var i F
i = s1Val
i = s1Ptr
i = s2Ptr

// Наступний код не скомпілюється, оскільки s2Val є значенням, а для f немає одержувача за значенням.
// i = s2Val
```

Effective Go чудово описує [вказівники або значення].

[вказівники або значення]: https://golang.org/doc/effective_go.html#pointers_vs_values

### Дозволене використання м'ютексів (mutex) з нульовими значеннями

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
Не вставляйте м'ютекс у структуру, навіть якщо структуру не було експортовано.

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

### Обмеження копіювання зрізів (slices) та карт (maps)

Зрізи та карти містять вказівники на основні дані, тому будьте обережні зі сценаріями,
коли їх потрібно скопіювати.

#### Отримання зрізів і карт

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

#### Повернення зрізів і карт

Подібним чином, будьте обережні з модифікаціями користувачами карт або зрізів,
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

// Знімок повертає поточну статистику.
func (s *Stats) Snapshot() map[string]int {
  s.mu.Lock()
  defer s.mu.Unlock()

  return s.counters
}

// знімок більше не захищений м'ютексом, тому будь-який доступ
// до знімка підлягає гонці даних (data race).
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

### Defer для звільнення ресурсів

Використовуйте `defer` для звільнення ресурсів, таких як файли та блокування (locks).

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

### Розмір каналу (channel) дорівнює одиниці або не вказано

Канали (channels) зазвичай повинні мати розмір, який дорівнює одиниці або ж бути небуферизованими.
За замовчуванням, канали не буферизовані та мають нульовий розмір. Будь-який інший розмір
повинен ретельно контролюватися. Розглянемо, як визначається розмір, який заважає каналу
заповнюватися під навантаженням та блокує запис, і що відбувається, коли такий сценарій стався.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
// Має вистачити для всіх!
c := make(chan int, 64)
```

</td><td>

```go
// Розмір дорівнює одиниці
c := make(chan int, 1) // або
// небуферизований канал, розмір дорівнює нулю
c := make(chan int)
```

</td></tr>
</tbody></table>

### Починайте перерахування (enums) з одиниці

Стандартним способом впровадження enums у Go є оголошення власного типу
та групи `const` за допомогою `iota`. Оскільки змінні за замовчуванням мають значення, яке дорівнює 0,
ви зазвичай повинні починати свої enums з ненульових значень, наприклад з 1.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
type Operation int

const (
  Add Operation = iota
  Subtract
  Multiply
)

// Add=0, Subtract=1, Multiply=2
```

</td><td>

```go
type Operation int

const (
  Add Operation = iota + 1
  Subtract
  Multiply
)

// Add=1, Subtract=2, Multiply=3
```

</td></tr>
</tbody></table>

Бувають випадки, коли використання нульового значення має сенс,
наприклад, в ситуації, коли нульове значення є бажаною поведінкою за замовчуванням.

```go
type LogOutput int

const (
  LogToStdout LogOutput = iota
  LogToFile
  LogToRemote
)

// LogToStdout=0, LogToFile=1, LogToRemote=2
```

### Використовуйте пакет `"time"` для обробки часу

Управління часом є складною темою. Неправильні припущення, які часто пов'язанні з "часом",
припускають наступне.

1. Тривалість доби становить 24 години
2. Година має 60 хвилин
3. Тиждень має 7 днів
4. Рік має 365 днів
5. [Та багато іншого](https://infiniteundo.com/post/25326999628/falsehoods-programmers-believe-about-time)

Наприклад, *1* означає, що додавання 24 годин до певного моменту часу не гарантує,
що ви отримаєте інший календарний день.

Тому завжди використовуйте пакет [`"time"`] коли маєте справу з часом, оскільки він
допомагає впоратися з цими неправильними припущеннями безпечнішим та значно точнішим способом.

[`"time"`]: https://golang.org/pkg/time/

#### Використовуйте `time.Time` для моментів з часом

Використовуйте [`time.Time`] коли маєте справу з моментами часу, а також методи `time.Time` для
порівняння, додавання або віднімання часу.

[`time.Time`]: https://golang.org/pkg/time/#Time

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
func isActive(now, start, stop int) bool {
  return start <= now && now < stop
}
```

</td><td>

```go
func isActive(now, start, stop time.Time) bool {
  return (start.Before(now) || start.Equal(now)) && now.Before(stop)
}
```

</td></tr>
</tbody></table>

#### Використовуйте `time.Duration` для проміжків часу

Використовуйте [`time.Duration`] коли маєте справу з часовими проміжками.

[`time.Duration`]: https://golang.org/pkg/time/#Duration

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
func poll(delay int) {
  for {
    // ...
    time.Sleep(time.Duration(delay) * time.Millisecond)
  }
}

poll(10) // це були секунди чи мілісекунди?
```

</td><td>

```go
func poll(delay time.Duration) {
  for {
    // ...
    time.Sleep(delay)
  }
}

poll(10*time.Second)
```

</td></tr>
</tbody></table>

Повертаючись до прикладу з додаванням 24 годин до певного моменту часу,
метод, який ми використовуємо для додавання часу, залежить від наших намірів.
Якщо ми хочемо отримати той самий час доби, але наступного календарного дня, ми повинні
використовувати [`Time.AddDate`]. Однак, якщо ми хочемо гарантувати, що отримаємо момент часу,
зміщений на 24 години після попереднього часу, то слід використовувати [`Time.Add`].

[`Time.AddDate`]: https://golang.org/pkg/time/#Time.AddDate
[`Time.Add`]: https://golang.org/pkg/time/#Time.Add

```go
newDay := t.AddDate(0 /* роки */, 0 /* місяці */, 1 /* дні */)
maybeNewDay := t.Add(24 * time.Hour)
```

#### Використовуйте `time.Time` та `time.Duration` із зовнішніми системами

Використовуйте `time.Duration` та `time.Time` у взаємодії із зовнішніми системами, коли це можливо.
Наприклад:

- Прапори командного рядка: [`flag`] підтримують `time.Duration` через
  [`time.ParseDuration`]
- JSON: [`encoding/json`] підтримує кодування `time.Time` як [RFC 3339]
  рядок через його [`UnmarshalJSON` метод]
- SQL: [`database/sql`] підтримує перетворення `DATETIME` або `TIMESTAMP` стовпців
  в `time.Time` і назад, якщо це підтримує базовий драйвер
- YAML: [`gopkg.in/yaml.v2`] підтримує `time.Time` як [RFC 3339] рядок, та
  `time.Duration` через [`time.ParseDuration`].

  [`flag`]: https://golang.org/pkg/flag/
  [`time.ParseDuration`]: https://golang.org/pkg/time/#ParseDuration
  [`encoding/json`]: https://golang.org/pkg/encoding/json/
  [RFC 3339]: https://tools.ietf.org/html/rfc3339
  [`UnmarshalJSON` метод]: https://golang.org/pkg/time/#Time.UnmarshalJSON
  [`database/sql`]: https://golang.org/pkg/database/sql/
  [`gopkg.in/yaml.v2`]: https://godoc.org/gopkg.in/yaml.v2

Якщо неможливо використати `time.Duration` у цих взаємодіях, використовуйте
`int` або `float64` та включіть одиницю часу в назву поля.

Наприклад, оскільки `encoding/json` не підтримує `time.Duration`, одиниця часу включається в назву поля.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
// {"interval": 2}
type Config struct {
  Interval int `json:"interval"`
}
```

</td><td>

```go
// {"intervalMillis": 2000}
type Config struct {
  IntervalMillis int `json:"intervalMillis"`
}
```

</td></tr>
</tbody></table>

Якщо неможливо використовувати `time.Time` у цих взаємодіях та якщо інше не погоджено,
використовуйте `string` і форматуйте мітки часу (timestamps), як визначено в [RFC 3339].
Цей формат використовується за замовчуванням  [`Time.UnmarshalText`] та доступний
для використання в `Time.Format` і `time.Parse` через [`time.RFC3339`].

[`Time.UnmarshalText`]: https://golang.org/pkg/time/#Time.UnmarshalText
[`time.RFC3339`]: https://golang.org/pkg/time/#RFC3339

Хоча на практиці це не є проблемою, майте на увазі, що пакет `"time"` не підтримує
розбір позначок часу (timestamps) з високосними (додатковими) секундами ([8728]),
а також не враховує високосні секунди в обчисленнях ([15190]). Якщо ви порівнюєте два моменти часу,
різниця не включатиме високосні секунди, які могли відбутися між цими двома моментами.

[8728]: https://github.com/golang/go/issues/8728
[15190]: https://github.com/golang/go/issues/15190

<!-- TODO: section on String methods for enums -->

### Помилки

#### Типи помилок

Існує кілька варіантів оголошення помилок.
Перш ніж вибрати варіант, який найкраще підходить для вашого випадку, врахуйте наступне.

- Чи потрібно клієнту зіставити помилку з іншим типом помилки, щоб обробити її?
  Якщо так, нам потрібно підтримувати функції [`errors.Is`] або [`errors.As`],
  оголошуючи змінну помилки вищого рівня або власного (кастомного) типу.
- Повідомлення про помилку це статичний рядок чи динамічний, для якого потрібна
  контекстна інформація?
  Для першого ми можемо використовувати [`errors.New`], але для останнього ми повинні
  використовувати [`fmt.Errorf`] або власний тип помилки.
- Передаєте помилку з функцій, яка розташована нижче по стеку викликів?
  Тоді перегляньте [розділ про упакування помилок](#упакування помилок-wrapping).

[`errors.Is`]: https://golang.org/pkg/errors/#Is
[`errors.As`]: https://golang.org/pkg/errors/#As

| Зіставлення помилки? | Повідомлення про помилку | Рекомендація                           |
|----------------------|--------------------------|----------------------------------------|
| ні                   | статична                 | [`errors.New`]                         |
| ні                   | динамічна                | [`fmt.Errorf`]                         |
| так                  | статична                 | верхнього рівня `var` з [`errors.New`] |
| так                  | динамічна                | власний тип `error`                    |

[`errors.New`]: https://golang.org/pkg/errors/#New
[`fmt.Errorf`]: https://golang.org/pkg/fmt/#Errorf

Наприклад, використовуйте [`errors.New`] для помилки зі статичним рядком.
Якщо клієнту потрібно знайти відповідність і обробити помилку, експортуйте цю помилку
як змінну, щоб була підтримка зіставлення з `errors.Is`.

<table>
<thead><tr><th>Немає зіставлення помилки</th><th>Зіставлення помилки</th></tr></thead>
<tbody>
<tr><td>

```go
// package foo

func Open() error {
  return errors.New("could not open")
}

// package bar

if err := foo.Open(); err != nil {
  // Не можливо обробити помилку
  panic("unknown error")
}
```

</td><td>

```go
// package foo

var ErrCouldNotOpen = errors.New("could not open")

func Open() error {
  return ErrCouldNotOpen
}

// package bar

if err := foo.Open(); err != nil {
  if errors.Is(err, foo.ErrCouldNotOpen) {
    // Обробка помилки
  } else {
    panic("unknown error")
  }
}
```

</td></tr>
</tbody></table>

Для помилки з динамічним рядком, використовуйте [`fmt.Errorf`] якщо клієнту не потрібна
перевірка зіставлення або власний `error`, якщо клієнт потребує перевірки зіставлення з помилкою.

<table>
<thead><tr><th>Немає зіставлення помилки</th><th>Зіставлення помилки</th></tr></thead>
<tbody>
<tr><td>

```go
// package foo

func Open(file string) error {
  return fmt.Errorf("file %q not found", file)
}

// package bar

if err := foo.Open("testfile.txt"); err != nil {
  // Не можливо обробити помилку
  panic("unknown error")
}
```

</td><td>

```go
// package foo

type NotFoundError struct {
  File string
}

func (e *NotFoundError) Error() string {
  return fmt.Sprintf("file %q not found", e.File)
}

func Open(file string) error {
  return &NotFoundError{File: file}
}


// package bar

if err := foo.Open("testfile.txt"); err != nil {
  var notFound *NotFoundError
  if errors.As(err, &notFound) {
    // Обробка помилки
  } else {
    panic("unknown error")
  }
}
```

</td></tr>
</tbody></table>

Врахуйте, якщо ви експортуєте змінні або типи помилок із пакета,
вони стануть частиною загальнодоступного API пакета.

#### Упакування помилок (wrapping)

Є три основні способи поширення помилок у разі невдачі під час виклику:

- повернути оригінальну помилку "як є"
- додати контекст за допомогою `fmt.Errorf` та параметру `%w`
- додати контекст за допомогою `fmt.Errorf` та параметру `%v`

Повернути оригінальну помилку "як є", якщо вам не потрібно додавати додаткову контекстну інформацію.
Це зберігає вихідний тип помилки та повідомлення.
Це добре підходить для випадків, коли базове повідомлення про помилку містить
достатньо інформації, щоб визначити, звідки вона походить.

В іншому випадку додайте контекст до повідомлення про помилку, де це можливо,
щоб замість не зрозумілої помилки, як-от "підключення відмовлено", ви отримували
більш корисні помилки, наприклад "виклик служби foo: підключення відмовлено".

Використовуйте `fmt.Errorf`, щоб додати контекст до ваших помилок, вибираючи між параметрами
`%w` або `%v` залежно від того, чи повинен клієнт мати можливість знайти та виділити основну
причину.

- Використовуйте `%w`, якщо клієнт повинен мати доступ до основної помилки.
  Це хороший варіант за замовчуванням для більшості обгорнутих (wrapped) помилок,
  але майте на увазі, що клієнти можуть почати покладатися на таку поведінку.
  Тож у випадках, коли загорнута помилка є відомою `var` або типом,
  задокументуйте та протестуйте її як частину контракту вашої функції.
- Використовуйте `%v`, щоб приховати основну помилку.
  Клієнти не зможуть порівнювати з оригінальною помилкою, але ви можете перемикнутися
  на `%w` в майбутньому, якщо буде потрібно.

Додаючи контекст до помилок, зберігайте його лаконічним, уникаючи таких фраз,
як "не вдалося" ("failed to"), які стверджують очевидне та накопичуються, коли помилка просочується
крізь стек:

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
s, err := store.New()
if err != nil {
    return fmt.Errorf(
        "failed to create new store: %w", err)
}
```

</td><td>

```go
s, err := store.New()
if err != nil {
    return fmt.Errorf(
        "new store: %w", err)
}
```

</td></tr><tr><td>

```
failed to x: failed to y: failed to create new store: the error
```

</td><td>

```
x: y: new store: the error
```

</td></tr>
</tbody></table>

Однак після надсилання повідомлення про помилку в іншу систему має бути зрозуміло,
що повідомлення є помилкою (наприклад, тег `err` або префікс "Failed" в журналах).

Дивіться також [Не просто перевіряйте помилки, обробляйте їх витончено].

[`"pkg/errors".Cause`]: https://godoc.org/github.com/pkg/errors#Cause
[Не просто перевіряйте помилки, обробляйте їх витончено]: https://dave.cheney.net/2016/04/27/dont-just-check-errors-handle-them-gracefully

#### Іменування помилок

Для значень помилок, які зберігаються як глобальні змінні,
використовуйте префікс `Err` або `err` залежно від того, експортовані вони чи ні.
Ці рекомендації замінюють [Використовуйте префікс `_` для не експортованих глобальних змінних](#використовуйте-префікс-_-для-не-експортованих-глобальних-змінних).
```go
var (
  // Наступні дві помилки експортовано, щоб
  // користувачі даного пакету могли порівняти їх з errors.Is.

  ErrBrokenLink = errors.New("link is broken")
  ErrCouldNotOpen = errors.New("could not open")

  // Ця помилка не експортується, оскільки ми не хочемо
  // робити її частиною нашого загальнодоступного API.
  // Ми все ще можемо використовувати її в середині пакету з errors.Is.

  errNotFound = errors.New("not found")
)
```

Для власних типів помилок використовуйте суфікс `Error`.

```go
// Подібним чином ця помилка експортується, щоб
// користувачі даного пакету могли зіставити її з errors.As.

type NotFoundError struct {
  File string
}

func (e *NotFoundError) Error() string {
  return fmt.Sprintf("file %q not found", e.File)
}

// Ця помилка не експортується, тому що ми не хочемо
// робити її частиною публічного API.
// Ми все ще можемо використовувати її в середині пакету з errors.As.

type resolveError struct {
  Path string
}

func (e *resolveError) Error() string {
  return fmt.Sprintf("resolve %q", e.Path)
}
```

### Обробка помилок підтвердження типу

Форма [підтвердження типу] з єдиним значенням, що повертається, викличе паніку
через неправильний тип. Тому завжди використовуйте ідіому "кома добре" ("comma ok").

[підтвердження типу]: https://golang.org/ref/spec#Type_assertions

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
t := i.(string)
```

</td><td>

```go
t, ok := i.(string)
if !ok {
  // обробити помилку
}
```

</td></tr>
</tbody></table>

<!-- TODO: There are a few situations where the single assignment form is
fine. -->

### Уникайте паніки

Код, який працює у виробничому середовищі (production), повинен уникати паніки.
Паніка є основним джерелом [каскадних збоїв]. Якщо виникає помилка,
функція повинна повернути помилку та дозволити користувачу вирішити, як її обробити.

[каскадних збоїв]: https://en.wikipedia.org/wiki/Cascading_failure

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
func run(args []string) {
  if len(args) == 0 {
    panic("an argument is required")
  }
  // ...
}

func main() {
  run(os.Args[1:])
}
```

</td><td>

```go
func run(args []string) error {
  if len(args) == 0 {
    return errors.New("an argument is required")
  }
  // ...
  return nil
}

func main() {
  if err := run(os.Args[1:]); err != nil {
    fmt.Fprintln(os.Stderr, err)
    os.Exit(1)
  }
}
```

</td></tr>
</tbody></table>

Panic/recover не є стратегією обробки помилок. Програма повинна панікувати лише тоді,
коли трапляється щось непоправне, наприклад, nil розіменування (nil dereference).
Винятком є ініціалізація програми: помилки під час запуску програми, які мають
порушити роботу програми, можуть викликати паніку.

```go
var _statusTemplate = template.Must(template.New("name").Parse("_statusHTML"))
```

Навіть у тестах віддавайте перевагу `t.Fatal` або `t.FailNow` замість паніки,
щоб гарантувати, що тест буде позначено як невдалий.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
// func TestFoo(t *testing.T)

f, err := os.CreateTemp("", "test")
if err != nil {
  panic("failed to set up test")
}
```

</td><td>

```go
// func TestFoo(t *testing.T)

f, err := os.CreateTemp("", "test")
if err != nil {
  t.Fatal("failed to set up test")
}
```

</td></tr>
</tbody></table>

<!-- TODO: Explain how to use _test packages. -->

### Використовуйте go.uber.org/atomic

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

### Уникайте непостійних (mutable) глобальних змінних

Уникайте зміни глобальних змінних, натомість вибирайте впровадження залежностей (dependency injection).
Це стосується вказівників на функції, а також інших типів значень.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
// sign.go

var _timeNow = time.Now

func sign(msg string) string {
  now := _timeNow()
  return signWithTime(msg, now)
}
```

</td><td>

```go
// sign.go

type signer struct {
  now func() time.Time
}

func newSigner() *signer {
  return &signer{
    now: time.Now,
  }
}

func (s *signer) Sign(msg string) string {
  now := s.now()
  return signWithTime(msg, now)
}
```
</td></tr>
<tr><td>

```go
// sign_test.go

func TestSign(t *testing.T) {
  oldTimeNow := _timeNow
  _timeNow = func() time.Time {
    return someFixedTime
  }
  defer func() { _timeNow = oldTimeNow }()

  assert.Equal(t, want, sign(give))
}
```

</td><td>

```go
// sign_test.go

func TestSigner(t *testing.T) {
  s := newSigner()
  s.now = func() time.Time {
    return someFixedTime
  }

  assert.Equal(t, want, s.Sign(give))
}
```

</td></tr>
</tbody></table>

### Уникайте вбудовування типів (type embedding) у публічні структури

Типи, вбудовані в публічні структури, пропускають деталі реалізації,
обмежують еволюцію типів і негативно впливають на якість документації.

Якщо припустити, що ви реалізували різні типи списків за допомогою спільного
`AbstractList`, уникайте вбудовування `AbstractList` у ваші конкретні реалізації списків.
Натомість додайте до свого конкретного списку методи, які будуть делегувати
завдання методам абстрактного списку `AbstractList`.

```go
type AbstractList struct {}

// Add додає сутність до списку.
func (l *AbstractList) Add(e Entity) {
  // ...
}

// Remove видаляє сутність зі списку.
func (l *AbstractList) Remove(e Entity) {
  // ...
}
```

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
// ConcreteList - список сутностей.
type ConcreteList struct {
  *AbstractList
}
```

</td><td>

```go
// ConcreteList - список сутностей.
type ConcreteList struct {
  list *AbstractList
}

// Add додає сутність до списку.
func (l *ConcreteList) Add(e Entity) {
  l.list.Add(e)
}

// Remove видаляє сутність зі списку.
func (l *ConcreteList) Remove(e Entity) {
  l.list.Remove(e)
}
```

</td></tr>
</tbody></table>

Go дозволяє [вбудовування типу] як компроміс між наслідуванням та композицією.
Зовнішній тип отримує неявні копії методів вбудованого типу.
За замовчуванням ці методи делегують завдання методам вбудованого екземпляра.

[вбудовування типу]: https://golang.org/doc/effective_go.html#embedding

Структура також отримує поле з тим же іменем, що й тип.
Отже, якщо вбудований тип загальнодоступний, поле також буде публічним.
Для зворотної сумісності, будь-яка майбутня версія зовнішнього типу
повинна зберігати вбудований тип.

Вбудований тип рідко буває необхідним.
В основному це зручний спосіб уникнути виснажливого написання методів делегування.

Навіть вбудовування сумісного *інтерфейсу* `AbstractList` замість структури
дасть розробнику більше гнучкості для внесення змін в майбутньому,
але все одно призведе до витоку інформації про те, що конкретні списки
використовують абстрактну реалізацію.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
// AbstractList — це узагальнена реалізація для різних
// видів списків сутностей.
type AbstractList interface {
  Add(Entity)
  Remove(Entity)
}

// ConcreteList — це список сутностей.
type ConcreteList struct {
  AbstractList
}
```

</td><td>

```go
// ConcreteList — це список сутностей.
type ConcreteList struct {
  list AbstractList
}

// Add додає сутність до списку.
func (l *ConcreteList) Add(e Entity) {
  l.list.Add(e)
}

// Remove видаляє сутність зі списку.
func (l *ConcreteList) Remove(e Entity) {
  l.list.Remove(e)
}
```

</td></tr>
</tbody></table>

Чи це вбудована структура, чи вбудований інтерфейс, вбудований тип обмежує еволюцію типів.

- Додавання методів до вбудованого інтерфейсу порушує сумісність (breaking changes).
- Видалення методів із вбудованої структури порушує сумісність.
- Видалення вбудованого типу порушує сумісність.
- Заміна вбудованого типу, навіть якщо заміна відповідає тому самому інтерфейсу,
  порушує сумісність.

Попри те, що написання цих методів делегування (методи, визначені в інтерфейсі) є громіздким,
додаткові зусилля приховують деталі реалізації, залишають більше можливостей для змін,
а також усувають непрямий доступ до повного інтерфейсу List в документації.

### Уникайте використання вбудованих імен

У [специфікації мови] Go описано декілька вбудованих
[попередньо визначених ідентифікаторів], які не слід використовувати як імена в програмах Go.

Залежно від контексту повторне використання цих ідентифікаторів як імен
або приховає оригінал у межах поточної лексичної області (і всіх вкладених областях),
або призведе до заплутування коду.
У кращому випадку можна очікувати попереджень від компілятора, у гіршому випадку такий
код може створити приховані помилки, які важко помітити.

[специфікації мови]: https://golang.org/ref/spec
[попередньо визначених ідентифікаторів]: https://golang.org/ref/spec#Predeclared_identifiers

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
var error string
// `error` приховує таке ж саме вбудоване ім'я 

// або

func handleErrorMessage(error string) {
    // `error` приховує вбудоване ім'я
}
```

</td><td>

```go
var errorMessage string
// `error` відноситься до вбудованого імені

// або

func handleErrorMessage(msg string) {
    // `error` відноситься до вбудованого імені
}
```

</td></tr>
<tr><td>

```go
type Foo struct {
    // Хоча ці поля технічно не приховують
    // вбудовані імена, пошук для рядків `error` або` string`
    // дає неоднозначні результатти.
    error  error
    string string
}

func (f Foo) Error() error {
    // `error` та `f.error` візуально схожі
    return f.error
}

func (f Foo) String() string {
    // `string` та `f.string` візуально схожі
    return f.string
}
```

</td><td>

```go
type Foo struct {
    // Рядки `error` and `string` тепер однозначні.
    err error
    str string
}

func (f Foo) Error() error {
    return f.err
}

func (f Foo) String() string {
    return f.str
}
```

</td></tr>
</tbody></table>


Зауважте, що компілятор не генеруватиме помилок під час використання попередньо оголошених
ідентифікаторів, але такі інструменти, як `go vet`, мають правильно вказувати на ці та інші
випадки приховування вбудованих імен.

### Уникайте `init()`

Уникайте `init()` де це можливо. Якщо використання `init()` неминуче або ж бажане,
ваш код повинен:

1. Бути повністю детермінованим, незалежно від програмного середовища чи виклику.
2. Уникайте залежності від порядку або побічних ефектів інших функцій `init()`.
   Хоча порядок виклику `init()` добре відомий, код може змінюватися, і відповідно зв'язки
   між функціями `init()` можуть зробити код крихким та схильним до помилок.
3. Уникайте доступу або маніпулювання глобальним станом або станом середовища,
   таким як інформація про машину, змінні середовища, робочий каталог,
   аргументи виклику, вхідні дані програми тощо.
4. Уникайте операцій вводу-виводу (I/O), включаючи файлову систему, мережу та системні виклики.

Код, який не відповідає наведеним вище вимогам, швидше за все, буде допоміжним кодом,
який буде викликатися як частина `main()` (або в іншому місці життєвого циклу програми),
або буде написаний як частина самого `main()`.
Зокрема, бібліотеки, що призначені для використання іншими програмами, повинні бути
особливо обережними, щоб бути повністю детермінованими та не виконувати "магію ініціалізації".

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
type Foo struct {
    // ...
}

var _defaultFoo Foo

func init() {
    _defaultFoo = Foo{
        // ...
    }
}
```

</td><td>

```go
var _defaultFoo = Foo{
    // ...
}

// або так, краще для тестування:

var _defaultFoo = defaultFoo()

func defaultFoo() Foo {
    return Foo{
        // ...
    }
}
```

</td></tr>
<tr><td>

```go
type Config struct {
    // ...
}

var _config Config

func init() {
    // Погано: на основі поточного каталогу
    cwd, _ := os.Getwd()

    // Погано: I/O
    raw, _ := os.ReadFile(
        path.Join(cwd, "config", "config.yaml"),
    )

    yaml.Unmarshal(raw, &_config)
}
```

</td><td>

```go
type Config struct {
    // ...
}

func loadConfig() Config {
    cwd, err := os.Getwd()
    // обробка помилки

    raw, err := os.ReadFile(
        path.Join(cwd, "config", "config.yaml"),
    )
    // обробка помилки

    var config Config
    yaml.Unmarshal(raw, &config)

    return config
}
```

</td></tr>
</tbody></table>

Враховуючи вищезазначене, деякі ситуації, в яких `init()` може бути кращим або необхідним,
можуть включати:

- Складні вирази, які не можна представити як окреме призначення.
- Підключаються хуки, такі як діалекти `database/sql`, реєстри типів кодування тощо.
- Оптимізація для [Google Cloud Functions] та інших форм детермінованого попереднього обчислення.

  [Google Cloud Functions]: https://cloud.google.com/functions/docs/bestpractices/tips#use_global_variables_to_reuse_objects_in_future_invocations

### Вихід в Main

Програми Go використовують [`os.Exit`] або [`log.Fatal*`] для негайного виходу.
(Паніка не є хорошим способом виходу з програм, будь ласка, [не панікуйте](#dont-panic).)

[`os.Exit`]: https://golang.org/pkg/os/#Exit
[`log.Fatal*`]: https://golang.org/pkg/log/#Fatal

Викликайте `os.Exit` або `log.Fatal*` **лише в `main()`**.
Усі інші функції повинні повертати помилки, щоб повідомляти про збій.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
func main() {
  body := readFile(path)
  fmt.Println(body)
}

func readFile(path string) string {
  f, err := os.Open(path)
  if err != nil {
    log.Fatal(err)
  }

  b, err := io.ReadAll(f)
  if err != nil {
    log.Fatal(err)
  }

  return string(b)
}
```

</td><td>

```go
func main() {
  body, err := readFile(path)
  if err != nil {
    log.Fatal(err)
  }
  fmt.Println(body)
}

func readFile(path string) (string, error) {
  f, err := os.Open(path)
  if err != nil {
    return "", err
  }

  b, err := io.ReadAll(f)
  if err != nil {
    return "", err
  }

  return string(b), nil
}
```

</td></tr>
</tbody></table>

Обґрунтування: програми з кількома функціями, які містять вихід (exit), викликають кілька проблем:

- Неочевидний потік керування: будь-яка функція може вийти з програми,
  тому стає важко міркувати про потік керування.
- Важко тестувати: функція, яка виходить з програми, також вийде із тесту, який її викликає.
  Це ускладнює тестування функції та створює ризик пропуску інших тестів,
  які ще не були запущені `go test`.
- Пропущене звільнення ресурсів: коли функція виходить з програми, вона пропускає виклики функцій,
  поставлених у чергу з операторами `defer`. Це збільшує ризик пропуску важливих завдань очищення
  (звільнення ресурсів).

#### Виходьте один раз

Якщо можливо, надайте перевагу виклику `os.Exit` або `log.Fatal` **не більше одного разу**
у вашій функції `main()`. Якщо існує кілька сценаріїв помилок, які зупиняють виконання програми,
помістіть цю логіку в окрему функцію та вже з неї повертайте помилки.

Це призводить до скорочення вашої функції `main()` та тримає всю ключову бізнес-логіку
в окремій функції, яку можна протестувати.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
package main

func main() {
  args := os.Args[1:]
  if len(args) != 1 {
    log.Fatal("missing file")
  }
  name := args[0]

  f, err := os.Open(name)
  if err != nil {
    log.Fatal(err)
  }
  defer f.Close()

  // Якщо ми викличемо log.Fatal після цього рядка,
  // f.Close не буде викликано.

  b, err := io.ReadAll(f)
  if err != nil {
    log.Fatal(err)
  }

  // ...
}
```

</td><td>

```go
package main

func main() {
  if err := run(); err != nil {
    log.Fatal(err)
  }
}

func run() error {
  args := os.Args[1:]
  if len(args) != 1 {
    return errors.New("missing file")
  }
  name := args[0]

  f, err := os.Open(name)
  if err != nil {
    return err
  }
  defer f.Close()

  b, err := io.ReadAll(f)
  if err != nil {
    return err
  }

  // ...
}
```

</td></tr>
</tbody></table>

### Використовуйте теги полів у серіалізованих структурах

Будь-яке поле структури, серіалізоване в JSON, YAML або інші формати,
які підтримують іменування полів на основі тегів, має бути анотовано відповідним тегом.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
type Stock struct {
  Price int
  Name  string
}

bytes, err := json.Marshal(Stock{
  Price: 137,
  Name:  "UBER",
})
```

</td><td>

```go
type Stock struct {
  Price int    `json:"price"`
  Name  string `json:"name"`
  // Можна безпечно перейменувати Name на Symbol.
}

bytes, err := json.Marshal(Stock{
  Price: 137,
  Name:  "UBER",
})
```

</td></tr>
</tbody></table>

Обґрунтування:
Серіалізована форма структури є контрактом між різними системами.
Зміни в структурі серіалізованої форми, включаючи імена полів, порушують цей контракт.
Додання імен полів всередині тегів, дозволяє зробити контракт явним та захищеним від
випадкового порушення контракту шляхом рефакторингу або перейменування полів.

### Don't fire-and-forget goroutines

Goroutines are lightweight, but they're not free:
at minimum, they cost memory for their stack and CPU to be scheduled.
While these costs are small for typical uses of goroutines,
they can cause significant performance issues
when spawned in large numbers without controlled lifetimes.
Goroutines with unmanaged lifetimes can also cause other issues
like preventing unused objects from being garbage collected
and holding onto resources that are otherwise no longer used.

Therefore, do not leak goroutines in production code.
Use [go.uber.org/goleak](https://pkg.go.dev/go.uber.org/goleak)
to test for goroutine leaks inside packages that may spawn goroutines.

In general, every goroutine:

- must have a predictable time at which it will stop running; or
- there must be a way to signal to the goroutine that it should stop

In both cases, there must be a way code to block and wait for the goroutine to
finish.

For example:

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
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
  stop = make(chan struct{}) // tells the goroutine to stop
  done = make(chan struct{}) // tells us that the goroutine exited
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

// Elsewhere...
close(stop)  // signal the goroutine to stop
<-done       // and wait for it to exit
```

</td></tr>
<tr><td>

There's no way to stop this goroutine.
This will run until the application exits.

</td><td>

This goroutine can be stopped with `close(stop)`,
and we can wait for it to exit with `<-done`.

</td></tr>
</tbody></table>

#### Wait for goroutines to exit

Given a goroutine spawned by the system,
there must be a way to wait for the goroutine to exit.
There are two popular ways to do this:

- Use a `sync.WaitGroup`.
  Do this if there are multiple goroutines that you want to wait for

    ```go
    var wg sync.WaitGroup
    for i := 0; i < N; i++ {
      wg.Add(1)
      go func() {
        defer wg.Done()
        // ...
      }()
    }
    
    // To wait for all to finish:
    wg.Wait()
    ```

- Add another `chan struct{}` that the goroutine closes when it's done.
  Do this if there's only one goroutine.

    ```go
    done := make(chan struct{})
    go func() {
      defer close(done)
      // ...
    }()
    
    // To wait for the goroutine to finish:
    <-done
    ```

#### No goroutines in `init()`

`init()` functions should not spawn goroutines.
See also [Avoid init()](#avoid-init).

If a package has need of a background goroutine,
it must expose an object that is responsible for managing a goroutine's
lifetime.
The object must provide a method (`Close`, `Stop`, `Shutdown`, etc)
that signals the background goroutine to stop, and waits for it to exit.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
func init() {
  go doWork()
}

func doWork() {
  for {
    // ...
  }
}
```

</td><td>

```go
type Worker struct{ /* ... */ }

func NewWorker(...) *Worker {
  w := &Worker{
    stop: make(chan struct{}),
    done: make(chan struct{}),
    // ...
  }
  go w.doWork()
}

func (w *Worker) doWork() {
  defer close(w.done)
  for {
    // ...
    case <-w.stop:
      return
  }
}

// Shutdown tells the worker to stop
// and waits until it has finished.
func (w *Worker) Shutdown() {
  close(w.stop)
  <-w.done
}
```

</td></tr>
<tr><td>

Spawns a background goroutine unconditionally when the user exports this package.
The user has no control over the goroutine or a means of stopping it.

</td><td>

Spawns the worker only if the user requests it.
Provides a means of shutting down the worker so that the user can free up
resources used by the worker.

Note that you should use `WaitGroup`s if the worker manages multiple
goroutines.
See [Wait for goroutines to exit](#wait-for-goroutines-to-exit).


</td></tr>
</tbody></table>

## Продуктивність

Інструкції щодо продуктивності застосовуються лише до так званого "hot path"
(шляхи виконання коду, де витрачається більша частина часу виконання і які можуть
виконуватися дуже часто).

### Надавайте перевагу `strconv` замість `fmt`

При конвертації типів в рядки/з рядків `strconv` швидше, ніж `fmt`.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
for i := 0; i < b.N; i++ {
  s := fmt.Sprint(rand.Int())
}
```

</td><td>

```go
for i := 0; i < b.N; i++ {
  s := strconv.Itoa(rand.Int())
}
```

</td></tr>
<tr><td>

```
BenchmarkFmtSprint-4    143 ns/op    2 allocs/op
```

</td><td>

```
BenchmarkStrconv-4    64.2 ns/op    1 allocs/op
```

</td></tr>
</tbody></table>

### Уникайте конвертації string-to-byte

Не створюйте зріз байтів із фіксованого рядка декілька разів.
Натомість виконайте конвертування один раз і збережіть результат.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
for i := 0; i < b.N; i++ {
  w.Write([]byte("Hello world"))
}
```

</td><td>

```go
data := []byte("Hello world")
for i := 0; i < b.N; i++ {
  w.Write(data)
}
```

</td></tr>
<tr><td>

```
BenchmarkBad-4   50000000   22.2 ns/op
```

</td><td>

```
BenchmarkGood-4  500000000   3.25 ns/op
```

</td></tr>
</tbody></table>

### Намагайтесь вказувати місткість (capacity) контейнера

Де це можливо, вказуйте місткість контейнера, щоб наперед виділити відповідний обсяг пам'яті.
Це мінімізує подальші виділення пам'яті (allocations) у міру додавання нових елементів
(шляхом копіювання та зміни розміру контейнера).

#### Вказуйте місткість для карт

Якщо це можливо, вкажіть місткість під час ініціалізації карт за допомогою `make()`.

```go
make(map[T1]T2, hint)
```

Задання місткості під час виклику `make()` намагається підібрати правильний розмір карти
під час її ініціалізації, що зменшує кількість операцій виділення пам'яті під час
додання нових елементів до карти.

Пам'ятайте, що, на відміну від зрізів, місткість для карт не гарантує
повного та остаточного виділення пам'яті, а використовуються для приблизної кількості
необхідних сегментів хеш-карти.
Отже, виділення додаткової пам'яті все одно можуть відбуватися під час додавання
елементів до карти, навіть до вказаної наперед місткості.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
m := make(map[string]os.FileInfo)

files, _ := os.ReadDir("./files")
for _, f := range files {
    m[f.Name()] = f
}
```

</td><td>

```go

files, _ := os.ReadDir("./files")

m := make(map[string]os.DirEntry, len(files))
for _, f := range files {
    m[f.Name()] = f
}
```

</td></tr>
<tr><td>

`m` створюється без вказування розміру; може бути більше розподілів під час призначення.

</td><td>

`m` створюється із вказуванням розміру; може бути менше розподілів під час призначення.

</td></tr>
</tbody></table>

#### Вказуйте місткість для зрізів

Якщо це можливо, вказуйте місткість зрізів під час ініціалізації за допомогою `make()`,
особливо при додаванні.
```go
make([]T, length, capacity)
```

На відміну від карт, для місткості зрізів компілятор виділить достатньо
пам'яті, скільки було вказано в `make()`. Це означає, що всі наступні операції `append()`
не вимагатимуть виділення додаткової пам'яті (допоки довжина зрізу відповідає місткості,
після чого будь-які додавання вимагатимуть зміни розміру, для додавання нових елементів).

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
for n := 0; n < b.N; n++ {
  data := make([]int, 0)
  for k := 0; k < size; k++{
    data = append(data, k)
  }
}
```

</td><td>

```go
for n := 0; n < b.N; n++ {
  data := make([]int, 0, size)
  for k := 0; k < size; k++{
    data = append(data, k)
  }
}
```

</td></tr>
<tr><td>

```
BenchmarkBad-4    100000000    2.48s
```

</td><td>

```
BenchmarkGood-4   100000000    0.21s
```

</td></tr>
</tbody></table>

## Стиль

### Уникайте надто довгих рядків

Уникайте рядків коду, які вимагають від читачів горизонтальної прокрутки
або надто сильного повороту голови.

Ми рекомендуємо обмежити довжину м'якого рядка (soft line) до **99 символів**.
Автори повинні прагнути до того, щоб не виходити за рамки цих обмежень,
хоча ці обмеження не є жорсткими. Код може перевищувати цю межу.

### Будьте послідовними

Деякі з настанов, викладених у цьому документі, можна оцінити об'єктивно;
інші є ситуативними, контекстними або суб'єктивними.

Перш за все, **будьте послідовними**.

Послідовний код легше підтримувати та раціоналізувати,
він потребує менше когнітивних витрат і його легше переносити чи оновлювати,
коли з'являються нові угоди або виправляються класи помилок.

І навпаки, наявність кількох різнорідних або потенційно конфліктних стилів в одній
кодовій базі спричиняє накладні витрати на технічне обслуговування, невизначеність
і когнітивний дисонанс. Усе це може безпосередньо сприяти зниженню швидкості,
болісним перевіркам коду та помилкам.

Застосовуючи ці вказівки до кодової бази, рекомендується вносити зміни на рівні пакета
(або більше): застосування на рівні під-пакету порушує вищезазначені проблеми,
додаючи кілька стилів до одного коду.

### Групуйте схожі оголошення

Go підтримує групування схожих декларацій.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
import "a"
import "b"
```

</td><td>

```go
import (
  "a"
  "b"
)
```

</td></tr>
</tbody></table>

Це також стосується констант, змінних і оголошень типів.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go

const a = 1
const b = 2



var a = 1
var b = 2



type Area float64
type Volume float64
```

</td><td>

```go
const (
  a = 1
  b = 2
)

var (
  a = 1
  b = 2
)

type (
  Area float64
  Volume float64
)
```

</td></tr>
</tbody></table>

Групуйте лише пов'язані оголошення. Не групуйте декларації, які не мають нічого спільного.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
type Operation int

const (
  Add Operation = iota + 1
  Subtract
  Multiply
  EnvVar = "MY_ENV"
)
```

</td><td>

```go
type Operation int

const (
  Add Operation = iota + 1
  Subtract
  Multiply
)

const EnvVar = "MY_ENV"
```

</td></tr>
</tbody></table>

Групи не мають обмежень щодо місця використання.
Наприклад, ви можете використовувати їх всередині функцій.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
func f() string {
  red := color.New(0xff0000)
  green := color.New(0x00ff00)
  blue := color.New(0x0000ff)

  // ...
}
```

</td><td>

```go
func f() string {
  var (
    red   = color.New(0xff0000)
    green = color.New(0x00ff00)
    blue  = color.New(0x0000ff)
  )

  // ...
}
```

</td></tr>
</tbody></table>

Виняток: оголошення змінних, особливо всередині функцій, повинні бути згруповані разом,
якщо вони оголошені поруч з іншими змінними.
Зробіть це для змінних, оголошених разом, навіть якщо вони не мають нічого спільного.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
func (c *client) request() {
  caller := c.name
  format := "json"
  timeout := 5*time.Second
  var err error

  // ...
}
```

</td><td>

```go
func (c *client) request() {
  var (
    caller  = c.name
    format  = "json"
    timeout = 5*time.Second
    err error
  )

  // ...
}
```

</td></tr>
</tbody></table>

### Порядок імпортування бібліотек

Повинно бути дві групи імпорту:

- Стандартна бібліотека
- Все інше

Це групування, застосоване `goimports` за замовчуванням.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
import (
  "fmt"
  "os"
  "go.uber.org/atomic"
  "golang.org/x/sync/errgroup"
)
```

</td><td>

```go
import (
  "fmt"
  "os"

  "go.uber.org/atomic"
  "golang.org/x/sync/errgroup"
)
```

</td></tr>
</tbody></table>

### Назви пакетів

При іменуванні пакетів, оберіть таку назву:

- Все з малих літер. Без великих літер і підкреслень.
- Не потрібно перейменовувати за допомогою іменованого імпорту на більшості викликів сайтів.
- Коротко і лаконічно. Пам'ятайте, що ім'я вказується повністю на кожному виклику сайту.
- Не множини. Наприклад, `net/url`, а не `net/urls`.
- Не "common", "util", "shared" або "lib". Це погані, не інформативні назви.

Дивіться також [Package Names] та [Style guideline for Go packages].

[Package Names]: https://blog.golang.org/package-names
[Style guideline for Go packages]: https://rakyll.org/style-packages/

### Назви функцій

Ми дотримуємося конвенцій спільноти Go щодо використання [MixedCaps для імен функцій].
Виняток зроблено для тестових функцій, які можуть містити підкреслення з метою групування
пов'язаних тестів, наприклад, `TestMyFunction_WhatIsBeingTested`.

[MixedCaps для імен функцій]: https://golang.org/doc/effective_go.html#mixed-caps

### Імпорт псевдонімів

Псевдонім імпорту слід використовувати, якщо назва пакета не збігається з останнім
елементом шляху імпорту.

```go
import (
  "net/http"

  client "example.com/client-go"
  trace "example.com/trace/v2"
)
```

У всіх інших сценаріях слід уникати псевдонімів імпорту, якщо немає прямого конфлікту між імпортами.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
import (
  "fmt"
  "os"


  nettrace "golang.net/x/trace"
)
```

</td><td>

```go
import (
  "fmt"
  "os"
  "runtime/trace"

  nettrace "golang.net/x/trace"
)
```

</td></tr>
</tbody></table>

### Групування та впорядкування функцій

- Функції мають бути відсортовані в приблизному порядку викликів.
- Функції у файлі повинні бути згруповані відповідно до отримувача.

Таким чином, експортовані функції повинні з'явитися у файлі першими, одразу після визначень
`struct`, `const` та `var`.

Функції `newXYZ()`/`NewXYZ()` можуть з'явитися після визначення типу,
але перед рештою методів одержувача.

Оскільки функції згруповані за одержувачами, звичайні службові функції мають з'являтися
в кінці файлу.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
func (s *something) Cost() {
  return calcCost(s.weights)
}

type something struct{ ... }

func calcCost(n []int) int {...}

func (s *something) Stop() {...}

func newSomething() *something {
    return &something{}
}
```

</td><td>

```go
type something struct{ ... }

func newSomething() *something {
    return &something{}
}

func (s *something) Cost() {
  return calcCost(s.weights)
}

func (s *something) Stop() {...}

func calcCost(n []int) int {...}
```

</td></tr>
</tbody></table>

### Зменште вкладеність

Потрібно зменшувати вкладеність де це можливо, спочатку обробляючи випадки
помилок або спеціальні умови та повертати результат раніше або продовжувати цикл.
Зменште кількість коду вкладеного на кілька рівнів.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
for _, v := range data {
  if v.F1 == 1 {
    v = process(v)
    if err := v.Call(); err == nil {
      v.Send()
    } else {
      return err
    }
  } else {
    log.Printf("Invalid v: %v", v)
  }
}
```

</td><td>

```go
for _, v := range data {
  if v.F1 != 1 {
    log.Printf("Invalid v: %v", v)
    continue
  }

  v = process(v)
  if err := v.Call(); err != nil {
    return err
  }
  v.Send()
}
```

</td></tr>
</tbody></table>

### Зайвий оператор else

Якщо змінна встановлена в обох частинах if, її можна замінити одним if.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
var a int
if b {
  a = 100
} else {
  a = 10
}
```

</td><td>

```go
a := 10
if b {
  a = 100
}
```

</td></tr>
</tbody></table>

### Оголошення змінних верхнього рівня

На верхньому рівні (рівні файлу) використовуйте стандартне ключове слово `var`.
Не вказуйте тип, за винятком випадків, коли вираз не збігається з типом.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
var _s string = F()

func F() string { return "A" }
```

</td><td>

```go
var _s = F()
// Оскільки ф-ція F вже вказує, що повертає рядок,
// нам не потрібно вказувати тип знову.

func F() string { return "A" }
```

</td></tr>
</tbody></table>

Вкажіть тип, якщо тип виразу не відповідає бажаному типу.

```go
type myError struct{}

func (myError) Error() string { return "error" }

func F() myError { return myError{} }

var _e error = F()
// F повертає об'єкт типу myError, але ми хочемо повернути error.
```

### Використовуйте префікс `_` для не експортованих глобальних змінних

До не експортованих змінних `var` та констант `const` верхнього рівня додайте префікс `_`,
щоб під час їх використання було зрозуміло, що вони є глобальними символами.

Пояснення: змінні та константи верхнього рівня мають область видимості всього пакету.
Використання загальних імен дозволяє випадкове використання неправильного значення
в іншому файлі того ж пакету.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
// foo.go

const (
  defaultPort = 8080
  defaultUser = "user"
)

// bar.go

func Bar() {
  defaultPort := 9090
  ...
  fmt.Println("Default port", defaultPort)

  // Ми не побачимо помилку компіляції, якщо перший рядок
  // Bar() буде видалено.
}
```

</td><td>

```go
// foo.go

const (
  _defaultPort = 8080
  _defaultUser = "user"
)
```

</td></tr>
</tbody></table>

**Виняток**: не експортовані значення помилок повинні мати префікс `err` без підкреслення.
Див. [Іменування помилок](#іменування-помилок).

### Вбудовування в структури (embedding)

Вбудовані типи повинні бути у верхній частині списку полів структури,
також повинен бути порожній рядок, який відокремлює вбудовані поля від звичайних полів.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
type Client struct {
  version int
  http.Client
}
```

</td><td>

```go
type Client struct {
  http.Client

  version int
}
```

</td></tr>
</tbody></table>

Вбудовування має забезпечувати відчутні переваги, як-от додавання або розширення функціональності
семантично прийнятним способом. Це повинно робитись без негативних наслідків для користувача
(див. також: [Уникайте вбудовування типів у публічні структури]).

Виняток: м'ютекси (mutex) не можна вбудовувати, навіть у не експортовані типи.
Дивіться також: [М'ютекси (mutex) з нульовими значеннями правильні].

[Уникайте вбудовування типів у публічні структури]: #уникайте-вбудовування-типів-у-публічні-структури
[М'ютекси (mutex) з нульовими значеннями правильні]: #Мютекси-mutex-з-нульовими-значеннями-правильні

Вбудовування **не повинно**:

- Бути суто косметичними або орієнтованими лише на зручність.
- Робити зовнішні типи більш складними для створення або використання.
- Впливати на нульові значення зовнішніх типів.
  Якщо зовнішній тип має корисне нульове значення, то вбудовування внутрішнього типу не повинно
  це змінити.
- Робити публічними функції або поля внутрішнього типу, які жодним чином не пов'язані із зовнішнім типом.
- Розкривати не експортовані типи.
- Впливати на семантику копіювання зовнішніх типів.
- Змінювати API або семантику зовнішнього типу.
- Вставляти неканонічну форму внутрішнього типу.
- Розкривати деталі реалізації зовнішнього типу.
- Дозволяти користувачам спостерігати або контролювати внутрішні елементи.
- Змінювати загальну поведінку внутрішніх функцій, обгорнувши неочікуваними для користувача способами.

Простіше кажучи, робіть вбудовування свідомо та цілеспрямовано.
Хорошим лакмусовим папірцем є запитати себе:
"чи всі ці експортовані внутрішні методи/поля будуть додані безпосередньо до зовнішнього типу?";
якщо відповідь "деякі" або "ні", не вставляйте внутрішній тип, замість цього використовуйте поле.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
type A struct {
    // Погано: A.Lock() і A.Unlock() тепер доступні,
    //         не забезпечують жодних функціональних переваг
    //         і дозволяють користувачам контролювати деталі
    //         внутрішніх елементів A.
    sync.Mutex
}
```

</td><td>

```go
type countingWriteCloser struct {
    // Добре: Write() надається на цьому зовнішньому рівні
    //        для певної мети та делегує роботу 
    //        до внутрішнього типу Write().
    io.WriteCloser

    count int
}

func (w *countingWriteCloser) Write(bs []byte) (int, error) {
    w.count += len(bs)
    return w.WriteCloser.Write(bs)
}
```

</td></tr>
<tr><td>

```go
type Book struct {
    // Погано: вказівник змінює корисне нульове значення
    io.ReadWriter

    // інші поля
}

// пізніше

var b Book
b.Read(...)  // panic: вказівник nil
b.String()   // panic: вказівник nil
b.Write(...) // panic: вказівник nil
```

</td><td>

```go
type Book struct {
    // Добре: має корисне нульове значення
    bytes.Buffer

    // інші поля
}

// пізніше

var b Book
b.Read(...)  // ok
b.String()   // ok
b.Write(...) // ok
```

</td></tr>
<tr><td>

```go
type Client struct {
    sync.Mutex
    sync.WaitGroup
    bytes.Buffer
    url.URL
}
```

</td><td>

```go
type Client struct {
    mtx sync.Mutex
    wg  sync.WaitGroup
    buf bytes.Buffer
    url url.URL
}
```

</td></tr>
</tbody></table>

### Оголошення локальних змінних

Короткі оголошення змінних (`:=`) повинні використовуватися, якщо змінна має явне значення.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
var s = "foo"
```

</td><td>

```go
s := "foo"
```

</td></tr>
</tbody></table>

Однак існують випадки, коли значення за замовчуванням виглядає зрозумілішим,
якщо використовується ключове слово `var`. Наприклад, [Оголошення порожніх зрізів].

[Оголошення порожніх зрізів]: https://github.com/golang/go/wiki/CodeReviewComments#declaring-empty-slices

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
func f(list []int) {
  filtered := []int{}
  for _, v := range list {
    if v > 10 {
      filtered = append(filtered, v)
    }
  }
}
```

</td><td>

```go
func f(list []int) {
  var filtered []int
  for _, v := range list {
    if v > 10 {
      filtered = append(filtered, v)
    }
  }
}
```

</td></tr>
</tbody></table>

### nil це повноцінний зріз

`nil` - дійсне значення зрізу нульової довжини. Це означає, що,

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
  // або, nums := make([]int)

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

Пам'ятайте, що хоча nil є дійсним зрізом, але це не еквівалентно зрізу, якому задано нульову довжину.
Перший зріз nil, а другий ні – тому в різних ситуаціях вони можуть розглядатися по-різному
(наприклад, серіалізація).

### Зменште область видимості змінних

Де це можливо, зменшуйте область видимості змінних.
Дане правило не повинно суперечити наступному [Зменште вкладення](#зменште-вкладення).

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
err := os.WriteFile(name, data, 0644)
if err != nil {
 return err
}
```

</td><td>

```go
if err := os.WriteFile(name, data, 0644); err != nil {
 return err
}
```

</td></tr>
</tbody></table>

Якщо вам потрібен результат функції поза межами if, тоді вам не слід намагатися зменшувати область видимості.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
if data, err := os.ReadFile(name); err == nil {
  err = cfg.Decode(data)
  if err != nil {
    return err
  }

  fmt.Println(cfg)
  return nil
} else {
  return err
}
```

</td><td>

```go
data, err := os.ReadFile(name)
if err != nil {
   return err
}

if err := cfg.Decode(data); err != nil {
  return err
}

fmt.Println(cfg)
return nil
```

</td></tr>
</tbody></table>

### Уникайте відкритих параметрів

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

### Використовуйте необроблені рядкові літерали, щоб уникнути екранування

Go підтримує [необроблені рядкові літерали (raw string literals)](https://golang.org/ref/spec#raw_string_lit),
які можуть охоплювати кілька рядків та містити лапки.
Використовуйте їх, щоб уникнути ручного додавання екранованих символів, яке значно погіршує читабельність.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
wantError := "unknown name:\"test\""
```

</td><td>

```go
wantError := `unknown error:"test"`
```

</td></tr>
</tbody></table>

### Ініціалізація структур

#### Використовуйте імена полів для ініціалізації структур

Ви майже завжди повинні вказувати імена полів під час ініціалізації структур.
Це обов'язково при використанні [`go vet`].

[`go vet`]: https://golang.org/cmd/vet/

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>


```go
k := User{"John", "Doe", true}
```

</td><td>

```go
k := User{
    FirstName: "John",
    LastName: "Doe",
    Admin: true,
}
```

</td></tr>
</tbody></table>

Виняток: назви полів *можна* опускати в тестових таблицях, якщо є 3 або менше полів.

```go
tests := []struct{
  op Operation
  want string
}{
  {Add, "add"},
  {Subtract, "subtract"},
}
```

#### Пропускайте поля з нульовими значеннями в структурах

Під час ініціалізації структур з іменами полів, не додавайте поля,
які мають нульові значення, якщо вони не несуть змістовний контекст.
В іншому випадку дозвольте Go автоматично встановити ці поля з нульовими значеннями.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
user := User{
  FirstName: "John",
  LastName: "Doe",
  MiddleName: "",
  Admin: false,
}
```

</td><td>

```go
user := User{
  FirstName: "John",
  LastName: "Doe",
}
```

</td></tr>
</tbody></table>

Це допомагає зменшити шум для читачів, пропускаючи значення за замовчуванням у цьому контексті.
Вказуються лише поля, які мають значення.

Додайте нульові значення, якщо назви полів надають змістовний контекст.
Наприклад, тестові приклади в [тестових таблицях](#тестові-таблиці) можуть бути корисними з іменами полів,
навіть якщо вони мають нульове значення.

```go
tests := []struct{
  give string
  want int
}{
  {give: "0", want: 0},
  // ...
}
```

#### Використовуйте `var` для структур з нульовим значенням

Якщо всі поля структури опущені в декларації, використовуйте форму `var`,
щоб оголосити структуру.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
user := User{}
```

</td><td>

```go
var user User
```

</td></tr>
</tbody></table>

Це відрізняє структури з нульовим значенням від структур з ненульовими полями,
подібно до розрізнення, створеного для [ініціалізації карти], і відповідає тому,
як ми вважаємо за краще [оголошувати порожні зрізи][Оголошення порожніх зрізів].

[ініціалізації карти]: #ініціалізація-карт-maps

#### Ініціалізація структурних посилань
Declaring Empty Slices
Використовуйте `&T{}` замість `new(T)` під час ініціалізації посилань на структуру,
щоб це було відповідно до ініціалізації структури.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
sval := T{Name: "foo"}

// не відповідний
sptr := new(T)
sptr.Name = "bar"
```

</td><td>

```go
sval := T{Name: "foo"}

sptr := &T{Name: "bar"}
```

</td></tr>
</tbody></table>

### Ініціалізація карт (maps)

Надавайте перевагу функції `make(..)` для порожніх карт і карт, що заповнені програмно.
Це робить ініціалізацію карт візуально відмінним від оголошення,
а також дозволяє простіше вказувати розмір карти пізніше, якщо доступно.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
var (
  // m1 безпечний для читання та запису;
  // m2 "panic" під час запису.
  m1 = map[T1]T2{}
  m2 map[T1]T2
)
```

</td><td>

```go
var (
  // m1 безпечний для читання та запису;
  // m2 "panic" під час запису;
  m1 = make(map[T1]T2)
  m2 map[T1]T2
)
```

</td></tr>
<tr><td>

Оголошення та ініціалізація візуально схожі.

</td><td>

Оголошення та ініціалізація візуально відрізняються.

</td></tr>
</tbody></table>

Якщо це можливо, вказуйте місткість під час ініціалізації карт за допомогою функції `make()`.
Додаткову інформацію дивіться в розділі [Вказуйте місткість для карт](#вказуйте-місткість-для-карт).

З іншого боку, якщо карта містить фіксований список елементів,
використовуйте літерали карти, щоб ініціалізувати карту.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
m := make(map[T1]T2, 3)
m[k1] = v1
m[k2] = v2
m[k3] = v3
```

</td><td>

```go
m := map[T1]T2{
  k1: v1,
  k2: v2,
  k3: v3,
}
```

</td></tr>
</tbody></table>

Основне правило полягає в тому, щоб використовувати літерали карт під час
додавання фіксованого набору елементів при ініціалізації, інакше використовуйте
функцію `make` та вкажіть місткість (capacity), якщо вона доступна.

### Форматування рядків поза Printf

Якщо ви оголошуєте форматування рядків для функцій у стилі `Printf` поза рядковим літералом,
винесіть їх значення в `const`.

Це допомагає `go vet` виконувати статичний аналіз форматування рядка.

<table>
<thead><tr><th>Не рекомендовано</th><th>Рекомендовано</th></tr></thead>
<tbody>
<tr><td>

```go
msg := "unexpected values %v, %v\n"
fmt.Printf(msg, 1, 2)
```

</td><td>

```go
const msg = "unexpected values %v, %v\n"
fmt.Printf(msg, 1, 2)
```

</td></tr>
</tbody></table>

### Назви функцій у стилі Printf

Коли ви оголошуєте функцію у стилі `Printf`, переконайтеся, що `go vet` може її виявити
та перевірити форматування рядка.

Це означає, що ви повинні використовувати попередньо визначені назви функцій
у стилі `Printf`, якщо це можливо. `go vet` перевірить їх за замовчуванням.
Для отримання додаткової інформації дивіться [Printf family].

[Printf family]: https://golang.org/cmd/vet/#hdr-Printf_family

Якщо використання попередньо визначених імен неможливе, завершіть вибране ім'я символом f:
`Wrapf`, а не `Wrap`. `go vet` можна попросити перевірити конкретні назви у стилі `Printf`,
але вони мають закінчуватися на f.

```shell
$ go vet -printfuncs=wrapf,statusf
```

Також дивіться [go vet: Printf family check].

[go vet: Printf family check]: https://kuzminva.wordpress.com/2017/11/07/go-vet-printf-family-check/

## Шаблони

### Тестові таблиці

Використовуйте тести на основі таблиць з [під-тестами], щоб уникнути дублювання коду,
коли основна логіка тестування повторюється.

[під-тестами]: https://blog.golang.org/subtests

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
зменшують дублювання логіки та додають нові тестові випадки (test cases).

Ми дотримуємося домовленості, згідно з якою зріз структур називається `тестами`,
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

Паралельні тести, як і деякі спеціалізовані цикли (наприклад, ті, що породжують
горутини або захоплюють посилання як частину тіла циклу), мають подбати про те,
щоб явно призначити змінні циклу в межах циклу, щоб гарантувати, що вони зберігають
очікувані значення.

```go
tests := []struct{
  give string
  // ...
}{
  // ...
}

for _, tt := range tests {
  tt := tt // for t.Parallel
  t.Run(tt.give, func(t *testing.T) {
    t.Parallel()
    // ...
  })
}
```

У наведеному вище прикладі ми повинні оголосити змінну `tt`, обмежену ітерацією циклу,
через використання `t.Parallel()` нижче.
Якщо ми цього не зробимо, більшість або всі тести отримають неочікуване значення для `tt`
або значення, яке змінюється під час їх виконання.

### Функціональні параметри

Функціональні параметри – це шаблон, у якому ви оголошуєте непрозорий тип `Option`,
який записує інформацію в деякій внутрішній структурі. Ви приймаєте різноманітну
кількість цих параметрів і дієте відповідно до повної інформації,
записаної параметрами у внутрішній структурі.

Використовуйте цей шаблон для додаткових аргументів у конструкторах та інших
загальнодоступних API, які, як ви передбачаєте, потребують розширення,
особливо якщо у вас уже є три або більше аргументів для цих функцій.

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

// Open creates a connection.
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

Наш запропонований спосіб реалізації цього шаблону полягає в інтерфейсі `Option`,
який містить не експортований метод, записуючи параметри в не експортовану структуру `options`.

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
що наведений вище шаблон забезпечує більшу гнучкість для авторів і легше налагоджувати та
тестувати для користувачів. Зокрема, це дозволяє порівнювати параметри один з одним у тестах
та макетах (mocks), проти замикань, де це неможливо. Крім того, це дозволяє опціям реалізувати
інші інтерфейси, включаючи `fmt.Stringer`, який дозволяє зрозумілі для користувача рядкові
представлення параметрів.

Дивіться також,

- [Самореференційні функції та дизайн опцій]
- [Функціональні параметри для дружніх API]

  [Самореференційні функції та дизайн опцій]: https://commandcenter.blogspot.com/2014/01/self-referential-functions-and-design.html
  [Функціональні параметри для дружніх API]: https://dave.cheney.net/2014/10/17/functional-options-for-friendly-apis

<!-- TODO: replace this with parameter structs and functional options, when to
use one vs other -->

## Linting

Значно важливіше, ніж будь-який "благословенний" набір лінтерів,
послідовне розміщення лінту в кодовій базі.

Ми рекомендуємо використовувати принаймні такі лінтери, оскільки вважаємо,
що вони допомагають виявити найпоширеніші проблеми, а також встановлюють високу
планку якості коду:

- [errcheck] - щоб переконатися, що помилки обробляються
- [goimports] - для форматування коду та керування імпортом
- [golint] - щоб вказати на типові помилки стилю
- [govet] - для аналізу коду на наявність типових помилок
- [staticcheck] - для виконання різних перевірок статичного аналізу

  [errcheck]: https://github.com/kisielk/errcheck
  [goimports]: https://godoc.org/golang.org/x/tools/cmd/goimports
  [golint]: https://github.com/golang/lint
  [govet]: https://golang.org/cmd/vet/
  [staticcheck]: https://staticcheck.io/


### Lint Runners

Ми рекомендуємо [golangci-lint] як засіб запуску лінтів для коду Go,
головним чином завдяки його продуктивності у великих базах коду, а також можливості
налаштовувати та використовувати багато канонічних лінтерів одночасно.
Цей репозиторій містить приклад конфігураційного файлу [.golangci.yml]
із рекомендованими лінтерами та їх налаштуваннями.

golangci-lint має [різні лінтери], що доступні для використання.
Наведені вище лінтери рекомендовано як базовий набір і ми заохочуємо команди
додавати будь-які додаткові лінтери, які потрібні для їхніх проектів.

[golangci-lint]: https://github.com/golangci/golangci-lint
[.golangci.yml]: https://github.com/uber-go/guide/blob/master/.golangci.yml
[різні лінтери]: https://golangci-lint.run/usage/linters/

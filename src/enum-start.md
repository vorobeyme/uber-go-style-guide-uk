# Починайте перерахування (enum) з одиниці

Стандартним способом впровадження перерахуваннь у Go є оголошення власного типу
та групи `const` за допомогою `iota`. Оскільки змінні за замовчуванням мають значення, яке дорівнює 0,
ви зазвичай повинні починати свої перерахування з ненульових значень, наприклад з 1.

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

<!-- TODO: section on String methods for enums -->

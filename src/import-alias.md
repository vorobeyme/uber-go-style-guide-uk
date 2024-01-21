# Імпорт псевдонімів

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

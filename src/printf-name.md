# Назви функцій у стилі Printf

Коли ви оголошуєте функцію у стилі `Printf`, переконайтеся,
що `go vet` може її виявити та перевірити форматування рядка.

Це означає, що ви повинні використовувати попередньо визначені назви функцій
у стилі `Printf`, якщо це можливо. `go vet` перевірить їх за замовчуванням.
Для отримання додаткової інформації дивіться [Printf family].

  [Printf family]: https://golang.org/cmd/vet/#hdr-Printf_family

Якщо використання попередньо визначених імен неможливе, завершіть вибране ім'я символом f:
`Wrapf`, а не `Wrap`. `go vet` можна попросити перевірити конкретні назви у стилі `Printf`,
але вони мають закінчуватися на f.


```shell
go vet -printfuncs=wrapf,statusf
```
Також дивіться [go vet: Printf family check].

  [go vet: Printf family check]: https://kuzminva.wordpress.com/2017/11/07/go-vet-printf-family-check/

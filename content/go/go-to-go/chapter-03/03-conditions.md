---
series: ["go-to-go"]
title:  "Условные конструкции"
weight: 3
---

## if / else

```go
if x > 0 {
    fmt.Println("положительное")
} else if x < 0 {
    fmt.Println("отрицательное")
} else {
    fmt.Println("ноль")
}
```

Фигурные скобки обязательны. Скобки вокруг условия не нужны.

**Инициализатор в `if`:**

```go
if err := doSomething(); err != nil {
    fmt.Println("ошибка:", err)
}
```

Переменная `err` видна только внутри блока `if/else`. Этот паттерн повсеместно используется при обработке ошибок в Go.

## switch

```go
switch day {
case "Monday", "Tuesday", "Wednesday":
    fmt.Println("начало недели")
case "Thursday", "Friday":
    fmt.Println("конец недели")
default:
    fmt.Println("выходной")
}
```

`switch` не требует `break` — каждый `case` завершается автоматически. Для «провала» в следующий `case` — `fallthrough`.

**`switch` без выражения** — компактная альтернатива цепочке `if/else if`:

```go
switch {
case x < 0:
    fmt.Println("отрицательное")
case x == 0:
    fmt.Println("ноль")
default:
    fmt.Println("положительное")
}
```

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | easy      |
| 4  | Упражнение 4  | medium    |
| 5  | Упражнение 5  | medium    |

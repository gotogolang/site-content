---
series: ["go-to-go"]
title:  "Ограничения типов"
weight: 2
---

## Интерфейс как ограничение

Ограничение (constraint) задаёт, что умеет тип-параметр. Любой интерфейс можно использовать как ограничение:

```go
type Stringer interface {
    String() string
}

func Print[T Stringer](v T) {
    fmt.Println(v.String())
}
```

## Объединения типов

Ограничение может перечислять конкретные типы через `|`:

```go
type Number interface {
    int | int32 | int64 | float32 | float64
}

func Sum[T Number](s []T) T {
    var total T
    for _, v := range s {
        total += v
    }
    return total
}
```

## ~T — включая подтипы

`~int` означает «int и любой именованный тип с базовым типом int»:

```go
type Celsius float64

type Temperature interface {
    ~float64
}

func ToKelvin[T Temperature](t T) T {
    return t + 273.15
}

fmt.Println(ToKelvin(Celsius(100)))  // 373.15
```

## Пакет constraints

Стандартный пакет `golang.org/x/exp/constraints` содержит готовые ограничения:

```go
import "golang.org/x/exp/constraints"

func Min[T constraints.Ordered](a, b T) T {
    if a < b { return a }
    return b
}
```

`constraints.Ordered` — все типы, поддерживающие `<`, `>`, `<=`, `>=`.

---

## Упражнения

| №  | Задание      | Сложность |
|----|--------------|-----------|
| 1  | Упражнение 1 | medium    |
| 2  | Упражнение 2 | medium    |

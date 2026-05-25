---
series: ["go-to-go"]
title:  "Объявление функций"
weight: 1
---

## Синтаксис

```go
func add(a int, b int) int {
    return a + b
}
```

Если несколько параметров одного типа — тип можно указать один раз:

```go
func add(a, b int) int {
    return a + b
}
```

## Несколько возвращаемых значений

Go позволяет возвращать несколько значений — это стандартный способ передачи ошибок:

```go
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("деление на ноль")
    }
    return a / b, nil
}

result, err := divide(10, 3)
if err != nil {
    log.Fatal(err)
}
```

## Именованные возвращаемые значения

Возвращаемые значения можно именовать — тогда `return` без аргументов вернёт их текущие значения:

```go
func minMax(s []int) (min, max int) {
    min, max = s[0], s[0]
    for _, v := range s[1:] {
        if v < min { min = v }
        if v > max { max = v }
    }
    return   // «голый» return
}
```

Именованные возвращаемые значения читаемы в коротких функциях, но в длинных могут запутать.

## Функции как значения

Функции — полноправные значения. Их можно присваивать, передавать и возвращать:

```go
op := func(a, b int) int { return a + b }
fmt.Println(op(3, 4))  // 7

func apply(f func(int, int) int, a, b int) int {
    return f(a, b)
}
```

---

## Упражнения

| №  | Задание      | Сложность |
|----|--------------|-----------|
| 1  | Упражнение 1 | easy      |
| 2  | Упражнение 2 | easy      |
| 3  | Упражнение 3 | medium    |

---
series: ["go-to-go"]
title:  "Функции с переменным числом аргументов"
weight: 3
---

## Variadic-функции

Многоточие `...T` позволяет принимать произвольное количество аргументов одного типа:

```go
func sum(nums ...int) int {
    total := 0
    for _, n := range nums {
        total += n
    }
    return total
}

fmt.Println(sum(1, 2, 3))        // 6
fmt.Println(sum(10, 20, 30, 40)) // 100
```

Внутри функции `nums` — обычный срез `[]int`.

## Распаковка среза

Если данные уже в срезе, его можно распаковать оператором `...`:

```go
nums := []int{1, 2, 3}
fmt.Println(sum(nums...))   // 6
```

## Указатель + variadic

Указатели и variadic-параметры независимы — их можно сочетать:

```go
func appendTo(dst *[]string, vals ...string) {
    *dst = append(*dst, vals...)
}

var result []string
appendTo(&result, "a", "b", "c")
fmt.Println(result) // [a b c]
```

---

## Упражнения

| №  | Задание      | Сложность |
|----|--------------|-----------|
| 1  | Упражнение 1 | easy      |
| 2  | Упражнение 2 | medium    |
| 3  | Упражнение 3 | medium    |

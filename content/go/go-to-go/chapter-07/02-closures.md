---
series: ["go-to-go"]
title:  "Анонимные функции и замыкания"
weight: 2
---

## Анонимная функция

Функция без имени, определённая прямо в выражении:

```go
double := func(x int) int {
    return x * 2
}
fmt.Println(double(5))   // 10
```

Можно вызвать немедленно (IIFE):

```go
result := func(x, y int) int {
    return x + y
}(3, 4)
fmt.Println(result)   // 7
```

## Замыкание

Анонимная функция «захватывает» переменные из окружающей области видимости:

```go
func counter() func() int {
    n := 0
    return func() int {
        n++
        return n
    }
}

c := counter()
fmt.Println(c())   // 1
fmt.Println(c())   // 2
fmt.Println(c())   // 3
```

`n` живёт, пока жива функция `c` — сборщик мусора не удалит её раньше.

## Замыкание в цикле — ловушка

Классическая ошибка: все замыкания захватывают **одну и ту же** переменную `i`:

```go
funcs := make([]func(), 3)
for i := 0; i < 3; i++ {
    i := i   // новая переменная на каждой итерации
    funcs[i] = func() { fmt.Println(i) }
}
for _, f := range funcs {
    f()  // 0, 1, 2
}
```

Без повторного `i := i` все три функции напечатали бы `3`.

## Функциональные паттерны

```go
// Мидлвар / декоратор
func withLogging(f func(int) int) func(int) int {
    return func(x int) int {
        fmt.Printf("вызов с %d\n", x)
        return f(x)
    }
}
```

---

## Упражнения

| №  | Задание      | Сложность |
|----|--------------|-----------|
| 1  | Упражнение 1 | easy      |
| 2  | Упражнение 2 | medium    |
| 3  | Упражнение 3 | hard      |

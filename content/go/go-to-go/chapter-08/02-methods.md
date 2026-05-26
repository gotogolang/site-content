---
series: ["go-to-go"]
title:  "Методы и приёмники"
weight: 2
---

## Метод — функция с приёмником

```go
type Rect struct {
    Width, Height float64
}

func (r Rect) Area() float64 {
    return r.Width * r.Height
}

func (r *Rect) Scale(factor float64) {
    r.Width  *= factor
    r.Height *= factor
}
```

`(r Rect)` — value receiver: метод получает копию. `(r *Rect)` — pointer receiver: метод работает с оригиналом.

## Когда использовать pointer receiver

- Метод изменяет поля структуры.
- Структура большая — избегаем копирования.
- Нужна консистентность: если хоть один метод типа использует `*T`, остальные обычно тоже.

Go автоматически берёт адрес при вызове метода с pointer receiver:

```go
r := Rect{3, 4}
r.Scale(2)          // эквивалентно (&r).Scale(2)
fmt.Println(r.Area())  // 24
```

## Методы на не-структурах

Метод можно объявить на любом именованном типе в том же пакете:

```go
type Celsius float64
type Fahrenheit float64

func (c Celsius) ToFahrenheit() Fahrenheit {
    return Fahrenheit(c*9/5 + 32)
}
```

---

## Упражнения

| №  | Задание      | Сложность |
|----|--------------|-----------|
| 1  | Упражнение 1 | easy      |
| 2  | Упражнение 2 | medium    |
| 3  | Упражнение 3 | medium    |

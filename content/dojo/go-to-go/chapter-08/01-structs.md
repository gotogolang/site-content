---
series: ["go-to-go"]
title:  "Структуры"
weight: 1
---

## Объявление и литерал

Структура объединяет поля разных типов под одним именем:

```go
type Point struct {
    X float64
    Y float64
}

p := Point{X: 1.0, Y: 2.5}
fmt.Println(p.X)   // 1.0
```

Поля с прописной буквы — экспортируемые (видны из других пакетов). Строчные — приватные.

## Анонимные поля (встраивание)

```go
type Animal struct {
    Name string
}

type Dog struct {
    Animal          // встраивание
    Breed string
}

d := Dog{Animal: Animal{Name: "Rex"}, Breed: "Husky"}
fmt.Println(d.Name)   // Rex — поднято из Animal
```

## Сравнение структур

Если все поля сравнимы — структуры можно сравнивать через `==`:

```go
p1 := Point{1, 2}
p2 := Point{1, 2}
fmt.Println(p1 == p2)   // true
```

## Перечисления через iota

Go не имеет отдельного типа `enum`. Стандартный паттерн — типизированные константы с `iota`:

```go
type Direction int

const (
    North Direction = iota
    East
    South
    West
)

func (d Direction) String() string {
    return [...]string{"North", "East", "South", "West"}[d]
}
```

---

## Упражнения

| №  | Задание      | Сложность |
|----|--------------|-----------|
| 1  | Упражнение 1 | easy      |
| 2  | Упражнение 2 | easy      |
| 3  | Упражнение 3 | medium    |

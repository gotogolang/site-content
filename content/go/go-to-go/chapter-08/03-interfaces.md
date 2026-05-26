---
series: ["go-to-go"]
title:  "Интерфейсы"
weight: 3
---

## Неявная реализация

Интерфейс в Go реализуется **неявно**: если тип реализует все методы интерфейса — он его реализует. Никакого `implements`.

```go
type Stringer interface {
    String() string
}

type User struct{ Name string }

func (u User) String() string {
    return "User:" + u.Name
}

var s Stringer = User{Name: "Alice"}
fmt.Println(s.String())   // User:Alice
```

## Полиморфизм

```go
type Shape interface {
    Area() float64
}

func printArea(s Shape) {
    fmt.Printf("площадь: %.2f\n", s.Area())
}

printArea(Rect{3, 4})
printArea(Circle{5})
```

## Пустой интерфейс

`any` (псевдоним `interface{}`) принимает значение любого типа:

```go
func dump(v any) {
    fmt.Printf("%T = %v\n", v, v)
}
```

## Приведение типов

```go
var s Stringer = User{"Bob"}

u, ok := s.(User)   // type assertion
if ok {
    fmt.Println(u.Name)
}

switch v := s.(type) {
case User:
    fmt.Println("User:", v.Name)
case fmt.Stringer:
    fmt.Println("Stringer:", v.String())
}
```

---

## Упражнения

| №  | Задание      | Сложность |
|----|--------------|-----------|
| 1  | Упражнение 1 | easy      |
| 2  | Упражнение 2 | medium    |
| 3  | Упражнение 3 | hard      |

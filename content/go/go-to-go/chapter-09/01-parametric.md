---
series: ["go-to-go"]
title:  "Параметрический полиморфизм"
weight: 1
---

## Проблема до дженериков

До Go 1.18 универсальный код писали через `any` (`interface{}`), теряя типобезопасность, или через кодогенерацию.

```go
// без дженериков — unsafe
func MaxAny(a, b any) any {
    // нельзя сравнить без type switch
}
```

## Синтаксис дженериков

Параметры типа указываются в квадратных скобках:

```go
func Max[T int | float64 | string](a, b T) T {
    if a > b {
        return a
    }
    return b
}

fmt.Println(Max(3, 5))        // 5
fmt.Println(Max(3.14, 2.71))  // 3.14
fmt.Println(Max("ab", "ac"))  // ac
```

## Дженерик-тип

Параметры типа работают и для определений типов:

```go
type Stack[T any] struct {
    items []T
}

func (s *Stack[T]) Push(v T)    { s.items = append(s.items, v) }
func (s *Stack[T]) Pop() (T, bool) {
    if len(s.items) == 0 {
        var zero T
        return zero, false
    }
    v := s.items[len(s.items)-1]
    s.items = s.items[:len(s.items)-1]
    return v, true
}

s := Stack[int]{}
s.Push(1)
s.Push(2)
v, _ := s.Pop()   // 2
```

---

## Упражнения

| №  | Задание      | Сложность |
|----|--------------|-----------|
| 1  | Упражнение 1 | easy      |
| 2  | Упражнение 2 | medium    |

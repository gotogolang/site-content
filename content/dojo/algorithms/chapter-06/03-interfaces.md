---
series: ["algorithms"]
title:  "Реализация списка с интерфейсами Go"
weight: 3
---

## container/list из стандартной библиотеки

Go предоставляет двусвязный список в `container/list`:

```go
import "container/list"

l := list.New()

// Добавление
e1 := l.PushBack(10)
e2 := l.PushBack(20)
e3 := l.PushFront(5)

// Вставка относительно элемента
l.InsertAfter(15, e1)
l.InsertBefore(7, e1)

// Удаление
l.Remove(e2)

// Обход
for e := l.Front(); e != nil; e = e.Next() {
    fmt.Println(e.Value.(int))
}
```

`list.Element.Value` имеет тип `any` — нужно приведение типа при чтении.

## Типобезопасный список через дженерики

```go
type List[T any] struct {
    head *node[T]
    tail *node[T]
    len  int
}

type node[T any] struct {
    val  T
    prev *node[T]
    next *node[T]
}

func (l *List[T]) PushBack(v T) *node[T] {
    n := &node[T]{val: v, prev: l.tail}
    if l.tail != nil { l.tail.next = n }
    l.tail = n
    if l.head == nil { l.head = n }
    l.len++
    return n
}

func (l *List[T]) Len() int { return l.len }
```

## Функциональный обход

```go
func (l *List[T]) Filter(fn func(T) bool) *List[T] {
    result := &List[T]{}
    for n := l.head; n != nil; n = n.next {
        if fn(n.val) { result.PushBack(n.val) }
    }
    return result
}

func (l *List[T]) Map(fn func(T) T) *List[T] {
    result := &List[T]{}
    for n := l.head; n != nil; n = n.next {
        result.PushBack(fn(n.val))
    }
    return result
}
```

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | medium    |

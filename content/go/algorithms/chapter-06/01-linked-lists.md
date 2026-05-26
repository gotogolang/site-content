---
series: ["algorithms"]
title:  "Односвязные и двусвязные списки"
weight: 1
---

## Односвязный список

Каждый узел хранит значение и указатель на следующий узел. Последний узел указывает на `nil`.

```go
type Node[T any] struct {
    Value T
    Next  *Node[T]
}

type SList[T any] struct {
    Head *Node[T]
    size int
}

func (l *SList[T]) PushFront(v T) {
    l.Head = &Node[T]{Value: v, Next: l.Head}
    l.size++
}

func (l *SList[T]) PopFront() (T, bool) {
    if l.Head == nil { var zero T; return zero, false }
    v := l.Head.Value
    l.Head = l.Head.Next
    l.size--
    return v, true
}
```

## Двусвязный список

Каждый узел хранит указатели на следующий **и** предыдущий узлы. Позволяет обходить список в обоих направлениях и удалять узел за `O(1)` при наличии указателя.

```go
type DNode[T any] struct {
    Value T
    Prev  *DNode[T]
    Next  *DNode[T]
}

type DList[T any] struct {
    Head *DNode[T]
    Tail *DNode[T]
    size int
}

func (l *DList[T]) PushBack(v T) {
    n := &DNode[T]{Value: v, Prev: l.Tail}
    if l.Tail != nil { l.Tail.Next = n }
    l.Tail = n
    if l.Head == nil { l.Head = n }
    l.size++
}
```

## Кольцевой список

Хвост указывает на голову. Нет `nil`-окончания — полезен для кольцевых буферов и карусельных планировщиков.

```go
// Для кольцевого списка из стандартной библиотеки:
import "container/ring"

r := ring.New(5)
for i := range r.Len() {
    r.Value = i
    r = r.Next()
}
```

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | medium    |

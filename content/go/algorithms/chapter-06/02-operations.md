---
series: ["algorithms"]
title:  "Операции над списком"
weight: 2
---

## Вставка

```go
// Вставка в начало — O(1)
func (l *SList[T]) PushFront(v T) {
    l.Head = &Node[T]{Value: v, Next: l.Head}
}

// Вставка после узла — O(1) при наличии указателя
func insertAfter[T any](node *Node[T], v T) {
    node.Next = &Node[T]{Value: v, Next: node.Next}
}

// Вставка в конец — O(n) для односвязного (нет tail)
func (l *SList[T]) PushBack(v T) {
    n := &Node[T]{Value: v}
    if l.Head == nil { l.Head = n; return }
    cur := l.Head
    for cur.Next != nil { cur = cur.Next }
    cur.Next = n
}
```

## Удаление

```go
// Удаление головы — O(1)
func (l *SList[T]) PopFront() (T, bool) {
    if l.Head == nil { var zero T; return zero, false }
    v := l.Head.Value
    l.Head = l.Head.Next
    return v, true
}

// Удаление по значению — O(n)
func (l *SList[T]) Remove(v T, eq func(T, T) bool) bool {
    if l.Head == nil { return false }
    if eq(l.Head.Value, v) { l.Head = l.Head.Next; return true }
    cur := l.Head
    for cur.Next != nil {
        if eq(cur.Next.Value, v) { cur.Next = cur.Next.Next; return true }
        cur = cur.Next
    }
    return false
}
```

## Обход и поиск

```go
// Обход — O(n)
func (l *SList[T]) ForEach(fn func(T)) {
    for cur := l.Head; cur != nil; cur = cur.Next {
        fn(cur.Value)
    }
}

// Поиск — O(n)
func (l *SList[T]) Find(v T, eq func(T, T) bool) *Node[T] {
    for cur := l.Head; cur != nil; cur = cur.Next {
        if eq(cur.Value, v) { return cur }
    }
    return nil
}
```

## Разворот списка

Классическая задача — итеративный разворот за `O(n)` и `O(1)` памяти:

```go
func (l *SList[T]) Reverse() {
    var prev *Node[T]
    cur := l.Head
    for cur != nil {
        next := cur.Next
        cur.Next = prev
        prev = cur
        cur = next
    }
    l.Head = prev
}
```

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | medium    |
| 4  | Упражнение 4  | medium    |

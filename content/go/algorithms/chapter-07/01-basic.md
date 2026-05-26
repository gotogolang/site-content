---
series: ["algorithms"]
title:  "Стеки и очереди"
weight: 1
---

## Стек (LIFO)

Last In First Out — последний вошёл, первый вышел. Операции: `Push`, `Pop`, `Peek`.

```go
type Stack[T any] struct{ data []T }

func (s *Stack[T]) Push(v T)            { s.data = append(s.data, v) }
func (s *Stack[T]) Len() int             { return len(s.data) }
func (s *Stack[T]) IsEmpty() bool        { return len(s.data) == 0 }

func (s *Stack[T]) Pop() (T, bool) {
    if s.IsEmpty() { var z T; return z, false }
    n := len(s.data) - 1
    v := s.data[n]
    s.data = s.data[:n]
    return v, true
}

func (s *Stack[T]) Peek() (T, bool) {
    if s.IsEmpty() { var z T; return z, false }
    return s.data[len(s.data)-1], true
}
```

Применения: история браузера, отмена действий (undo), обход DFS, проверка скобочных выражений.

## Очередь (FIFO)

First In First Out — первый вошёл, первый вышел. Операции: `Enqueue`, `Dequeue`, `Front`.

```go
type Queue[T any] struct{ data []T }

func (q *Queue[T]) Enqueue(v T)         { q.data = append(q.data, v) }
func (q *Queue[T]) Len() int             { return len(q.data) }
func (q *Queue[T]) IsEmpty() bool        { return len(q.data) == 0 }

func (q *Queue[T]) Dequeue() (T, bool) {
    if q.IsEmpty() { var z T; return z, false }
    v := q.data[0]
    q.data = q.data[1:]   // O(n) — сдвиг; для O(1) нужен кольцевой буфер
    return v, true
}
```

Применения: очереди задач, обход BFS, буферизация потоков.

## Дек (Deque)

Double-ended queue — вставка и удаление с обоих концов за `O(1)`.

```go
import "container/list"

type Deque[T any] struct{ l *list.List }

func NewDeque[T any]() *Deque[T]         { return &Deque[T]{list.New()} }
func (d *Deque[T]) PushFront(v T)        { d.l.PushFront(v) }
func (d *Deque[T]) PushBack(v T)         { d.l.PushBack(v) }
func (d *Deque[T]) PopFront() (T, bool)  { return popEnd[T](d.l, d.l.Front()) }
func (d *Deque[T]) PopBack() (T, bool)   { return popEnd[T](d.l, d.l.Back()) }

func popEnd[T any](l *list.List, e *list.Element) (T, bool) {
    if e == nil { var z T; return z, false }
    return l.Remove(e).(T), true
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

---
series: ["algorithms"]
title:  "Кольцевой буфер"
weight: 2
---

## Идея

Кольцевой (циклический) буфер — массив фиксированного размера с двумя указателями: `head` (читаем) и `tail` (пишем). При достижении конца массива указатель «оборачивается» на начало.

```
┌───┬───┬───┬───┬───┐
│   │ 2 │ 3 │ 4 │   │
└───┴───┴───┴───┴───┘
  ↑ head           ↑ tail
```

Даёт `O(1)` для `Enqueue` и `Dequeue` без аллокаций.

## Реализация

```go
type RingBuffer[T any] struct {
    data []T
    head int
    tail int
    size int
    cap  int
}

func NewRingBuffer[T any](cap int) *RingBuffer[T] {
    return &RingBuffer[T]{data: make([]T, cap), cap: cap}
}

func (r *RingBuffer[T]) Enqueue(v T) bool {
    if r.size == r.cap { return false } // переполнен
    r.data[r.tail] = v
    r.tail = (r.tail + 1) % r.cap
    r.size++
    return true
}

func (r *RingBuffer[T]) Dequeue() (T, bool) {
    if r.size == 0 { var z T; return z, false }
    v := r.data[r.head]
    r.head = (r.head + 1) % r.cap
    r.size--
    return v, true
}

func (r *RingBuffer[T]) Len() int  { return r.size }
func (r *RingBuffer[T]) Full() bool { return r.size == r.cap }
```

## Ограниченная очередь (producer-consumer)

```go
import "sync"

type BoundedQueue[T any] struct {
    buf  *RingBuffer[T]
    mu   sync.Mutex
    notEmpty sync.Cond
    notFull  sync.Cond
}

func (q *BoundedQueue[T]) Put(v T) {
    q.mu.Lock()
    for q.buf.Full() { q.notFull.Wait() }
    q.buf.Enqueue(v)
    q.notEmpty.Signal()
    q.mu.Unlock()
}

func (q *BoundedQueue[T]) Take() T {
    q.mu.Lock()
    for q.buf.Len() == 0 { q.notEmpty.Wait() }
    v, _ := q.buf.Dequeue()
    q.notFull.Signal()
    q.mu.Unlock()
    return v
}
```

## Применения

- Буферы аудио/видео потоков.
- Логгеры с ограниченной историей (последние N событий).
- Межпотоковая передача данных без аллокаций.
- Сетевые пакетные буферы.

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | medium    |

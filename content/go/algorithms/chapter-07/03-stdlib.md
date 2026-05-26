---
series: ["algorithms"]
title:  "Очереди и стеки в стандартной библиотеке"
weight: 3
---

## container/list как очередь и стек

`container/list` — двусвязный список, используется как основа для дека:

```go
import "container/list"

// Стек
stack := list.New()
stack.PushBack(1)
stack.PushBack(2)
stack.PushBack(3)
top := stack.Back()
stack.Remove(top)
fmt.Println(top.Value) // 3

// Очередь
queue := list.New()
queue.PushBack("a")
queue.PushBack("b")
front := queue.Front()
queue.Remove(front)
fmt.Println(front.Value) // "a"
```

## container/heap — приоритетная очередь

Реализуй интерфейс `heap.Interface` для любого типа:

```go
import "container/heap"

type MinHeap []int

func (h MinHeap) Len() int           { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h MinHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *MinHeap) Push(x any)        { *h = append(*h, x.(int)) }
func (h *MinHeap) Pop() any {
    old := *h
    n := len(old)
    v := old[n-1]
    *h = old[:n-1]
    return v
}

h := &MinHeap{5, 1, 3}
heap.Init(h)
heap.Push(h, 2)
fmt.Println(heap.Pop(h)) // 1
fmt.Println(heap.Pop(h)) // 2
```

## Горутинный канал как очередь

Канал Go — это потокобезопасная очередь с фиксированным буфером:

```go
// Буферизированный канал = очередь ёмкостью 10
ch := make(chan int, 10)

go func() {
    for i := 0; i < 5; i++ { ch <- i }
    close(ch)
}()

for v := range ch { fmt.Println(v) }
```

Для задач producer-consumer в Go каналы — идиоматичный инструмент.

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | medium    |

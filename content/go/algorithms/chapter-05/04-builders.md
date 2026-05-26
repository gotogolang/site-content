---
series: ["algorithms"]
title:  "Строитель строк и другие динамические структуры"
weight: 4
---

## strings.Builder

Для конкатенации многих строк `+` создаёт `O(n²)` копий. `strings.Builder` — динамический буфер под капотом:

```go
import "strings"

var b strings.Builder
for i := 0; i < 5; i++ {
    fmt.Fprintf(&b, "item %d\n", i)
}
result := b.String()

// Сброс и повторное использование
b.Reset()
```

`strings.Builder` реализует `io.Writer` — совместим со всеми функциями, принимающими Writer.

## bytes.Buffer

Универсальный буфер для байтов — чтение и запись:

```go
import "bytes"

var buf bytes.Buffer
buf.WriteString("Hello")
buf.WriteByte(',')
buf.WriteString(" Go!")

fmt.Println(buf.String()) // "Hello, Go!"

// Чтение
line, _ := buf.ReadString('!')
fmt.Println(line) // "Hello, Go!"
```

## Срез как стек

```go
type Stack[T any] struct{ data []T }

func (s *Stack[T]) Push(v T)      { s.data = append(s.data, v) }
func (s *Stack[T]) Pop() (T, bool) {
    if len(s.data) == 0 {
        var zero T
        return zero, false
    }
    n := len(s.data) - 1
    v := s.data[n]
    s.data = s.data[:n]
    return v, true
}
func (s *Stack[T]) Peek() (T, bool) {
    if len(s.data) == 0 { var zero T; return zero, false }
    return s.data[len(s.data)-1], true
}
```

## Срез как очередь (неэффективно)

```go
// Наивная очередь на срезе — O(n) для Dequeue из-за сдвига
queue := []int{}
queue = append(queue, 1)          // enqueue
front := queue[0]; queue = queue[1:] // dequeue — сдвигает все элементы
```

Для эффективной очереди — кольцевой буфер или `container/list`.

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | medium    |

---
series: ["algorithms"]
title:  "Кратчайшие пути: алгоритм Дейкстры"
weight: 5
---

## Задача

Найти кратчайший путь от исходной вершины `src` до всех остальных в **взвешенном** графе с неотрицательными весами.

## Алгоритм

Жадный подход: на каждом шаге выбирает вершину с минимальным известным расстоянием и расслабляет её рёбра.

```go
import (
    "container/heap"
    "math"
)

type Item struct{ vertex, dist int }
type PQ []Item

func (pq PQ) Len() int            { return len(pq) }
func (pq PQ) Less(i, j int) bool  { return pq[i].dist < pq[j].dist }
func (pq PQ) Swap(i, j int)       { pq[i], pq[j] = pq[j], pq[i] }
func (pq *PQ) Push(x any)         { *pq = append(*pq, x.(Item)) }
func (pq *PQ) Pop() any           { old := *pq; n := len(old); v := old[n-1]; *pq = old[:n-1]; return v }

func dijkstra(g *ListGraph, src int) []int {
    dist := make([]int, g.n)
    for i := range dist { dist[i] = math.MaxInt }
    dist[src] = 0

    pq := &PQ{{src, 0}}
    heap.Init(pq)

    for pq.Len() > 0 {
        item := heap.Pop(pq).(Item)
        v, d := item.vertex, item.dist
        if d > dist[v] { continue } // устаревший элемент
        for _, e := range g.Neighbors(v) {
            if nd := dist[v] + e.Weight; nd < dist[e.To] {
                dist[e.To] = nd
                heap.Push(pq, Item{e.To, nd})
            }
        }
    }
    return dist
}
```

**Сложность:** `O((V + E) log V)` с двоичной кучей.

## Восстановление пути

```go
func dijkstraWithPath(g *ListGraph, src int) ([]int, []int) {
    dist := make([]int, g.n)
    prev := make([]int, g.n)
    for i := range dist { dist[i] = math.MaxInt; prev[i] = -1 }
    dist[src] = 0
    // ... (как выше, но добавляем: prev[e.To] = v)

    return dist, prev
}

func reconstructPath(prev []int, dst int) []int {
    var path []int
    for dst != -1 { path = append([]int{dst}, path...); dst = prev[dst] }
    return path
}
```

## Ограничения и альтернативы

| Алгоритм       | Веса    | Сложность       | Особенности               |
|----------------|---------|-----------------|---------------------------|
| Дейкстра       | ≥ 0     | `O((V+E) log V)`| Один источник             |
| Беллман-Форд   | Любые   | `O(V·E)`        | Обнаруживает отриц. циклы |
| Флойд-Уоршелл  | Любые   | `O(V³)`         | Все пары вершин           |
| A*             | ≥ 0     | `O(E log V)`    | С эвристикой              |

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | medium    |
| 4  | Упражнение 4  | medium    |

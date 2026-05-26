---
series: ["algorithms"]
title:  "Интерфейсы для графов в Go"
weight: 4
---

## Обобщённый интерфейс графа

```go
type Graph[V comparable] interface {
    AddVertex(v V)
    AddEdge(from, to V, weight float64)
    Neighbors(v V) []V
    Vertices() []V
    HasEdge(from, to V) bool
    Weight(from, to V) (float64, bool)
}
```

## Реализация через map

```go
type MapGraph[V comparable] struct {
    adj map[V]map[V]float64
}

func NewMapGraph[V comparable]() *MapGraph[V] {
    return &MapGraph[V]{adj: make(map[V]map[V]float64)}
}

func (g *MapGraph[V]) AddVertex(v V) {
    if g.adj[v] == nil { g.adj[v] = make(map[V]float64) }
}

func (g *MapGraph[V]) AddEdge(from, to V, weight float64) {
    g.AddVertex(from); g.AddVertex(to)
    g.adj[from][to] = weight
}

func (g *MapGraph[V]) Neighbors(v V) []V {
    neighbors := make([]V, 0, len(g.adj[v]))
    for u := range g.adj[v] { neighbors = append(neighbors, u) }
    return neighbors
}

func (g *MapGraph[V]) Vertices() []V {
    vs := make([]V, 0, len(g.adj))
    for v := range g.adj { vs = append(vs, v) }
    return vs
}
```

## Обобщённый DFS

```go
func DFS[V comparable](g Graph[V], start V, fn func(V)) {
    visited := make(map[V]bool)
    var dfs func(V)
    dfs = func(v V) {
        if visited[v] { return }
        visited[v] = true
        fn(v)
        for _, u := range g.Neighbors(v) { dfs(u) }
    }
    dfs(start)
}
```

## Преимущества подхода

- Один алгоритм (DFS, BFS, Dijkstra) работает с любой реализацией графа.
- Легко подменить матрицу смежности на список смежности без изменения алгоритмов.
- Тестируемость: mock-граф для юнит-тестов.

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | medium    |

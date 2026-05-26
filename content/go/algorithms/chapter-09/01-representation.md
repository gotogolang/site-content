---
series: ["algorithms"]
title:  "Представление графов"
weight: 1
---

## Основные понятия

**Граф** G = (V, E): множество вершин V и рёбер E.

- **Ориентированный** (directed): рёбра имеют направление.
- **Взвешенный** (weighted): рёбра имеют вес/стоимость.
- **Цикл**: путь, начинающийся и заканчивающийся в одной вершине.
- **Связный**: между любыми двумя вершинами существует путь.

## Матрица смежности

Двумерный массив `adj[i][j]` = вес ребра (i→j), 0 если ребра нет.

```go
type MatrixGraph struct {
    adj [][]int
    n   int
}

func NewMatrixGraph(n int) *MatrixGraph {
    adj := make([][]int, n)
    for i := range adj { adj[i] = make([]int, n) }
    return &MatrixGraph{adj, n}
}

func (g *MatrixGraph) AddEdge(from, to, weight int) { g.adj[from][to] = weight }
func (g *MatrixGraph) HasEdge(from, to int) bool     { return g.adj[from][to] != 0 }
```

**Сложность:** `O(V²)` памяти. Проверка ребра — `O(1)`. Перебор соседей — `O(V)`.
Подходит для **плотных** графов (E ≈ V²).

## Список смежности

Для каждой вершины — список её соседей.

```go
type Edge struct{ To, Weight int }

type ListGraph struct {
    adj [][]Edge
    n   int
}

func NewListGraph(n int) *ListGraph {
    return &ListGraph{adj: make([][]Edge, n), n: n}
}

func (g *ListGraph) AddEdge(from, to, weight int) {
    g.adj[from] = append(g.adj[from], Edge{to, weight})
}

func (g *ListGraph) Neighbors(v int) []Edge { return g.adj[v] }
```

**Сложность:** `O(V + E)` памяти. Перебор соседей — `O(degree(v))`.
Подходит для **разреженных** графов (E << V²).

## Сравнение

| Критерий           | Матрица смежности | Список смежности |
|--------------------|-------------------|------------------|
| Память             | `O(V²)`           | `O(V + E)`       |
| Проверка ребра     | `O(1)`            | `O(degree)`      |
| Перебор соседей    | `O(V)`            | `O(degree)`      |
| Добавление ребра   | `O(1)`            | `O(1)`           |
| Применение         | Плотные графы     | Разреженные      |

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | medium    |

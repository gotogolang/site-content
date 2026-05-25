---
series: ["algorithms"]
title:  "Минимальное остовное дерево"
weight: 3
---

## Определение

**Остовное дерево** связного графа — подграф, содержащий все вершины и являющийся деревом (нет циклов).
**Минимальное** (MST) — остовное дерево с наименьшей суммой весов рёбер.

## Алгоритм Прима

Жадный алгоритм: начинает с произвольной вершины, на каждом шаге добавляет ребро минимального веса, соединяющее дерево с новой вершиной.

```go
func prim(g *MatrixGraph) int {
    n := g.n
    inMST := make([]bool, n)
    minEdge := make([]int, n)
    for i := range minEdge { minEdge[i] = 1<<31 - 1 }
    minEdge[0] = 0
    total := 0

    for i := 0; i < n; i++ {
        // Найти вершину с минимальным ребром вне MST
        u := -1
        for v := 0; v < n; v++ {
            if !inMST[v] && (u == -1 || minEdge[v] < minEdge[u]) { u = v }
        }
        inMST[u] = true
        total += minEdge[u]
        // Обновить minEdge для соседей
        for v := 0; v < n; v++ {
            if g.adj[u][v] != 0 && !inMST[v] && g.adj[u][v] < minEdge[v] {
                minEdge[v] = g.adj[u][v]
            }
        }
    }
    return total
}
```

Сложность с очередью с приоритетом: `O(E log V)`.

## Алгоритм Крускала

Сортирует все рёбра по весу, добавляет ребро, если оно не создаёт цикл. Использует структуру **Disjoint Set Union (DSU)** для проверки циклов.

```go
type DSU struct{ parent, rank []int }

func NewDSU(n int) *DSU {
    p := make([]int, n); r := make([]int, n)
    for i := range p { p[i] = i }
    return &DSU{p, r}
}

func (d *DSU) Find(x int) int {
    if d.parent[x] != x { d.parent[x] = d.Find(d.parent[x]) }
    return d.parent[x]
}

func (d *DSU) Union(x, y int) bool {
    px, py := d.Find(x), d.Find(y)
    if px == py { return false }
    if d.rank[px] < d.rank[py] { px, py = py, px }
    d.parent[py] = px
    if d.rank[px] == d.rank[py] { d.rank[px]++ }
    return true
}
```

Сложность: `O(E log E)`.

## Когда использовать

- **Прим** — для плотных графов (матрица смежности).
- **Крускал** — для разреженных графов (список рёбер).

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | medium    |

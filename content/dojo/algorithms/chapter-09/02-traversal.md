---
series: ["algorithms"]
title:  "Обходы графа: DFS и BFS"
weight: 2
---

## DFS — обход в глубину

Идёт как можно глубже, отступает при тупике. Использует стек (рекурсию или явный).

```go
func dfs(g *ListGraph, start int, visited []bool, fn func(int)) {
    if visited[start] { return }
    visited[start] = true
    fn(start)
    for _, e := range g.Neighbors(start) {
        dfs(g, e.To, visited, fn)
    }
}

// Использование
visited := make([]bool, g.n)
dfs(g, 0, visited, func(v int) { fmt.Println(v) })
```

**Итеративная версия:**

```go
func dfsIterative(g *ListGraph, start int) []int {
    visited := make([]bool, g.n)
    stack := []int{start}
    var order []int
    for len(stack) > 0 {
        v := stack[len(stack)-1]
        stack = stack[:len(stack)-1]
        if visited[v] { continue }
        visited[v] = true
        order = append(order, v)
        for _, e := range g.Neighbors(v) {
            if !visited[e.To] { stack = append(stack, e.To) }
        }
    }
    return order
}
```

## BFS — обход в ширину

Обходит вершины слоями — сначала все соседи, потом соседи соседей. Использует очередь. Находит кратчайший путь в **невзвешенном** графе.

```go
func bfs(g *ListGraph, start int) []int {
    visited := make([]bool, g.n)
    queue := []int{start}
    visited[start] = true
    var order []int
    for len(queue) > 0 {
        v := queue[0]
        queue = queue[1:]
        order = append(order, v)
        for _, e := range g.Neighbors(v) {
            if !visited[e.To] {
                visited[e.To] = true
                queue = append(queue, e.To)
            }
        }
    }
    return order
}
```

## Применения

| Алгоритм | Применение                                                |
|----------|-----------------------------------------------------------|
| DFS      | Топологическая сортировка, поиск циклов, компоненты связности |
| BFS      | Кратчайший путь (невзв.), уровни графа, двудольность       |

**Сложность обоих:** `O(V + E)`.

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | medium    |
| 4  | Упражнение 4  | medium    |

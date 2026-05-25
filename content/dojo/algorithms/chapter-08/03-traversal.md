---
series: ["algorithms"]
title:  "Обходы дерева"
weight: 3
---

## Обходы в глубину (DFS)

Три варианта — отличаются порядком обработки узла относительно его поддеревьев.

```go
// Pre-order: корень → левое → правое
func preOrder[T any](n *TreeNode[T], fn func(T)) {
    if n == nil { return }
    fn(n.Value)
    preOrder(n.Left, fn)
    preOrder(n.Right, fn)
}

// In-order: левое → корень → правое (для BST даёт отсортированный порядок)
func inOrder[T any](n *TreeNode[T], fn func(T)) {
    if n == nil { return }
    inOrder(n.Left, fn)
    fn(n.Value)
    inOrder(n.Right, fn)
}

// Post-order: левое → правое → корень (удаление дерева, вычисление выражений)
func postOrder[T any](n *TreeNode[T], fn func(T)) {
    if n == nil { return }
    postOrder(n.Left, fn)
    postOrder(n.Right, fn)
    fn(n.Value)
}
```

## Итеративный in-order (без рекурсии)

```go
func inOrderIterative[T any](root *TreeNode[T]) []T {
    var result []T
    var stack []*TreeNode[T]
    cur := root
    for cur != nil || len(stack) > 0 {
        for cur != nil { stack = append(stack, cur); cur = cur.Left }
        cur = stack[len(stack)-1]
        stack = stack[:len(stack)-1]
        result = append(result, cur.Value)
        cur = cur.Right
    }
    return result
}
```

## Обход в ширину (BFS / Level-order)

Обходит дерево уровень за уровнем, используя очередь:

```go
func bfs[T any](root *TreeNode[T], fn func(T)) {
    if root == nil { return }
    queue := []*TreeNode[T]{root}
    for len(queue) > 0 {
        node := queue[0]
        queue = queue[1:]
        fn(node.Value)
        if node.Left  != nil { queue = append(queue, node.Left) }
        if node.Right != nil { queue = append(queue, node.Right) }
    }
}
```

## Применения

| Обход      | Применение                                        |
|------------|---------------------------------------------------|
| Pre-order  | Копирование дерева, сериализация                  |
| In-order   | Отсортированный вывод BST, range queries          |
| Post-order | Удаление дерева, вычисление арифметических выражений |
| BFS        | Кратчайший путь, уровни дерева, is-balanced       |

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | medium    |
| 4  | Упражнение 4  | medium    |

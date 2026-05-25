---
series: ["algorithms"]
title:  "Бинарное дерево поиска"
weight: 1
---

## Структура

В бинарном дереве поиска (BST) каждый узел содержит ключ: все ключи в левом поддереве меньше, в правом — больше.

```go
type TreeNode[T any] struct {
    Key   int
    Value T
    Left  *TreeNode[T]
    Right *TreeNode[T]
}
```

## Вставка — O(h)

```go
func insert[T any](root *TreeNode[T], key int, val T) *TreeNode[T] {
    if root == nil { return &TreeNode[T]{Key: key, Value: val} }
    if key < root.Key      { root.Left  = insert(root.Left,  key, val) }
    else if key > root.Key { root.Right = insert(root.Right, key, val) }
    else                   { root.Value = val } // обновление
    return root
}
```

## Поиск — O(h)

```go
func search[T any](root *TreeNode[T], key int) (T, bool) {
    if root == nil         { var z T; return z, false }
    if key == root.Key     { return root.Value, true }
    if key < root.Key      { return search(root.Left,  key) }
    return search(root.Right, key)
}
```

## Удаление — O(h)

Три случая: узел — лист, имеет одного ребёнка, имеет двух детей.
При двух детях — заменяем на **in-order successor** (минимум правого поддерева).

```go
func delete[T any](root *TreeNode[T], key int) *TreeNode[T] {
    if root == nil { return nil }
    if key < root.Key      { root.Left  = delete(root.Left,  key) }
    else if key > root.Key { root.Right = delete(root.Right, key) }
    else {
        if root.Left == nil  { return root.Right }
        if root.Right == nil { return root.Left }
        min := findMin(root.Right)
        root.Key, root.Value = min.Key, min.Value
        root.Right = delete(root.Right, min.Key)
    }
    return root
}

func findMin[T any](n *TreeNode[T]) *TreeNode[T] {
    for n.Left != nil { n = n.Left }
    return n
}
```

## Высота и деградация

Высота `h` определяет сложность операций. В сбалансированном дереве `h = O(log n)`. Для отсортированных данных BST деградирует в список: `h = O(n)`. Решение — самобалансирующиеся деревья.

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | medium    |
| 4  | Упражнение 4  | medium    |

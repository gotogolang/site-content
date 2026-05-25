---
series: ["algorithms"]
title:  "Быстрые сортировки"
weight: 2
---

Алгоритмы с `O(n log n)` в среднем или гарантированно. Основа стандартных библиотек.

## QuickSort

Выбирает опорный элемент (pivot), делит массив на два подмассива, рекурсивно сортирует каждый.
Среднее: `O(n log n)`. Худшее: `O(n²)` при плохом выборе pivot.

```go
func quickSort(a []int, lo, hi int) {
    if lo >= hi { return }
    p := partition(a, lo, hi)
    quickSort(a, lo, p-1)
    quickSort(a, p+1, hi)
}

func partition(a []int, lo, hi int) int {
    pivot := a[hi]
    i := lo
    for j := lo; j < hi; j++ {
        if a[j] <= pivot {
            a[i], a[j] = a[j], a[i]
            i++
        }
    }
    a[i], a[hi] = a[hi], a[i]
    return i
}
```

## MergeSort

Делит массив пополам, рекурсивно сортирует каждую половину, затем сливает. Гарантированное `O(n log n)`, но требует `O(n)` памяти.

```go
func mergeSort(a []int) []int {
    if len(a) <= 1 { return a }
    mid := len(a) / 2
    left := mergeSort(a[:mid])
    right := mergeSort(a[mid:])
    return merge(left, right)
}

func merge(l, r []int) []int {
    result := make([]int, 0, len(l)+len(r))
    i, j := 0, 0
    for i < len(l) && j < len(r) {
        if l[i] <= r[j] { result = append(result, l[i]); i++ } else { result = append(result, r[j]); j++ }
    }
    result = append(result, l[i:]...)
    return append(result, r[j:]...)
}
```

## HeapSort

Строит max-heap, затем последовательно извлекает максимумы. `O(n log n)` гарантированно, `O(1)` памяти.

```go
func heapSort(a []int) {
    n := len(a)
    for i := n/2 - 1; i >= 0; i-- { heapify(a, n, i) }
    for i := n - 1; i > 0; i-- {
        a[0], a[i] = a[i], a[0]
        heapify(a, i, 0)
    }
}

func heapify(a []int, n, i int) {
    largest, l, r := i, 2*i+1, 2*i+2
    if l < n && a[l] > a[largest] { largest = l }
    if r < n && a[r] > a[largest] { largest = r }
    if largest != i {
        a[i], a[largest] = a[largest], a[i]
        heapify(a, n, largest)
    }
}
```

## Сравнение

| Алгоритм  | Лучший       | Средний      | Худший       | Память    | Стабильная |
|-----------|--------------|--------------|--------------|-----------|------------|
| QuickSort | `O(n log n)` | `O(n log n)` | `O(n²)`      | `O(log n)`| ✗          |
| MergeSort | `O(n log n)` | `O(n log n)` | `O(n log n)` | `O(n)`    | ✓          |
| HeapSort  | `O(n log n)` | `O(n log n)` | `O(n log n)` | `O(1)`    | ✗          |

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | medium    |
| 4  | Упражнение 4  | medium    |

---
series: ["algorithms"]
title:  "Временная и пространственная сложность"
weight: 2
---

## Два измерения эффективности

Алгоритм характеризуется двумя показателями:

- **Временная сложность** — сколько операций выполняется.
- **Пространственная сложность** — сколько дополнительной памяти требуется.

Часто приходится выбирать: быстрее *или* экономнее по памяти.

## Временная сложность

Оценивается в трёх сценариях:

| Сценарий     | Описание                                 |
|--------------|------------------------------------------|
| Лучший случай | Входные данные идеальны (редко важен)   |
| Средний случай | Ожидаемое поведение на случайных данных |
| Худший случай | Гарантия для любых данных (основной)    |

Пример — линейный поиск:
- Лучший: `O(1)` — элемент первый.
- Средний: `O(n/2)` = `O(n)`.
- Худший: `O(n)` — элемента нет.

## Пространственная сложность

Учитывается **дополнительная** память (не считая входных данных).

```go
// O(1) — только переменные
func sum(nums []int) int {
    total := 0
    for _, v := range nums { total += v }
    return total
}

// O(n) — копия входного среза
func doubled(nums []int) []int {
    result := make([]int, len(nums))
    for i, v := range nums { result[i] = v * 2 }
    return result
}

// O(log n) — стек рекурсии при бинарном поиске
func bsearch(nums []int, lo, hi, target int) int {
    if lo > hi { return -1 }
    mid := (lo + hi) / 2
    if nums[mid] == target { return mid }
    if nums[mid] < target { return bsearch(nums, mid+1, hi, target) }
    return bsearch(nums, lo, mid-1, target)
}
```

## Компромиссы

Типичный паттерн «время против памяти»:

```go
// O(n²) время, O(1) память — сравниваем все пары
func hasDuplicateSlow(nums []int) bool {
    for i := range nums {
        for j := i + 1; j < len(nums); j++ {
            if nums[i] == nums[j] { return true }
        }
    }
    return false
}

// O(n) время, O(n) память — используем хеш-сет
func hasDuplicateFast(nums []int) bool {
    seen := make(map[int]bool, len(nums))
    for _, v := range nums {
        if seen[v] { return true }
        seen[v] = true
    }
    return false
}
```

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | medium    |

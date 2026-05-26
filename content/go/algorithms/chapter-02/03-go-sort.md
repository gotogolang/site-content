---
series: ["algorithms"]
title:  "Сортировка в стандартном пакете Go"
weight: 3
---

Пакет `sort` содержит всё необходимое для практической сортировки.

## sort.Slice

Самый удобный способ — сортировка через функцию-компаратор:

```go
import "sort"

people := []struct{ Name string; Age int }{
    {"Alice", 30}, {"Bob", 25}, {"Carol", 35},
}

// По возрасту
sort.Slice(people, func(i, j int) bool {
    return people[i].Age < people[j].Age
})

// По имени в обратном порядке
sort.Slice(people, func(i, j int) bool {
    return people[i].Name > people[j].Name
})
```

`sort.SliceStable` сохраняет исходный порядок равных элементов.

## sort.Sort

Для повторного использования — реализуйте интерфейс `sort.Interface`:

```go
type ByAge []Person

func (a ByAge) Len() int           { return len(a) }
func (a ByAge) Less(i, j int) bool { return a[i].Age < a[j].Age }
func (a ByAge) Swap(i, j int)      { a[i], a[j] = a[j], a[i] }

sort.Sort(ByAge(people))
```

## Встроенные функции

```go
nums := []int{5, 2, 8, 1, 9}
sort.Ints(nums)              // [1 2 5 8 9]

words := []string{"banana", "apple", "cherry"}
sort.Strings(words)          // [apple banana cherry]

// Проверка отсортированности
sort.IntsAreSorted(nums)     // true

// Бинарный поиск в отсортированном срезе
i := sort.SearchInts(nums, 5) // индекс первого элемента >= 5
```

## sort.Search (бинарный поиск)

```go
// Найти первый индекс, где условие выполняется
i := sort.Search(len(nums), func(i int) bool {
    return nums[i] >= 8
})
// i == 3 (nums[3] == 8)
```

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | medium    |

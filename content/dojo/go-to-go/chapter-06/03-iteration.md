---
series: ["go-to-go"]
title:  "Итерация по карте"
weight: 3
---

## range по карте

```go
m := map[string]int{"a": 1, "b": 2, "c": 3}

for k, v := range m {
    fmt.Printf("%s: %d\n", k, v)
}
```

**Порядок итерации не определён.** При каждом запуске он может быть разным — это намеренное решение Go. Не пишите код, который рассчитывает на конкретный порядок.

## Только ключи или только значения

```go
for k := range m {
    fmt.Println(k)
}

for _, v := range m {
    fmt.Println(v)
}
```

## Детерминированный порядок

Если нужен фиксированный порядок — отсортируйте ключи отдельно:

```go
import "sort"

keys := make([]string, 0, len(m))
for k := range m {
    keys = append(keys, k)
}
sort.Strings(keys)

for _, k := range keys {
    fmt.Printf("%s: %d\n", k, m[k])
}
```

## Карта из срезов

Для группировки элементов:

```go
groups := make(map[string][]string)
groups["fruits"] = append(groups["fruits"], "apple", "banana")
groups["vegs"]   = append(groups["vegs"], "carrot")
```

---

## Упражнения

| №  | Задание      | Сложность |
|----|--------------|-----------|
| 1  | Упражнение 1 | easy      |
| 2  | Упражнение 2 | medium    |

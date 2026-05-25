---
series: ["algorithms"]
title:  "Эффективная обработка больших данных"
weight: 3
---

## Потоковая обработка

Вместо загрузки всех данных в память — обрабатывай по частям:

```go
import (
    "bufio"
    "os"
)

func processLines(filename string) error {
    f, err := os.Open(filename)
    if err != nil { return err }
    defer f.Close()

    scanner := bufio.NewScanner(f)
    for scanner.Scan() {
        line := scanner.Text()
        process(line) // O(1) памяти на строку
    }
    return scanner.Err()
}
```

## Переиспользование срезов

```go
// Плохо: новая аллокация на каждой итерации
for _, chunk := range chunks {
    result := make([]int, 0)
    result = transform(chunk, result)
}

// Хорошо: переиспользуем буфер
buf := make([]int, 0, 1024)
for _, chunk := range chunks {
    buf = buf[:0]              // сброс длины без освобождения памяти
    buf = transform(chunk, buf)
    process(buf)
}
```

## Параллельная обработка

```go
import "sync"

func parallelProcess(data []int, workers int) []int {
    chunkSize := (len(data) + workers - 1) / workers
    results := make([][]int, workers)
    var wg sync.WaitGroup

    for i := 0; i < workers; i++ {
        i := i
        lo := i * chunkSize
        hi := min(lo+chunkSize, len(data))
        wg.Add(1)
        go func() {
            defer wg.Done()
            results[i] = processChunk(data[lo:hi])
        }()
    }
    wg.Wait()

    out := make([]int, 0, len(data))
    for _, r := range results { out = append(out, r...) }
    return out
}
```

## sort.Search для отсортированных данных

```go
// Бинарный поиск нижней границы в отсортированном срезе
nums := []int{1, 3, 5, 7, 9, 11}
target := 7
i := sort.SearchInts(nums, target)
if i < len(nums) && nums[i] == target {
    fmt.Println("найден на позиции", i)
}
```

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | medium    |

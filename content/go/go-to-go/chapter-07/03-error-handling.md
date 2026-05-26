---
series: ["go-to-go"]
title:  "Обработка ошибок"
weight: 3
---

## error — встроенный интерфейс

В Go нет исключений. Ошибка — обычное значение, которое функция возвращает последним:

```go
type error interface {
    Error() string
}
```

## Создание ошибок

```go
import "errors"

var ErrNotFound = errors.New("not found")

func find(id int) (string, error) {
    if id < 0 {
        return "", fmt.Errorf("некорректный id: %d", id)
    }
    if id > 100 {
        return "", ErrNotFound
    }
    return "item", nil
}
```

## Проверка и оборачивание

Ошибки можно оборачивать (`%w`), сохраняя цепочку:

```go
if err := doSomething(); err != nil {
    return fmt.Errorf("шаг X: %w", err)
}
```

Развернуть цепочку:

```go
if errors.Is(err, ErrNotFound) {
    // обработка конкретной ошибки
}

var e *PathError
if errors.As(err, &e) {
    fmt.Println(e.Path)
}
```

## panic и recover

`panic` — нештатное завершение. Используется только для программных ошибок (инвариант нарушен). Пользовательский ввод, сетевые сбои — это `error`, не `panic`.

`recover` перехватывает панику внутри `defer`:

```go
func safe(f func()) (err error) {
    defer func() {
        if r := recover(); r != nil {
            err = fmt.Errorf("паника: %v", r)
        }
    }()
    f()
    return
}
```

---

## Упражнения

| №  | Задание      | Сложность |
|----|--------------|-----------|
| 1  | Упражнение 1 | easy      |
| 2  | Упражнение 2 | medium    |
| 3  | Упражнение 3 | medium    |

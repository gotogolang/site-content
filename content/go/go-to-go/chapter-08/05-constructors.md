---
series: ["go-to-go"]
title:  "Конструкторы и фабрики"
weight: 5
---

## Go не имеет конструкторов

Вместо них — обычные функции с именем `New...` или `Make...`:

```go
type Server struct {
    host    string
    port    int
    timeout time.Duration
}

func NewServer(host string, port int) *Server {
    return &Server{
        host:    host,
        port:    port,
        timeout: 30 * time.Second,
    }
}
```

Функция-конструктор устанавливает значения по умолчанию и гарантирует корректное начальное состояние.

## Функциональные опции

Для структур с множеством настроек — паттерн functional options:

```go
type Option func(*Server)

func WithTimeout(d time.Duration) Option {
    return func(s *Server) { s.timeout = d }
}

func NewServerWith(host string, port int, opts ...Option) *Server {
    s := &Server{host: host, port: port, timeout: 30 * time.Second}
    for _, o := range opts {
        o(s)
    }
    return s
}

srv := NewServerWith("localhost", 8080,
    WithTimeout(60 * time.Second),
)
```

## Фабричная функция

Когда один конструктор создаёт разные реализации интерфейса:

```go
type Storage interface{ Save(data []byte) error }

func NewStorage(kind string) Storage {
    switch kind {
    case "s3":
        return &S3Storage{}
    case "disk":
        return &DiskStorage{}
    default:
        return &MemStorage{}
    }
}
```

---

## Упражнения

| №  | Задание      | Сложность |
|----|--------------|-----------|
| 1  | Упражнение 1 | easy      |
| 2  | Упражнение 2 | medium    |

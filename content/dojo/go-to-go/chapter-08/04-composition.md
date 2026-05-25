---
series: ["go-to-go"]
title:  "Композиция вместо наследования"
weight: 4
---

## Принцип

В Go нет классового наследования. Вместо него — встраивание (embedding) и интерфейсы.

Встраивание структуры «поднимает» её поля и методы:

```go
type Logger struct{}
func (l Logger) Log(msg string) { fmt.Println("[log]", msg) }

type Service struct {
    Logger           // встраивание
    Name string
}

s := Service{Name: "auth"}
s.Log("started")    // вызывает Logger.Log
```

Это не наследование: `Service` не является `Logger`. Но пользоваться его методами удобно.

## Реализация интерфейса через встраивание

```go
type ReadWriter interface {
    Read() string
    Write(s string)
}

type Buffer struct {
    data string
}
func (b *Buffer) Read() string    { return b.data }
func (b *Buffer) Write(s string)  { b.data = s }

type LoggedBuffer struct {
    *Buffer            // встраивание по указателю
    Logger
}

// LoggedBuffer реализует ReadWriter через Buffer
```

## Переопределение метода

Встроенный метод можно переопределить:

```go
func (lb LoggedBuffer) Write(s string) {
    lb.Log("write: " + s)
    lb.Buffer.Write(s)   // явный вызов встроенного метода
}
```

---

## Упражнения

| №  | Задание      | Сложность |
|----|--------------|-----------|
| 1  | Упражнение 1 | medium    |
| 2  | Упражнение 2 | medium    |

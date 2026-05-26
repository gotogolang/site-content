---
series: ["algorithms"]
title:  "Классические алгоритмы поиска по шаблону"
weight: 2
---

Задача: найти все вхождения строки-шаблона `p` (длина `m`) в текст `t` (длина `n`).

## Brute Force

Сравнивает шаблон с каждой позицией текста. `O(n·m)`.

```go
func bruteForce(text, pattern string) []int {
    var result []int
    n, m := len(text), len(pattern)
    for i := 0; i <= n-m; i++ {
        if text[i:i+m] == pattern {
            result = append(result, i)
        }
    }
    return result
}
```

## Алгоритм Рабина-Карпа

Вычисляет хеш шаблона, затем скользит по тексту, сравнивая хеши. При совпадении — дополнительная проверка строк. Среднее `O(n+m)`, худшее `O(n·m)`.

Ключевой приём — **rolling hash**: хеш следующего окна вычисляется за `O(1)` из предыдущего.

```go
const base, mod = 31, 1_000_000_007

func rabinKarp(text, pattern string) []int {
    n, m := len(text), len(pattern)
    ph := hash(pattern)
    wh := hash(text[:m])
    pow := 1
    for i := 0; i < m-1; i++ { pow = pow * base % mod }

    var result []int
    if wh == ph && text[:m] == pattern { result = append(result, 0) }
    for i := 1; i <= n-m; i++ {
        wh = (wh - int(text[i-1]-'a'+1)*pow%mod + mod) % mod
        wh = (wh*base + int(text[i+m-1]-'a'+1)) % mod
        if wh == ph && text[i:i+m] == pattern {
            result = append(result, i)
        }
    }
    return result
}

func hash(s string) int {
    h := 0
    for _, c := range s { h = (h*base + int(c-'a'+1)) % mod }
    return h
}
```

## Алгоритм Кнута-Морриса-Пратта (KMP)

Предвычисляет таблицу «провалов» (failure function) — для каждой позиции шаблона показывает, куда откатиться при несовпадении. `O(n+m)`, без дополнительных сравнений.

```go
func kmpSearch(text, pattern string) []int {
    lps := buildLPS(pattern)
    var result []int
    j := 0
    for i := 0; i < len(text); {
        if text[i] == pattern[j] { i++; j++ }
        if j == len(pattern) {
            result = append(result, i-j)
            j = lps[j-1]
        } else if i < len(text) && text[i] != pattern[j] {
            if j != 0 { j = lps[j-1] } else { i++ }
        }
    }
    return result
}

func buildLPS(p string) []int {
    lps := make([]int, len(p))
    length, i := 0, 1
    for i < len(p) {
        if p[i] == p[length] { length++; lps[i] = length; i++ } else if length != 0 { length = lps[length-1] } else { i++ }
    }
    return lps
}
```

## Алгоритм Бойера-Мура (BM)

Сравнивает шаблон справа налево, использует две эвристики для больших пропусков. На практике быстрее KMP для длинных шаблонов и больших алфавитов. `O(n/m)` в лучшем случае.

Две таблицы:
- **Bad character** — при несовпадении сдвиг по последнему вхождению символа в шаблоне.
- **Good suffix** — сдвиг по уже совпавшему суффиксу.

## Алгоритм Ахо-Корасик

Строит **автомат** для одновременного поиска множества шаблонов. `O(n + Σ|pi| + k)`, где k — число совпадений. Используется в антивирусах, системах фильтрации.

Строится как trie из всех шаблонов с добавлением fail-ссылок (аналог LPS у KMP).

## Сравнение

| Алгоритм       | Время           | Предобработка | Когда использовать                  |
|----------------|-----------------|---------------|-------------------------------------|
| Brute Force    | `O(n·m)`        | —             | Короткие строки, простота           |
| Rabin-Karp     | `O(n+m)` avg    | `O(m)`        | Несколько шаблонов, rolling hash    |
| KMP            | `O(n+m)`        | `O(m)`        | Один шаблон, гарантия               |
| Boyer-Moore    | `O(n/m)` best   | `O(m+σ)`      | Длинные шаблоны, большой алфавит    |
| Aho-Corasick   | `O(n+Σm+k)`     | `O(Σm)`       | Множество шаблонов одновременно     |

---

## Упражнения

| №  | Задание       | Сложность |
|----|---------------|-----------|
| 1  | Упражнение 1  | easy      |
| 2  | Упражнение 2  | easy      |
| 3  | Упражнение 3  | medium    |
| 4  | Упражнение 4  | medium    |

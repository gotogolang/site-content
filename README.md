# site-content

Публичный контент сайта goalgo — упражнения и блог в формате Hugo Markdown.

## Структура

```
content/
├── _index.md               — главная страница
├── exercises/
│   ├── _index.md           — страница раздела
│   └── go/
│       ├── _index.md
│       ├── go-expr-01.md   — Multiplication Table (easy)
│       ├── go-func-01.md   — Reverse String (easy)
│       └── go-multi-01.md  — Package mathutils (medium)
└── blog/
    ├── _index.md
    └── hello-world.md
```

## Добавление упражнения

1. Создать `content/exercises/{lang}/{id}.md`
2. Указать в front matter: `pack`, `exercise`, `difficulty`, `tags`
3. Добавить шорткод `{{</* terminal pack="..." exercise="..." */>}}`
4. Пуш в `main` → GitHub Actions пересобирает сайт

## Связь с exercises репо

Упражнения на странице (условие, шорткод) живут здесь.
Файлы задачи (workspace, check.sh) — в репозитории `exercises`.

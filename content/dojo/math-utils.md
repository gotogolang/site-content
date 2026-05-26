---
title:      "Package mathutils"
pack:       "go"
exercise:   "go-multi-01"
difficulty: "medium"
tags:       ["packages", "slices", "testing"]
date:       2025-01-03
---

Build a `mathutils` package with three functions over integer slices:

| Function | Description |
|----------|-------------|
| `Max(nums []int) int` | maximum element |
| `Min(nums []int) int` | minimum element |
| `Sum(nums []int) int` | sum of elements |

Each function returns `0` for an empty slice.

Edit `mathutils/mathutils.go`. Run `check` or `go test ./...` to verify.

{{< terminal pack="go" exercise="go-multi-01" >}}

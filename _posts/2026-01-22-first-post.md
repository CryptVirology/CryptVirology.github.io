---
title: "Walking the PEB to Resolve APIs Without Imports"
date: 2026-01-22
---

## Goal

Avoid static imports to reduce user-mode detection.

## Background

Most EDR products hook common Win32 APIs such as `LoadLibrary` and
`GetProcAddress`.

## Approach

Instead of using the import table, the loader walks the PEB to locate
`kernel32.dll` and resolve exports manually.

## Implementation Notes

One mistake I made initially was assuming the module list order was
stable across versions.

```c
// PEB -> Ldr -> InMemoryOrderModuleList

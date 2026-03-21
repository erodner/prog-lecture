---
title: "Nützliche Array-Methoden"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Sortieren, suchen, kopieren – für viele typische Aufgaben rund um Arrays muss man das Rad nicht neu erfinden. Die .NET-Klassenbibliothek liefert mit der `Array`-Klasse fertige Methoden, die zuverlässig und effizient arbeiten.

## Sortieren

```csharp
int[] zahlen = { 5, 2, 8, 1, 9, 3 };
Array.Sort(zahlen);

foreach (int z in zahlen)
    Console.Write(z + " ");
// 1 2 3 5 8 9

// Rückwärts sortieren:
Array.Reverse(zahlen);
// 9 8 5 3 2 1
```

## Suchen

```csharp
int[] sortiert = { 1, 3, 5, 7, 9, 11 };

// Lineares Suchen (unsortiertes Array):
int idx = Array.IndexOf(sortiert, 7);
Console.WriteLine(idx); // 3

// Binäres Suchen (nur sortierte Arrays!):
int pos = Array.BinarySearch(sortiert, 9);
Console.WriteLine(pos); // 4
```

## Kopieren

```csharp
int[] original = { 1, 2, 3, 4, 5 };
int[] kopie = new int[original.Length];

Array.Copy(original, kopie, original.Length);
kopie[0] = 99; // original bleibt unverändert

// Kurzschreibweise:
int[] kopie2 = (int[])original.Clone();
```

Achtung: `kopie = original` kopiert **nicht** den Inhalt – beide Variablen zeigen dann auf dasselbe Array!
{: .notice--warning}

## Füllen

```csharp
int[] nullen = new int[5];
Array.Fill(nullen, -1);
// [-1, -1, -1, -1, -1]
```

## Übersicht

| Methode | Beschreibung |
| :--- | :--- |
| `Array.Sort(arr)` | Aufsteigend sortieren |
| `Array.Reverse(arr)` | Reihenfolge umkehren |
| `Array.IndexOf(arr, val)` | Index des ersten Vorkommens |
| `Array.BinarySearch(arr, val)` | Suche in sortiertem Array |
| `Array.Copy(src, dst, n)` | Erste n Elemente kopieren |
| `Array.Fill(arr, val)` | Alle Elemente auf Wert setzen |
| `arr.Length` | Anzahl der Elemente |

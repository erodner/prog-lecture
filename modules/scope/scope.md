---
title: "Gültigkeitsbereiche und Parameterübergabe"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Warum kann man auf eine Variable in einer Methode nicht von einer anderen aus zugreifen? Und warum bleibt eine Zahl nach dem Methodenaufruf manchmal unverändert, obwohl sie darin verändert wurde? Diese Fragen beantwortet das Konzept der Gültigkeitsbereiche und der Parameterübergabe.

## Gültigkeitsbereiche (Scope)

Eine Variable existiert nur innerhalb des Blocks `{ }`, in dem sie deklariert wurde:

```csharp
void Beispiel()
{
    int x = 10; // x ist hier sichtbar

    if (x > 5)
    {
        int y = 20; // y ist nur hier sichtbar
        Console.WriteLine(x + y); // OK
    }

    Console.WriteLine(y); // Compilerfehler! y ist hier nicht sichtbar
}
```

## Parameterübergabe: Wert vs. Referenz

### Call by Value (Standard)

Primitive Typen (`int`, `double`, `bool`, …) werden als **Kopie** übergeben:

```csharp
static void Verdoppeln(int x)
{
    x *= 2;
    Console.WriteLine($"In Methode: {x}"); // 20
}

int zahl = 10;
Verdoppeln(zahl);
Console.WriteLine($"Nach Aufruf: {zahl}"); // 10 – unverändert!
```

### Call by Reference mit `ref`

```csharp
static void Verdoppeln(ref int x)
{
    x *= 2;
}

int zahl = 10;
Verdoppeln(ref zahl);
Console.WriteLine(zahl); // 20 – geändert!
```

### Ausgabe-Parameter mit `out`

Wenn eine Methode mehrere Werte zurückgeben soll:

```csharp
static void MinMax(int[] arr, out int min, out int max)
{
    min = arr[0];
    max = arr[0];
    foreach (int x in arr)
    {
        if (x < min) min = x;
        if (x > max) max = x;
    }
}

int[] zahlen = { 3, 1, 7, 2, 9 };
MinMax(zahlen, out int minimum, out int maximum);
Console.WriteLine($"Min: {minimum}, Max: {maximum}"); // Min: 1, Max: 9
```

---
title: "Eindimensionale Arrays"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Fünf Noten, sieben Wochentage, hundert Messwerte – sobald man viele gleichartige Daten verwalten muss, stößt man mit einzelnen Variablen schnell an Grenzen. Arrays sind die einfachste Lösung: eine geordnete Folge von Werten desselben Typs, auf die man per Index zugreift.

## Was ist ein Array?

Ein Array ist eine **feste Folge gleichartiger Werte** unter einem gemeinsamen Namen.

```
Index:  [0]  [1]  [2]  [3]  [4]
         ┌────┬────┬────┬────┬────┐
noten:   │ 2  │ 1  │ 3  │ 2  │ 4  │
         └────┴────┴────┴────┴────┘
```

## Deklaration und Initialisierung

```csharp
// Leer anlegen (Standardwert: 0 für int)
int[] noten = new int[5];

// Direkt befüllen
int[] noten = { 2, 1, 3, 2, 4 };
string[] wochentage = { "Mo", "Di", "Mi", "Do", "Fr" };
```

## Zugriff über Index

Indizes starten bei **0**:

```csharp
Console.WriteLine(noten[0]);     // 2 – erstes Element
Console.WriteLine(noten[4]);     // 4 – letztes Element
Console.WriteLine(noten.Length); // 5 – Anzahl Elemente

noten[2] = 5;  // Element verändern
```

## Array durchlaufen

```csharp
int[] werte = { 10, 30, 20, 50, 40 };

int min = werte[0];
for (int i = 1; i < werte.Length; i++)
    if (werte[i] < min)
        min = werte[i];

Console.WriteLine($"Minimum: {min}"); // 10
```

## Häufiger Fehler: Index außerhalb des Bereichs

```csharp
int[] arr = new int[3]; // Indizes 0, 1, 2
arr[3] = 5;             // Laufzeitfehler! IndexOutOfRangeException
```

## Werttypen vs. Referenztypen

Dieser Unterschied ist grundlegend und erklärt viele auf den ersten Blick überraschende Verhaltensweisen in C#.

**Werttypen** (`int`, `double`, `bool`, `char`, `struct`) speichern ihren Wert direkt in der Variable. Eine Zuweisung erzeugt eine vollständig unabhängige Kopie:

```csharp
int a = 10;
int b = a;   // b ist eine eigenständige Kopie
b = 99;

Console.WriteLine(a); // 10 – unverändert
Console.WriteLine(b); // 99
```

**Referenztypen** (Arrays, Klassen, `string`) speichern in der Variable nicht den Wert selbst, sondern eine **Referenz** – eine Adresse im Heap-Speicher, die auf den eigentlichen Inhalt zeigt. Eine Zuweisung kopiert nur die Referenz, nicht die Daten dahinter:

```csharp
int[] x = { 1, 2, 3 };
int[] y = x;     // y zeigt auf dasselbe Array wie x

y[0] = 99;

Console.WriteLine(x[0]); // 99 – x wurde mitverändert!
Console.WriteLine(y[0]); // 99
```

Es gibt nur **ein** Array im Speicher, aber **zwei** Variablen, die darauf zeigen:

```
Stack:          Heap:
x ──────────→  ┌────┬────┬────┐
               │ 99 │  2 │  3 │
y ──────────→  └────┴────┴────┘
```

Das Ändern von `y[0]` verändert denselben Speicherbereich, auf den auch `x` zeigt. Es gibt keine automatische Kopie.

Um ein Array wirklich zu duplizieren, muss man explizit eine Kopie anlegen:

```csharp
int[] x = { 1, 2, 3 };
int[] y = (int[])x.Clone();  // tiefe Kopie für primitive Elemente

y[0] = 99;

Console.WriteLine(x[0]); // 1 – unverändert
Console.WriteLine(y[0]); // 99
```

## Arrays als Methodenparameter

Weil Arrays Referenztypen sind, erhält eine Methode beim Aufruf die Referenz auf das originale Array – keine Kopie der Elemente. Jede Änderung an den Elementen innerhalb der Methode wirkt sich unmittelbar auf das Array des Aufrufers aus:

```csharp
static void VerdoppleAlle(int[] arr)
{
    for (int i = 0; i < arr.Length; i++)
        arr[i] *= 2;
}

int[] zahlen = { 1, 2, 3 };
VerdoppleAlle(zahlen);

// zahlen wurde durch die Methode verändert:
Console.WriteLine(zahlen[0]); // 2
Console.WriteLine(zahlen[1]); // 4
Console.WriteLine(zahlen[2]); // 6
```

Das ist ein fundamentaler Unterschied zu primitiven Typen wie `int` oder `double`, die als Kopie übergeben werden: Änderungen an einem `int`-Parameter haben keinen Einfluss auf die aufrufende Stelle – Änderungen an Array-Elementen hingegen schon.

Genau genommen wird auch bei Arrays die Referenz als Kopie übergeben (call by value der Referenz). Das bedeutet: Die Methode kann die Elemente des Arrays verändern, aber sie kann die Variable des Aufrufers nicht auf ein anderes Array umbiegen – dafür wäre `ref` nötig.
{: .notice--primary}

Übung: Lies 5 Noten ein, speichere sie in einem Array und berechne Minimum, Maximum und Durchschnitt. Schreibe außerdem eine Methode `Normalisiere(double[] werte)`, die alle Werte so skaliert, dass das Maximum 1.0 ergibt, und prüfe, ob das originale Array verändert wurde.
{: .notice--info}

## Weitere Quellen

- [Eindimensionale Arrays – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/arrays/single-dimensional-arrays)
- [Sortieralgorithmen visualisiert – VisuAlgo](https://visualgo.net/en/sorting)

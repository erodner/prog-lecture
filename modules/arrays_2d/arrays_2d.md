---
title: "Mehrdimensionale Arrays"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Manchmal sind Daten von Natur aus tabellarisch – Noten pro Studierendem und Prüfung, Temperaturen nach Tag und Stunde, Spielfelder in einem Brettspiel. Für solche Strukturen bietet C# mehrdimensionale Arrays, die sich wie eine Tabelle mit Zeilen und Spalten verhalten.

## Zweidimensionale Arrays

Ein 2D-Array entspricht einer Tabelle mit Zeilen und Spalten.

```
          Spalte 0  Spalte 1  Spalte 2
Zeile 0:    1         2         3
Zeile 1:    4         5         6
Zeile 2:    7         8         9
```

```csharp
int[,] tabelle = {
    { 1, 2, 3 },
    { 4, 5, 6 },
    { 7, 8, 9 }
};

Console.WriteLine(tabelle[0, 0]); // 1  – erste Zeile, erste Spalte
Console.WriteLine(tabelle[1, 2]); // 6  – zweite Zeile, dritte Spalte
Console.WriteLine(tabelle.GetLength(0)); // 3 – Anzahl Zeilen
Console.WriteLine(tabelle.GetLength(1)); // 3 – Anzahl Spalten
```

## Durchlaufen mit verschachtelten Schleifen

```csharp
for (int zeile = 0; zeile < tabelle.GetLength(0); zeile++)
{
    for (int spalte = 0; spalte < tabelle.GetLength(1); spalte++)
        Console.Write($"{tabelle[zeile, spalte],4}");
    Console.WriteLine();
}
```

## Praxisbeispiel: Notentabelle

```csharp
// 3 Studierende, 4 Prüfungen
double[,] noten = {
    { 1.7, 2.0, 1.3, 2.3 },
    { 2.7, 3.0, 2.0, 1.7 },
    { 1.0, 1.3, 1.7, 2.0 }
};

string[] namen = { "Anna", "Ben", "Clara" };

for (int s = 0; s < noten.GetLength(0); s++)
{
    double summe = 0;
    for (int p = 0; p < noten.GetLength(1); p++)
        summe += noten[s, p];
    Console.WriteLine($"{namen[s]}: Ø {summe / noten.GetLength(1):F2}");
}
```

## Gezackte Arrays (jagged arrays)

Arrays, bei denen jede Zeile eine andere Länge haben kann:

```csharp
int[][] dreieck = new int[4][];
for (int i = 0; i < dreieck.Length; i++)
{
    dreieck[i] = new int[i + 1];
    for (int j = 0; j <= i; j++)
        dreieck[i][j] = j + 1;
}
// [[1], [1,2], [1,2,3], [1,2,3,4]]
```

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

Das Sortieren eines Arrays ist eine der häufigsten Operationen. Statt einen eigenen Sortieralgorithmus zu schreiben, nutzt man `Array.Sort()` — die Methode sortiert das Array **direkt** (in-place), d.h. das Original wird verändert, es entsteht kein neues Array:

```csharp
int[] zahlen = { 5, 2, 8, 1, 9, 3 };
Array.Sort(zahlen);

foreach (int z in zahlen)
    Console.Write(z + " ");
// 1 2 3 5 8 9

// Rückwärts sortieren: erst aufsteigend sortieren, dann umkehren
Array.Reverse(zahlen);
// 9 8 5 3 2 1
```

`Array.Sort` und `Array.Reverse` verändern das übergebene Array. Wenn man das Original behalten möchte, muss man vorher eine Kopie anlegen.
{: .notice--primary}

## Suchen

Zum Suchen eines Werts gibt es zwei Methoden. `Array.IndexOf` durchsucht das Array von vorne nach hinten und liefert den Index des ersten Treffers — oder `-1`, wenn der Wert nicht gefunden wird. `Array.BinarySearch` ist deutlich schneller, setzt aber voraus, dass das Array **bereits sortiert** ist:

```csharp
int[] sortiert = { 1, 3, 5, 7, 9, 11 };

// Lineares Suchen — funktioniert immer:
int idx = Array.IndexOf(sortiert, 7);
Console.WriteLine(idx); // 3

// Binäres Suchen — nur bei sortierten Arrays!
int pos = Array.BinarySearch(sortiert, 9);
Console.WriteLine(pos); // 4
```

Der Unterschied in der Geschwindigkeit wird bei großen Arrays spürbar: Lineares Suchen prüft im schlimmsten Fall jedes Element (bei 1 Million Elementen also 1 Million Vergleiche). Binäres Suchen halbiert den Suchbereich mit jedem Schritt und braucht nur etwa 20 Vergleiche für 1 Million Elemente.

## Kopieren

Wie wir beim Thema Werttypen und Referenztypen gesehen haben, kopiert eine einfache Zuweisung `kopie = original` bei Arrays nur die Referenz. Für eine echte, unabhängige Kopie gibt es `Array.Copy` und `Clone()`:

```csharp
int[] original = { 1, 2, 3, 4, 5 };
int[] kopie = new int[original.Length];

Array.Copy(original, kopie, original.Length);
kopie[0] = 99; // original bleibt unverändert

// Kurzschreibweise:
int[] kopie2 = (int[])original.Clone();
```

`Array.Copy` ist flexibler — man kann damit auch nur einen Teilbereich kopieren oder an eine bestimmte Position im Zielarray schreiben. `Clone()` kopiert immer das gesamte Array.

## Füllen

`Array.Fill` setzt alle Elemente eines Arrays auf denselben Wert. Das ist nützlich, wenn der Standardwert `0` nicht passt — etwa wenn man ein Array mit `-1` als „kein Wert"-Kennzeichen initialisieren will:

```csharp
int[] werte = new int[5];
Array.Fill(werte, -1);
// [-1, -1, -1, -1, -1]
```

Übung: Erstelle ein Array mit 10 Zufallszahlen, sortiere es und suche mit `BinarySearch` nach einem bestimmten Wert. Was passiert, wenn der Wert nicht im Array vorkommt?
{: .notice--info}

## Weitere Quellen

- [Array-Klasse – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/api/system.array)
- [Array.Sort – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/api/system.array.sort)
- [Binäre Suche visualisiert – VisuAlgo](https://visualgo.net/en/bst)

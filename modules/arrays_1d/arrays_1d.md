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

Ein Array ist eine **feste Folge gleichartiger Werte** unter einem gemeinsamen Namen. Man kann sich ein Array wie eine nummerierte Reihe von Schließfächern vorstellen — jedes Fach hat eine Nummer (den **Index**) und enthält genau einen Wert desselben Typs:

```
Index:  [0]  [1]  [2]  [3]  [4]
         ┌────┬────┬────┬────┬────┐
noten:   │ 2  │ 1  │ 3  │ 2  │ 4  │
         └────┴────┴────┴────┴────┘
```

Zwei wichtige Eigenschaften: Die Größe eines Arrays wird bei der Erstellung festgelegt und kann danach **nicht mehr geändert** werden. Und alle Elemente müssen denselben Typ haben — ein `int[]` kann nur `int`-Werte enthalten.

## Deklaration und Initialisierung

Es gibt zwei Wege, ein Array zu erstellen. Entweder man legt es mit einer festen Größe an — dann werden alle Elemente automatisch mit dem Standardwert des Typs gefüllt (`0` für `int`, `0.0` für `double`, `null` für Strings). Oder man gibt die Werte direkt an:

```csharp
// Leer anlegen (Standardwert: 0 für int)
int[] noten = new int[5];

// Direkt befüllen
int[] noten = { 2, 1, 3, 2, 4 };
string[] wochentage = { "Mo", "Di", "Mi", "Do", "Fr" };
```

Bei der zweiten Variante ergibt sich die Größe automatisch aus der Anzahl der angegebenen Werte. Die geschweiften Klammern `{ }` sind hier eine Kurzschreibweise für die Initialisierung — sie funktioniert nur direkt bei der Deklaration.

## Zugriff über Index

Jedes Element wird über seinen Index angesprochen. Indizes starten in C# (wie in den meisten Programmiersprachen) bei **0** — das erste Element hat also Index 0, das letzte Index `Length - 1`:

```csharp
Console.WriteLine(noten[0]);     // 2 — erstes Element
Console.WriteLine(noten[4]);     // 4 — letztes Element
Console.WriteLine(noten.Length); // 5 — Anzahl Elemente

noten[2] = 5;  // Element verändern
```

Die Eigenschaft `Length` liefert die Anzahl der Elemente. Man verwendet sie häufig als Obergrenze in Schleifen, um alle Elemente zu durchlaufen.

## Array durchlaufen

Ein typisches Muster ist das Durchlaufen eines Arrays mit einer `for`-Schleife, um einen bestimmten Wert zu finden. Im folgenden Beispiel wird das Minimum gesucht: Man startet mit dem ersten Element als vorläufiges Minimum und vergleicht es der Reihe nach mit allen weiteren Elementen:

```csharp
int[] werte = { 10, 30, 20, 50, 40 };

int min = werte[0];
for (int i = 1; i < werte.Length; i++)
    if (werte[i] < min)
        min = werte[i];

Console.WriteLine($"Minimum: {min}"); // 10
```

Die Schleife startet bei Index `1` (nicht `0`), weil das Element an Index `0` bereits als Startwert dient. Das ist ein häufiges Muster bei Minimum/Maximum-Suchen.

## Häufiger Fehler: Index außerhalb des Bereichs

Greift man auf einen Index zu, der nicht existiert, stürzt das Programm mit einer `IndexOutOfRangeException` ab. Das passiert besonders leicht, wenn man vergisst, dass der letzte gültige Index `Length - 1` ist:

```csharp
int[] arr = new int[3]; // Indizes 0, 1, 2
arr[3] = 5;             // Laufzeitfehler! IndexOutOfRangeException
```

Dieser Fehler tritt häufig bei Schleifen auf, wenn man `<=` statt `<` verwendet: `for (int i = 0; i <= arr.Length; i++)` greift im letzten Durchlauf auf einen ungültigen Index zu.
{: .notice--warning}

Übung: Lies 5 Noten ein, speichere sie in einem Array und berechne Minimum, Maximum und Durchschnitt.
{: .notice--info}

## Weitere Quellen

- [Eindimensionale Arrays – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/arrays/single-dimensional-arrays)
- [Arrays (Tutorial) – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/tour-of-csharp/tutorials/arrays-and-collections)
- [Sortieralgorithmen visualisiert – VisuAlgo](https://visualgo.net/en/sorting)

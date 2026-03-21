---
title: "foreach-Schleife"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Wenn man alle Elemente einer Sammlung der Reihe nach verarbeiten möchte – und dabei keinen Index braucht – ist `foreach` die klarste und sicherste Wahl. Sie nimmt einem die Verwaltung des Zählers ab und macht den Code ausdrucksstärker.

## Syntax

```csharp
foreach (Typ element in Sammlung)
{
    // element enthält das aktuelle Element
}
```

`foreach` iteriert automatisch über alle Elemente einer Sammlung – ohne Zähler, ohne Indexverwaltung.

## Beispiel: Array durchlaufen

```csharp
string[] städte = { "Berlin", "Hamburg", "München", "Köln" };

foreach (string stadt in städte)
{
    Console.WriteLine(stadt);
}
```

Vergleich mit `for`:
```csharp
// Äquivalent mit for:
for (int i = 0; i < städte.Length; i++)
    Console.WriteLine(städte[i]);
```

`foreach` ist lesbarer, wenn der Index nicht gebraucht wird.

## Beispiel: Summe einer Zahlenreihe

```csharp
int[] werte = { 3, 7, 2, 9, 1, 5 };
int summe = 0;

foreach (int w in werte)
    summe += w;

Console.WriteLine($"Summe: {summe}"); // 27
```

## Einschränkung

Mit `foreach` kann man Elemente einer Sammlung nur **lesen**, nicht **verändern**:

```csharp
int[] zahlen = { 1, 2, 3 };
foreach (int z in zahlen)
    z *= 2; // Compilerfehler! z ist eine Kopie, keine Referenz
```

Sollen Elemente verändert werden, muss `for` mit Index verwendet werden.
{: .notice--warning}

Übung: Gegeben eine Liste von Noten – berechne mit `foreach` den Durchschnitt.
{: .notice--info}

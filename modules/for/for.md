---
title: "for-Schleife"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Wenn man genau weiß, wie viele Durchläufe eine Schleife machen soll – etwa zehn Mal rechnen oder alle 100 Einträge einer Liste durchgehen – ist die `for`-Schleife die eleganteste Wahl. Sie fasst Zähler, Bedingung und Schritt in einer übersichtlichen Zeile zusammen.

## Syntax

```csharp
for (Initialisierung; Bedingung; Aktualisierung)
{
    // Schleifenrumpf
}
```

Die `for`-Schleife fasst Initialisierung, Bedingung und Aktualisierung des Zählers in einer Zeile zusammen.

## Grundbeispiel

```csharp
for (int i = 0; i < 5; i++)
{
    Console.WriteLine($"Durchlauf {i}");
}
// Durchlauf 0, 1, 2, 3, 4
```

## Rückwärts zählen

```csharp
for (int i = 10; i >= 1; i--)
    Console.Write(i + " ");
// 10 9 8 7 6 5 4 3 2 1
```

## Summe berechnen

```csharp
int summe = 0;
for (int i = 1; i <= 100; i++)
    summe += i;
Console.WriteLine($"Summe 1 bis 100: {summe}"); // 5050
```

## Schrittweite anpassen

```csharp
// Alle geraden Zahlen von 0 bis 20
for (int i = 0; i <= 20; i += 2)
    Console.Write(i + " ");
// 0 2 4 6 8 10 12 14 16 18 20
```

## Muster ausgeben

```csharp
// Dreieck aus Sternen
for (int zeile = 1; zeile <= 5; zeile++)
{
    for (int stern = 1; stern <= zeile; stern++)
        Console.Write("*");
    Console.WriteLine();
}
// *
// **
// ***
// ****
// *****
```

Übung: Gib die Multiplikationstabelle von 1 bis 10 mit zwei verschachtelten `for`-Schleifen aus.
{: .notice--info}

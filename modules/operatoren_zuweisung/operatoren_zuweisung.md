---
title: "Zuweisungsoperatoren"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Punkte sammeln, Guthaben abziehen, einen Zähler erhöhen – im Code laufen solche Operationen ständig auf dasselbe Muster hinaus: eine Variable wird gelesen, verändert und wieder gespeichert. Die kombinierten Zuweisungsoperatoren von C# machen das kürzer und lesbarer.

## Einfache Zuweisung

```csharp
int x = 10;   // Wert 10 wird in x gespeichert
x = x + 5;   // x wird um 5 erhöht → x ist jetzt 15
```

## Kombinierte Zuweisungsoperatoren

Diese sind Kurzschreibweisen für häufige Muster:

```csharp
int n = 10;

n += 5;   // n = n + 5  → 15
n -= 3;   // n = n - 3  → 12
n *= 2;   // n = n * 2  → 24
n /= 4;   // n = n / 4  → 6
n %= 4;   // n = n % 4  → 2
```

## Praxisbeispiel: Punkte in einem Spiel

```csharp
int punkte = 0;

punkte += 10;   // Münze aufgesammelt
punkte += 50;   // Gegner besiegt
punkte -= 5;    // Schaden erhalten
punkte *= 2;    // Doppelpunkte-Bonus

Console.WriteLine($"Endpunktzahl: {punkte}"); // 110
```

## Weitere Quellen

- [Zuweisungsoperatoren – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/operators/assignment-operator)

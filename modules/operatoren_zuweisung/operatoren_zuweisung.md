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

Punkte sammeln, Guthaben abziehen, einen Zähler erhöhen – im Code laufen solche Operationen ständig auf dasselbe Muster hinaus: eine Variable wird gelesen, verändert und das Ergebnis wieder in derselben Variable gespeichert. C# bietet dafür Kurzschreibweisen, die den Code kürzer und lesbarer machen.

## Einfache Zuweisung

Das `=`-Zeichen in C# ist kein mathematisches Gleichheitszeichen, sondern ein **Zuweisungsoperator**: Es nimmt den Wert auf der rechten Seite und speichert ihn in der Variable auf der linken Seite. Die rechte Seite wird dabei zuerst vollständig ausgewertet:

```csharp
int x = 10;   // Wert 10 wird in x gespeichert
x = x + 5;    // rechte Seite: 10 + 5 = 15 → wird in x gespeichert
```

Die Zeile `x = x + 5` liest sich mathematisch unsinnig, ist aber in C# völlig normal: „Nimm den aktuellen Wert von `x`, addiere 5, und speichere das Ergebnis zurück in `x`."

## Kombinierte Zuweisungsoperatoren

Das Muster „lies Variable, verrechne etwas, speichere zurück" kommt so oft vor, dass C# dafür Kurzschreibweisen anbietet. `x += 5` bedeutet genau dasselbe wie `x = x + 5`, ist aber kürzer und signalisiert sofort: hier wird eine bestehende Variable verändert, nicht eine neue berechnet.

```csharp
int n = 10;

n += 5;   // n = n + 5  → 15
n -= 3;   // n = n - 3  → 12
n *= 2;   // n = n * 2  → 24
n /= 4;   // n = n / 4  → 6
n %= 4;   // n = n % 4  → 2
```

Es gibt einen kombinierten Operator für jede Grundrechenart. Das Muster ist immer gleich: `Variable OP= Wert` ist die Kurzform von `Variable = Variable OP Wert`.

## Praxisbeispiel: Punkte in einem Spiel

Kombinierte Zuweisungsoperatoren machen Code lesbarer, weil sofort klar ist, dass eine bestehende Variable verändert wird. In einem Spielpunkte-System liest sich das fast wie eine Beschreibung:

```csharp
int punkte = 0;

punkte += 10;   // Münze aufgesammelt
punkte += 50;   // Gegner besiegt
punkte -= 5;    // Schaden erhalten
punkte *= 2;    // Doppelpunkte-Bonus

Console.WriteLine($"Endpunktzahl: {punkte}"); // 110
```

Ohne die Kurzform müsste man jedes Mal `punkte = punkte + 10` schreiben — das ist länger und der Name `punkte` taucht doppelt auf, was bei längeren Variablennamen schnell unübersichtlich wird.

Übung: Deklariere eine Variable `kontostand` mit dem Wert 1000. Simuliere dann drei Transaktionen (Einzahlung, Abhebung, Zinsen von 2%) und gib den Endstand aus.
{: .notice--info}

## Weitere Quellen

- [Zuweisungsoperatoren – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/operators/assignment-operator)

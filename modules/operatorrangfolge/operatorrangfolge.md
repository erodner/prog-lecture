---
title: "Operatorrangfolge"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Punkt vor Strich kennt man aus der Schule – aber in C# gibt es noch viel mehr Operatoren, die alle eine festgelegte Priorität haben. Wer das ignoriert, baut schnell Berechnungen, die zwar syntaktisch korrekt sind, aber leise das falsche Ergebnis liefern.

## Punkt vor Strich – und mehr

C# wertet Ausdrücke nach festgelegter Priorität aus, ähnlich wie in der Mathematik:

```csharp
int ergebnis = 2 + 3 * 4;   // = 14, nicht 20
//                 ^^^  Multiplikation zuerst
```

## Rangfolge (vereinfacht, höchste zuerst)

| Priorität | Operatoren |
| :--- | :--- |
| 1 (höchste) | `++`, `--` (Postfix), Methodenaufruf `()` |
| 2 | `!`, `++`/`--` (Präfix), Cast `(Typ)` |
| 3 | `*`, `/`, `%` |
| 4 | `+`, `-` |
| 5 | `<`, `>`, `<=`, `>=` |
| 6 | `==`, `!=` |
| 7 | `&&` |
| 8 | `\|\|` |
| 9 (niedrigste) | `=`, `+=`, `-=`, … |

## Klammern setzen

Die Rangfolge aller Operatoren muss man sich nicht merken. Die einfachste Regel: **Im Zweifel klammern.** Das erhöht die Lesbarkeit und vermeidet Fehler — auch für den, der den Code Wochen später nochmal liest.
{: .notice--primary}

```csharp
// Unklar und fehleranfällig:
bool ok = x > 0 && y < 10 || z == 5;

// Klar und eindeutig:
bool ok = (x > 0 && y < 10) || (z == 5);
```

## Praxisbeispiel

```csharp
double bmi = gewicht / (groesse * groesse);  // Klammern nötig!
// Ohne Klammern: gewicht / groesse * groesse → falsch
```

Übung: Berechne ohne Computer: `10 - 2 * 3 + 4 / 2`. Prüfe danach mit C#.
{: .notice--info}

## Weitere Quellen

- [Operatoren und Ausdrücke (Rangfolge) – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/operators/)

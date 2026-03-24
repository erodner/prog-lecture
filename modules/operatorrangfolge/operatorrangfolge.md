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

Punkt vor Strich kennt man aus der Schule – aber in C# gibt es noch viel mehr Operatoren, die alle eine festgelegte Priorität haben. Wer das ignoriert, baut schnell Berechnungen, die zwar syntaktisch korrekt sind, aber leise das falsche Ergebnis liefern. Der Compiler meldet keinen Fehler — er rechnet einfach in der falschen Reihenfolge.

## Punkt vor Strich – und mehr

Genau wie in der Mathematik wertet C# Ausdrücke nach festgelegter Priorität aus. Multiplikation und Division kommen vor Addition und Subtraktion:

```csharp
int ergebnis = 2 + 3 * 4;   // = 14, nicht 20
//                 ^^^  Multiplikation zuerst
```

Wenn man `(2 + 3) * 4 = 20` meint, muss man Klammern setzen. Ohne Klammern entscheidet die Rangfolge — und die ist bei arithmetischen Operatoren noch intuitiv, wird aber bei Vergleichs- und Logikoperatoren schnell unübersichtlich.

## Rangfolge (vereinfacht, höchste zuerst)

Die folgende Tabelle zeigt die Rangfolge aller bisher bekannten Operatoren. Operatoren mit höherer Priorität werden zuerst ausgewertet. Operatoren auf derselben Stufe werden von links nach rechts ausgewertet:

| Priorität | Operatoren | Beispiel |
| :--- | :--- | :--- |
| 1 (höchste) | `++`, `--` (Postfix), Methodenaufruf `()` | `i++`, `Math.Max(a, b)` |
| 2 | `!`, `++`/`--` (Präfix), Cast `(Typ)` | `!fertig`, `(double)x` |
| 3 | `*`, `/`, `%` | `a * b`, `n % 2` |
| 4 | `+`, `-` | `a + b` |
| 5 | `<`, `>`, `<=`, `>=` | `alter >= 18` |
| 6 | `==`, `!=` | `x == 0` |
| 7 | `&&` | `a && b` |
| 8 | `\|\|` | `a \|\| b` |
| 9 (niedrigste) | `=`, `+=`, `-=`, … | `x = 10`, `n += 1` |

Einige Details, die sich aus der Tabelle ergeben: Vergleiche (Stufe 5–6) binden stärker als Logikoperatoren (Stufe 7–8) — deshalb funktioniert `alter >= 18 && hatAusweis` ohne Klammern um die Vergleiche. Und `&&` bindet stärker als `||`, genau wie `*` stärker als `+` bindet.

## Klammern setzen

Die Rangfolge aller Operatoren muss man sich nicht merken. Die einfachste Regel: **Im Zweifel klammern.** Das erhöht die Lesbarkeit und vermeidet Fehler — auch für den, der den Code Wochen später nochmal liest.
{: .notice--primary}

```csharp
// Unklar und fehleranfällig:
bool ok = x > 0 && y < 10 || z == 5;

// Klar und eindeutig:
bool ok = (x > 0 && y < 10) || (z == 5);
```

Beide Zeilen sind syntaktisch korrekt, aber nur die zweite ist sofort verständlich. In professionellem Code sieht man fast immer Klammern bei gemischten `&&`/`||`-Ausdrücken — nicht weil der Compiler sie braucht, sondern weil der nächste Leser sie braucht.

## Typische Fehler durch falsche Rangfolge

Die Operatorrangfolge führt zu einigen klassischen Fehlern, die der Compiler nicht erkennt:

```csharp
// Fehler 1: BMI-Berechnung
double bmi = gewicht / groesse * groesse;        // FALSCH: links nach rechts!
double bmi = gewicht / (groesse * groesse);      // RICHTIG: Klammern erzwingen Reihenfolge

// Fehler 2: Durchschnittsberechnung
double avg = a + b + c / 3;                      // FALSCH: nur c wird geteilt
double avg = (a + b + c) / 3.0;                  // RICHTIG

// Fehler 3: Negation einer Bedingung
bool ok = !x > 0;        // FALSCH: !x wird zuerst ausgewertet (ergibt Compilerfehler bei int)
bool ok = !(x > 0);      // RICHTIG: Klammern um den Vergleich
```

All diese Fehler lassen sich mit einer einfachen Faustregel vermeiden: Wenn ein Ausdruck mehr als einen Operator enthält und die Reihenfolge nicht offensichtlich ist — Klammern setzen.

Übung: Berechne ohne Computer: `10 - 2 * 3 + 4 / 2`. Prüfe danach mit C#. Setze dann Klammern so, dass das Ergebnis 6 wird.
{: .notice--info}

## Weitere Quellen

- [Operatoren und Ausdrücke (Rangfolge) – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/operators/)

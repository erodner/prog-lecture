---
title: "Fehlerarten in C#"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Nicht jeder Fehler im Code ist gleich – ein vergessenes Semikolon ist etwas völlig anderes als eine falsche Formel, die das Programm unbemerkt falscher Ergebnisse liefern lässt. Wer die drei Grundkategorien von Fehlern kennt, weiß sofort, wo er suchen muss.

Fehler im Code werden in drei grundlegende Kategorien eingeteilt.

## 1. Syntaxfehler

Der Code verstößt gegen die Grammatikregeln der Sprache. Der Compiler verweigert die Übersetzung.

```csharp
Console.WriteLine("Hallo"   // Fehler: schließende Klammer und Semikolon fehlen
int x = ;                   // Fehler: kein Wert zugewiesen
```

Syntaxfehler sind **am einfachsten zu finden** – Visual Studio markiert sie sofort rot.

## 2. Laufzeitfehler (Runtime Errors)

Der Code ist syntaktisch korrekt, aber beim Ausführen tritt ein Fehler auf.

```csharp
string eingabe = Console.ReadLine();
int zahl = int.Parse(eingabe); // Laufzeitfehler, wenn Benutzer "abc" eingibt

int[] zahlen = new int[3];
Console.WriteLine(zahlen[5]); // Laufzeitfehler: Index außerhalb des Bereichs
```

Laufzeitfehler führen zu einer **Exception** und brechen das Programm ab.

## 3. Logikfehler

Der Code läuft ohne Absturz durch, liefert aber **falsche Ergebnisse**.

```csharp
// Durchschnitt von drei Zahlen – aber falsch!
int a = 10, b = 20, c = 30;
double durchschnitt = a + b + c / 3; // Fehler: Punkt vor Strich!
Console.WriteLine(durchschnitt);     // Gibt 40 aus, nicht 20

// Richtig:
double korrekt = (a + b + c) / 3.0;
```

Logikfehler sind **am schwierigsten zu finden**, weil das Programm nicht abstürzt.
{: .notice--warning}

## Übersicht

| Fehlerart | Wann erkennbar | Beispiel |
| :--- | :--- | :--- |
| Syntaxfehler | Beim Kompilieren | Fehlendes `;` |
| Laufzeitfehler | Beim Ausführen | Division durch 0 |
| Logikfehler | Durch Testen | Falsche Formel |

Übung: Welche Fehlerart liegt vor? `int x = 10 / 0;` – Syntaxfehler, Laufzeitfehler oder Logikfehler?
{: .notice--info}

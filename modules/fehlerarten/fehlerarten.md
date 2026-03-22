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

## Der Debugger

Für Laufzeit- und Logikfehler ist der **Debugger** das wichtigste Werkzeug. Er erlaubt es, das Programm Schritt für Schritt auszuführen, an beliebigen Stellen anzuhalten (Breakpoints) und den aktuellen Wert jeder Variable zu inspizieren. Visual Studio hat einen vollständigen Debugger direkt eingebaut — eine Einführung bietet die [offizielle Dokumentation auf Microsoft Learn](https://learn.microsoft.com/de-de/visualstudio/debugger/debugger-feature-tour).

Übung: Welche Fehlerart liegt vor? `int x = 10 / 0;` – Syntaxfehler, Laufzeitfehler oder Logikfehler?
{: .notice--info}

## Weitere Quellen

- [Debugger-Einführung – Microsoft Learn](https://learn.microsoft.com/de-de/visualstudio/debugger/debugger-feature-tour)
- [Ausnahmen und Fehlerbehandlung – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/fundamentals/exceptions/)

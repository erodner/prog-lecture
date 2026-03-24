---
title: "Compilerfehler"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Compilerfehler sind die gute Nachricht: Der Compiler hat ein Problem gefunden, **bevor** das Programm überhaupt laufen konnte. Das Programm wird gar nicht erst in die Intermediate Language übersetzt — man muss den Fehler zuerst beheben. Die Fehlermeldungen sind dabei die präziseste Hilfe, die man beim Programmieren bekommen kann.

## Syntaxfehler

Der häufigste Grund für Compilerfehler: Der Code verstößt gegen die Grammatikregeln der Sprache.

```csharp
Console.WriteLine("Hallo"   // Fehler: schließende Klammer und Semikolon fehlen
int x = ;                   // Fehler: kein Wert zugewiesen
```

Syntaxfehler sind **am einfachsten zu finden** — Visual Studio markiert sie sofort rot, noch bevor man auf "Start" drückt.

## Aufbau einer Compiler-Fehlermeldung

Jede Meldung hat dasselbe Format:

```
Program.cs(5,42): error CS1002: ; expected
```

| Teil | Bedeutung |
| :--- | :--- |
| `Program.cs(5,42)` | Datei, Zeile 5, Spalte 42 |
| `error` | Schweregrad (`error` = geht nicht, `warning` = geht, aber fragwürdig) |
| `CS1002` | Fehlernummer — lässt sich direkt in einer Suchmaschine nachschlagen |
| `; expected` | Beschreibung des Problems |

In Visual Studio erscheinen dieselben Informationen in der **Fehlerliste** (Menü: Ansicht → Fehlerliste). Ein Doppelklick springt direkt zur betroffenen Stelle.

Immer den **ersten** Fehler in der Liste zuerst beheben. Ein einziger Fehler (z.B. eine fehlende Klammer) kann dutzende Folgefehler auslösen, die nach dem Fix alle verschwinden.
{: .notice--primary}

## Häufige Compilerfehler

### CS1002: `;` expected

```csharp
Console.WriteLine("Hallo")   // ← Semikolon fehlt
```

Jede Anweisung in C# muss mit `;` enden. Dieser Fehler zeigt oft auf die **nächste** Zeile, weil der Compiler dort erst merkt, dass etwas fehlt.

### CS0029: Cannot implicitly convert type

```csharp
int zahl = "42";   // error CS0029: string kann nicht implizit zu int werden
```

Die Typen passen nicht zusammen. Lösung: explizit konvertieren (`int.Parse("42")`) oder den richtigen Typ verwenden.

### CS0103: The name '...' does not exist in the current context

```csharp
Console.WriteLine(alter);   // error CS0103: 'alter' ist nicht bekannt
```

Die Variable wurde nicht deklariert — oder sie heißt anders (Tippfehler!). C# unterscheidet Groß- und Kleinschreibung: `Alter` und `alter` sind zwei verschiedene Namen.

### CS0165: Use of unassigned local variable

```csharp
int summe;
Console.WriteLine(summe);   // error CS0165: nicht zugewiesen
```

Lokale Variablen müssen vor der Verwendung einen Wert bekommen. Lösung: `int summe = 0;`

### CS0019: Operator cannot be applied to operands of type

```csharp
string name = "Anna";
int ergebnis = name + 10;   // error CS0019: + passt nicht für string → int
```

Ein Operator wird mit Typen verwendet, für die er nicht definiert ist.

### CS1501: No overload for method '...' takes N arguments

```csharp
Console.WriteLine("a", "b", "c");   // error CS1501: keine passende Überladung
```

Die Methode wurde mit der falschen Anzahl oder falschen Typen von Argumenten aufgerufen.

## Fehlermeldungen effektiv nachschlagen

Wenn eine Fehlermeldung unverständlich ist:

1. **Fehlernummer kopieren** (z.B. `CS0029`) und in einer Suchmaschine eingeben
2. **Die exakte Meldung** in Anführungszeichen suchen — oft findet man sofort einen StackOverflow-Thread mit genau demselben Problem
3. **Microsoft Learn** hat zu jeder CS-Fehlernummer eine eigene Seite mit Erklärung und Lösungsbeispielen

Übung: Erzeuge absichtlich einen `CS0029`-Fehler und einen `CS0103`-Fehler. Lies die Fehlermeldung und behebe das Problem.
{: .notice--info}

## Weitere Quellen

- [Compiler-Fehlermeldungen (CS-Nummern) – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/compiler-messages/)
- [C# Fehlercodes durchsuchen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/misc/)

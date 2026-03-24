---
title: "Fehlermeldungen verstehen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Fehlermeldungen sind keine Schikane — sie sind die präziseste Hilfe, die man beim Programmieren bekommen kann. Der Compiler sagt einem genau, was falsch ist und wo. Wer lernt, diese Meldungen zu lesen, spart sich stundenlanges Raten.

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

## Häufige Laufzeitfehler

Laufzeitfehler treten erst beim Ausführen auf — der Compiler kann sie nicht vorhersehen. Dies passiert zum Beispiel wenn ein Nutzer Werte eingibt, welche nicht verarbeitet werden können. Wir werden noch viel später kennenlernen wie man solche Fehler auch innerhalb des Programmes abfangen kann. Aktuell wird die Ausführung des Programms immer abgebrochen, wenn ein Laufzeitfehler (engl. *exception*) auftritt. 

### System.FormatException

```csharp
int zahl = int.Parse("abc");   // FormatException: "abc" ist keine Zahl
```

Ein String soll in eine Zahl konvertiert werden, hat aber kein gültiges Format. Lösung: `TryParse` verwenden.

### System.DivideByZeroException

```csharp
int a = 10, b = 0;
Console.WriteLine(a / b);   // DivideByZeroException
```

Division durch null bei ganzen Zahlen. Bei `double` liefert `10.0 / 0` dagegen `Infinity` — kein Fehler, aber auch selten gewollt.

### System.IndexOutOfRangeException

```csharp
int[] zahlen = new int[3];
Console.WriteLine(zahlen[5]);   // IndexOutOfRangeException
```

Zugriff auf einen Index, der nicht existiert. Arrays zählen ab 0 — ein Array der Länge 3 hat die Indizes 0, 1 und 2.

### System.NullReferenceException

```csharp
string text = null;
Console.WriteLine(text.Length);   // NullReferenceException
```

Ein Methodenaufruf oder Eigenschaftszugriff auf `null` — es gibt kein Objekt, auf dem man operieren könnte. Dieser Fehler ist einer der häufigsten überhaupt.

### System.OverflowException

```csharp
checked
{
    int x = int.MaxValue + 1;   // OverflowException
}
```

Ein Wert überschreitet den Bereich seines Typs. Ohne `checked` rollt der Wert still um — noch gefährlicher.

## Weitere Quellen

- [Compiler-Fehlermeldungen (CS-Nummern) – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/compiler-messages/)
- [Häufige Laufzeit-Exceptions – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/standard/exceptions/)
- [C# Fehlercodes durchsuchen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/misc/)

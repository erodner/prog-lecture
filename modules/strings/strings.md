---
title: "Strings in C#"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Text ist allgegenwärtig in Programmen — Benutzernamen, Fehlermeldungen, Sucheingaben, Ausgaben. Strings in C# verhalten sich dabei etwas anders als man vielleicht erwarten würde: sie sind unveränderlich, was einige praktische Konsequenzen hat, die man kennen sollte.

## Was ist ein String?

Ein `string` ist eine **unveränderliche** (engl. *immutable*) Zeichenkette. Das bedeutet: Wenn man eine String-Methode aufruft, die den Text verändert, wird immer ein **neuer** String erzeugt — der ursprüngliche bleibt unverändert:

```csharp
string name = "Anna";
string gross = name.ToUpper(); // "ANNA" — neuer String
Console.WriteLine(name);      // "Anna" — unverändert!
```

Das ist ein fundamentaler Unterschied zu vielen anderen Datentypen. Wenn man den Wert ändern will, muss man das Ergebnis explizit zurückspeichern: `name = name.ToUpper();`.

## Häufige String-Methoden

Die `string`-Klasse bringt eine Vielzahl nützlicher Methoden mit. Alle geben einen neuen String oder Wert zurück, ohne den Original-String zu verändern:

```csharp
string s = "  Hallo, Welt!  ";

s.Length                    // 16  – Anzahl Zeichen (Eigenschaft, keine Methode)
s.ToUpper()                 // "  HALLO, WELT!  "
s.ToLower()                 // "  hallo, welt!  "
s.Trim()                    // "Hallo, Welt!"  – Leerzeichen an beiden Enden entfernen
s.Contains("Welt")          // true – enthält der String diesen Teilstring?
s.StartsWith("Hallo")       // false (wegen Leerzeichen vorne!)
s.Trim().StartsWith("Hallo")// true – erst trimmen, dann prüfen
s.Replace("Welt", "C#")     // "  Hallo, C#!  " – alle Vorkommen ersetzen
s.Substring(2, 5)           // "Hallo" – ab Index 2, Länge 5
s.Split(',')                // ["  Hallo", " Welt!  "] – am Trennzeichen aufteilen
```

Methoden lassen sich verketten, weil jede Methode wieder einen String zurückgibt: `s.Trim().ToLower().Replace(" ", "-")` erzeugt `"hallo,-welt!"`.

`Length` ist eine **Eigenschaft**, keine Methode — deshalb ohne Klammern. String-Methoden dagegen haben immer Klammern, auch wenn sie keine Parameter brauchen: `s.ToUpper()`.
{: .notice--primary}

## String-Vergleich

Der `==`-Operator vergleicht Strings in C# nach ihrem **Inhalt**, nicht nach ihrer Speicheradresse — anders als in manchen anderen Sprachen (z.B. Java). Allerdings unterscheidet er Groß- und Kleinschreibung:

```csharp
string a = "Hallo";
string b = "hallo";

Console.WriteLine(a == b);   // false – Groß-/Kleinschreibung zählt
Console.WriteLine(a.Equals(b, StringComparison.OrdinalIgnoreCase)); // true
```

Für Benutzereingaben empfiehlt sich fast immer der Vergleich ohne Berücksichtigung der Groß-/Kleinschreibung — Benutzer tippen selten exakt das, was man erwartet.

## String-Verkettung

Strings lassen sich auf mehrere Arten zusammensetzen. Welche man wählt, hängt davon ab, wie viele Teile es sind und woher sie kommen:

```csharp
string vorname = "Anna";
string nachname = "Müller";

// Variante 1: + Operator — einfach und direkt für wenige Teile
string voll = vorname + " " + nachname;

// Variante 2: string.Join — ideal wenn man eine Liste mit Trennzeichen verbinden will
string csv = string.Join(", ", "rot", "grün", "blau"); // "rot, grün, blau"
```

Für viele Einzelteile (z.B. in einer Schleife) wird der `+`-Operator ineffizient, weil bei jeder Verkettung ein neuer String entsteht. `StringBuilder` löst das, indem er intern einen veränderbaren Puffer verwendet:

```csharp
var sb = new System.Text.StringBuilder();
for (int i = 0; i < 5; i++)
    sb.Append(i + " ");
Console.WriteLine(sb.ToString()); // "0 1 2 3 4 "
```

Für die meisten Alltagsfälle (wenige Verkettungen) reicht der `+`-Operator oder String-Interpolation (`$"..."`) völlig aus. `StringBuilder` wird erst bei hunderten oder tausenden Teilen spürbar schneller.

Übung: Lies einen Satz ein und gib die Anzahl der Wörter, die Anzahl der Zeichen (ohne Leerzeichen) und den Satz rückwärts aus.
{: .notice--info}

## Weitere Quellen

- [Strings in C# – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/strings/)
- [String-Klasse (Methoden) – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/api/system.string)

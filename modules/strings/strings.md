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

Text ist allgegenwärtig in Programmen – Benutzernamen, Fehlermeldungen, Sucheingaben, Ausgaben. Strings in C# verhalten sich dabei etwas anders als man vielleicht erwarten würde: sie sind unveränderlich, was einige praktische Konsequenzen hat, die man kennen sollte.

## Was ist ein String?

Ein `string` ist eine **unveränderliche** Zeichenkette. Jede Operation erzeugt einen neuen String – der ursprüngliche bleibt unverändert.

```csharp
string name = "Anna";
string gross = name.ToUpper(); // "ANNA"
Console.WriteLine(name);      // "Anna" – unverändert!
```

## Häufige String-Methoden

```csharp
string s = "  Hallo, Welt!  ";

s.Length                    // 16  – Anzahl Zeichen
s.ToUpper()                 // "  HALLO, WELT!  "
s.ToLower()                 // "  hallo, welt!  "
s.Trim()                    // "Hallo, Welt!"  – Leerzeichen entfernen
s.Contains("Welt")          // true
s.StartsWith("Hallo")       // false (wegen Leerzeichen vorne)
s.Trim().StartsWith("Hallo")// true
s.Replace("Welt", "C#")     // "  Hallo, C#!  "
s.Substring(2, 5)           // "Hallo"  (ab Index 2, Länge 5)
s.Split(',')                // ["  Hallo", " Welt!  "]
```

## String-Vergleich

```csharp
string a = "Hallo";
string b = "hallo";

Console.WriteLine(a == b);   // false – Groß-/Kleinschreibung zählt
Console.WriteLine(a.Equals(b, StringComparison.OrdinalIgnoreCase)); // true
```

## String-Verkettung

```csharp
string vorname = "Anna";
string nachname = "Müller";

// Variante 1: + Operator
string voll = vorname + " " + nachname;

// Variante 2: string.Join
string csv = string.Join(", ", "rot", "grün", "blau"); // "rot, grün, blau"

// Variante 3: Für viele Teile – StringBuilder (effizient)
var sb = new System.Text.StringBuilder();
for (int i = 0; i < 5; i++)
    sb.Append(i + " ");
Console.WriteLine(sb.ToString()); // "0 1 2 3 4 "
```

Übung: Lies einen Satz ein und gib die Anzahl der Wörter, die Anzahl der Zeichen (ohne Leerzeichen) und den Satz rückwärts aus.
{: .notice--info}

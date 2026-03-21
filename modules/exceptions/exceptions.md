---
title: "Ausnahmen in C#"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Manche Fehler kann man beim Schreiben des Codes nicht verhindern – was passiert, wenn der Benutzer Text eingibt statt einer Zahl, oder wenn eine Datei nicht gefunden wird? Exceptions sind C#'s Mechanismus, um solche unerwarteten Situationen zu signalisieren und gezielt darauf reagieren zu können.

## Was ist eine Exception?

Eine **Exception** (Ausnahme) ist ein Laufzeitfehler, der den normalen Programmablauf unterbricht. Wenn eine Exception nicht abgefangen wird, stürzt das Programm ab.

```
Programmablauf
    │
    ▼
int.Parse("abc")  ←── FormatException wird geworfen!
    │
    ▼
Programm bricht ab (wenn nicht abgefangen)
```

## Häufige Exceptions in C#

```csharp
int.Parse("abc");           // FormatException
int x = 10 / 0;             // DivideByZeroException
int[] arr = new int[3];
arr[5] = 1;                 // IndexOutOfRangeException
string s = null;
s.Length;                   // NullReferenceException
```

## Die Exception-Hierarchie

```
Exception
├── SystemException
│   ├── FormatException
│   ├── DivideByZeroException
│   ├── IndexOutOfRangeException
│   ├── NullReferenceException
│   ├── OverflowException
│   └── InvalidCastException
├── IOException
│   ├── FileNotFoundException
│   └── DirectoryNotFoundException
└── ArgumentException
    ├── ArgumentNullException
    └── ArgumentOutOfRangeException
```

Alle Exceptions erben von `Exception` und enthalten:
- `Message` – menschenlesbare Fehlerbeschreibung
- `StackTrace` – wo im Code der Fehler auftrat

## Exception-Informationen lesen

```csharp
try
{
    int.Parse("abc");
}
catch (FormatException ex)
{
    Console.WriteLine(ex.Message);
    // "The input string 'abc' was not in a correct format."
}
```

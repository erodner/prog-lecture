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

## Vordefinierte Exception-Typen

Die BCL enthält viele spezialisierte Exception-Typen. Die wichtigsten und ihre typischen Ursachen:

| Exception | Auslöser | Beispiel |
| :--- | :--- | :--- |
| `FormatException` | String lässt sich nicht parsen | `int.Parse("abc")` |
| `DivideByZeroException` | Division durch 0 (Integer) | `int x = 5 / 0` |
| `IndexOutOfRangeException` | Array-Index ungültig | `arr[arr.Length]` |
| `NullReferenceException` | Methode/Property auf `null` aufgerufen | `string s = null; s.Length` |
| `OverflowException` | Arithmetischer Überlauf in `checked`-Block | `checked { int.MaxValue + 1 }` |
| `InvalidCastException` | Ungültiger Cast zwischen Typen | `(int)(object)"text"` |
| `ArgumentException` | Methode erhält ungültiges Argument | eigene Validierung |
| `ArgumentNullException` | `null` als Argument unzulässig | `string.IsNullOrEmpty(null)` intern |
| `ArgumentOutOfRangeException` | Argument außerhalb erlaubtem Bereich | `list[-1]` |
| `FileNotFoundException` | Datei nicht gefunden | `File.ReadAllText("x.txt")` |
| `InvalidOperationException` | Operation im aktuellen Zustand nicht erlaubt | Enumerator nach Änderung |
| `NotImplementedException` | Methode noch nicht implementiert | Platzhalter in der Entwicklung |

In catch-Blöcken immer die **spezifischste** passende Exception fangen – nicht gleich `Exception` abfangen, wenn man nur auf `FormatException` reagieren will.
{: .notice--primary}

## Exception-Informationen lesen

Jede Exception bietet zwei besonders nützliche Properties:

```csharp
try
{
    int.Parse("abc");
}
catch (FormatException ex)
{
    Console.WriteLine(ex.Message);
    // "The input string 'abc' was not in a correct format."
    Console.WriteLine(ex.StackTrace);
    // zeigt den genauen Aufrufpfad zur Fehlerstelle
}
```

## Weitere Quellen

- [Ausnahmen und Fehlerbehandlung – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/fundamentals/exceptions/)
- [Exception-Klasse – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/api/system.exception)

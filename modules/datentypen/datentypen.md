---
title: "Primitive Datentypen in C#"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Bevor ein Programm irgendetwas sinnvoll tun kann, muss es Daten speichern – und dabei spielt es eine große Rolle, ob es sich um eine ganze Zahl, eine Kommazahl oder einen Text handelt. C# ist eine streng typisierte Sprache: jede Variable bekommt von Anfang an einen festen Typ, der bestimmt, wie der Wert im Speicher abgelegt und welche Operationen darauf erlaubt sind.

## Überblick

| Typ | Beschreibung | Größe | Wertebereich (ca.) |
| :--- | :--- | :--- | :--- |
| `int` | Ganze Zahl | 32-bit | −2,1 Mrd. bis +2,1 Mrd. |
| `long` | Ganze Zahl | 64-bit | ±9,2 · 10¹⁸ |
| `double` | Gleitkommazahl | 64-bit | ±1,7 · 10³⁰⁸, ~15–16 Stellen Genauigkeit |
| `float` | Gleitkommazahl | 32-bit | ±3,4 · 10³⁸, ~6–7 Stellen Genauigkeit |
| `decimal` | Dezimalzahl | 128-bit | hohe Präzision, für Geldbeträge |
| `bool` | Wahrheitswert | 1-bit (logisch) | `true`, `false` |
| `char` | Einzelnes Zeichen | 16-bit (UTF-16) | `'A'`, `'\n'` |
| `string` | Zeichenkette | variabel | `"Hallo"` |

## Deklaration und Initialisierung

```csharp
int alter = 21;
double temperatur = 36.6;
bool istStudent = true;
char ersterBuchstabe = 'H';
string name = "HTW Berlin";
```

## `var` und Konstanten

Mit `var` erkennt der Compiler den Typ automatisch aus dem zugewiesenen Wert – der Typ ist aber trotzdem fest und ändert sich nicht zur Laufzeit:

```csharp
var zahl = 42;       // int
var text = "Hallo";  // string
var preis = 9.99;    // double
```

Für Werte, die sich nie ändern sollen, gibt es `const`:

```csharp
const double Pi = 3.14159;
const int MaxVersuche = 3;

Pi = 3.0; // Compilerfehler!
```

## Wie Zahlen im Speicher liegen

Ein Computer kennt nur Bits – 0 und 1. Wie ein Wert dargestellt wird, hängt vom Typ ab.

**Ganze Zahlen** werden im Zweierkomplement gespeichert. Bei 32-bit `int` stehen 31 Bits für den Betrag und 1 Bit für das Vorzeichen:

```csharp
int a = 5;   //  00000000 00000000 00000000 00000101
int b = -1;  //  11111111 11111111 11111111 11111111
```

Das erklärt, warum `int` genau bis 2.147.483.647 geht – das ist 2³¹ − 1.

**Gleitkommazahlen** folgen dem IEEE-754-Standard. Eine `double` wird aufgeteilt in:
- 1 Bit Vorzeichen
- 11 Bits Exponent
- 52 Bits Mantisse

Der Wert ergibt sich als `(-1)^Vorzeichen × 1.Mantisse × 2^(Exponent-1023)`. Das klingt kompliziert – die praktische Konsequenz ist wichtiger:

## Fallstricke bei Gleitkommazahlen

Nicht jede Dezimalzahl lässt sich exakt als Binärbruch darstellen – so wie `1/3` im Dezimalsystem keine endliche Darstellung hat, gilt das für `0.1` im Binärsystem:

```csharp
double x = 0.1 + 0.2;
Console.WriteLine(x);          // 0.30000000000000004
Console.WriteLine(x == 0.3);   // False!
```

Gleitkommazahlen nie mit `==` vergleichen – stattdessen eine Toleranz verwenden:
{: .notice--warning}

```csharp
double a = 0.1 + 0.2;
double b = 0.3;
bool gleich = Math.Abs(a - b) < 1e-10;  // true
```

Für Geldbeträge oder andere präzise Dezimalwerte immer `decimal` verwenden:

```csharp
decimal preis = 0.1m + 0.2m;
Console.WriteLine(preis);        // 0.3
Console.WriteLine(preis == 0.3m); // True
```

`decimal` ist langsamer als `double`, dafür verlustfrei im Dezimalbereich – ideal für Finanzberechnungen.
{: .notice--primary}

## Integer-Überlauf

Auch ganze Zahlen haben Grenzen. Was passiert, wenn man darüber hinausgeht?

```csharp
int max = int.MaxValue;          // 2147483647
Console.WriteLine(max + 1);     // -2147483648 – Überlauf!
```

Der Wert „rollt um" – ein klassischer und gefährlicher Bug. Mit `checked` lässt sich das zur Laufzeit abfangen:

```csharp
checked
{
    int x = int.MaxValue + 1;  // wirft OverflowException
}
```

Übung: Warum liefert `0.1 + 0.2 == 0.3` in C# `false`? Schreibe einen korrekten Vergleich. Prüfe außerdem: Was ist `int.MaxValue + 1`?
{: .notice--info}

## Weitere Quellen

- [Eingebaute Typen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/builtin-types/built-in-types)
- [Ganzzahltypen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/builtin-types/integral-numeric-types)
- [Gleitkommatypen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/builtin-types/floating-point-numeric-types)
- [IEEE 754 interaktiv visualisiert – float.exposed](https://float.exposed)

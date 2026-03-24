---
title: "String-Formatierung"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Ob Preise mit zwei Nachkommastellen, Tabellen mit gleichmäßigen Spalten oder Datumsangaben in deutschem Format — wie Zahlen und Werte dargestellt werden, hat großen Einfluss darauf, wie professionell ein Programm wirkt. C# bietet dafür elegante Werkzeuge, allen voran die String-Interpolation, die wir schon bei der Konsolenausgabe kennengelernt haben.

## String-Interpolation (empfohlen)

Ein `$` vor dem String-Literal aktiviert die String-Interpolation: Innerhalb von `{...}` können beliebige C#-Ausdrücke stehen, deren Ergebnis als Text eingefügt wird:

```csharp
string name = "Anna";
int alter = 21;
double note = 1.7;

Console.WriteLine($"Name: {name}, Alter: {alter}, Note: {note}");
// Name: Anna, Alter: 21, Note: 1,7
```

Das ist nicht auf einfache Variablen beschränkt — auch Berechnungen und Methodenaufrufe funktionieren:

```csharp
int a = 5, b = 3;
Console.WriteLine($"{a} + {b} = {a + b}");   // 5 + 3 = 8
Console.WriteLine($"Heute ist {DateTime.Now:dd.MM.yyyy}");
```

## Formatangaben

Hinter dem Ausdruck in den geschweiften Klammern kann man mit `:` eine **Formatangabe** anfügen. Sie steuert, wie der Wert dargestellt wird — Nachkommastellen, Tausendertrennzeichen, Währungssymbol:

| Format | Bedeutung | Beispiel (`1234.5`) |
| :--- | :--- | :--- |
| `F2` | Festkomma, 2 Nachkommastellen | `1234,50` |
| `N2` | Zahl mit Tausendertrennzeichen | `1.234,50` |
| `C` | Währungsformat (lokalisiert) | `1.234,50 €` |
| `P0` | Prozent (ohne Nachkommastellen) | für `0.75`: `75 %` |

```csharp
double preis = 1234.5;
double pi = Math.PI;

Console.WriteLine($"{preis:F2}");    // 1234,50
Console.WriteLine($"{preis:N2}");    // 1.234,50
Console.WriteLine($"{preis:C}");     // 1.234,50 €
Console.WriteLine($"{pi:F4}");       // 3,1416
Console.WriteLine($"{0.75:P0}");     // 75 %
```

Die Zahl nach dem Buchstaben gibt die Anzahl der Nachkommastellen an. `F2` bedeutet "Festkomma mit 2 Stellen", `F0` rundet auf ganze Zahlen. Die Darstellung hängt von der Systemsprache ab — auf einem deutschen System wird das Komma als Dezimaltrennzeichen und der Punkt als Tausendertrennzeichen verwendet.

## Ausrichtung und Breite

Für tabellarische Ausgaben kann man vor der Formatangabe eine **Breite** angeben. Eine positive Zahl richtet rechtsbündig aus, eine negative linksbündig:

```csharp
Console.WriteLine($"{"Name",-15} {"Note",5}");
Console.WriteLine($"{"Anna",-15} {1.7,5:F1}");
Console.WriteLine($"{"Ben",-15} {2.3,5:F1}");
```

Ausgabe:
```
Name             Note
Anna              1,7
Ben               2,3
```

Die Syntax ist `{Ausdruck,Breite:Format}` — die Breite gibt die Mindestanzahl an Zeichen an. Ist der Wert kürzer, wird mit Leerzeichen aufgefüllt. Das ist nützlich, um Spalten gleichmäßig breit zu halten.

## `string.Format` (ältere Variante)

Vor der Einführung von String-Interpolation (`$"..."`) in C# 6 wurde `string.Format` verwendet, wo Platzhalter per Nummer referenziert werden:

```csharp
string s = string.Format("Name: {0}, Alter: {1}", name, alter);
```

Man begegnet dieser Schreibweise noch in älterem Code. Heute wird fast ausschließlich String-Interpolation verwendet — sie ist lesbarer, weil man direkt sieht, welcher Wert wo eingefügt wird.
{: .notice--primary}

Übung: Formatiere eine Rechnung mit drei Produkten: Name (linksbündig, 20 Zeichen), Preis (rechtsbündig, Währungsformat).
{: .notice--info}

## Weitere Quellen

- [Interpolierte Zeichenfolgen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/tokens/interpolated)
- [Standardformate für numerische Werte – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/standard/base-types/standard-numeric-format-strings)

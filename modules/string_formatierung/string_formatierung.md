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

Ob Preise mit zwei Nachkommastellen, Tabellen mit gleichmäßigen Spalten oder Datumsangaben in deutschem Format – wie Zahlen und Werte dargestellt werden, hat großen Einfluss darauf, wie professionell ein Programm wirkt. C# bietet dafür elegante Werkzeuge, allen voran die String-Interpolation.

## String-Interpolation (empfohlen)

```csharp
string name = "Anna";
int alter = 21;
double gpa = 1.7;

Console.WriteLine($"Name: {name}, Alter: {alter}, GPA: {gpa}");
// Name: Anna, Alter: 21, GPA: 1,7
```

Innerhalb der `{}` können beliebige C#-Ausdrücke stehen:

```csharp
int a = 5, b = 3;
Console.WriteLine($"{a} + {b} = {a + b}");   // 5 + 3 = 8
Console.WriteLine($"Heute ist {DateTime.Now:dd.MM.yyyy}");
```

## Formatangaben

```csharp
double preis = 1234.5;
double pi = Math.PI;

Console.WriteLine($"{preis:F2}");    // 1234,50  – 2 Nachkommastellen
Console.WriteLine($"{preis:N2}");    // 1.234,50 – mit Tausendertrennzeichen
Console.WriteLine($"{preis:C}");     // 1.234,50 € – Währungsformat
Console.WriteLine($"{pi:F4}");       // 3,1416   – 4 Nachkommastellen
Console.WriteLine($"{0.75:P0}");     // 75 %      – Prozent
```

## Ausrichtung und Breite

```csharp
// Tabelle formatieren:
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

## `string.Format` (ältere Variante)

```csharp
string s = string.Format("Name: {0}, Alter: {1}", name, alter);
```

Heute wird fast ausschließlich String-Interpolation verwendet – sie ist lesbarer.
{: .notice--primary}

Übung: Formatiere eine Rechnung mit drei Produkten: Name (linksbündig, 20 Zeichen), Preis (rechtsbündig, Währungsformat).
{: .notice--info}

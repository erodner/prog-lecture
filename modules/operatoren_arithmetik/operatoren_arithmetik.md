---
title: "Arithmetische Operatoren"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Addieren, subtrahieren, den Rest einer Division bestimmen – arithmetische Operatoren erledigen in C# all das, was man vom Taschenrechner kennt. Allerdings gibt es ein paar Besonderheiten, die beim Rechnen mit ganzen Zahlen schnell zu unerwarteten Ergebnissen führen können.

## Grundrechenarten

```csharp
int a = 17, b = 5;

Console.WriteLine(a + b);  // 22  – Addition
Console.WriteLine(a - b);  // 12  – Subtraktion
Console.WriteLine(a * b);  // 85  – Multiplikation
Console.WriteLine(a / b);  // 3   – Ganzzahldivision (Nachkommastellen abgeschnitten!)
Console.WriteLine(a % b);  // 2   – Modulo (Rest der Division)
```

## Ganzzahldivision vs. Gleitkommadivision

```csharp
int x = 7, y = 2;
Console.WriteLine(x / y);           // 3   (int / int = int)
Console.WriteLine((double)x / y);   // 3.5 (cast auf double zuerst)
Console.WriteLine(7.0 / 2);         // 3.5 (ein Operand ist double)
```

## Modulo – praktische Anwendungen

Der Modulo-Operator `%` liefert den Rest einer Division. Der einfachste Anwendungsfall: die letzte Ziffer einer Zahl extrahieren, oder prüfen ob eine Zahl gerade ist:

```csharp
int n = 1234;
Console.WriteLine(n % 10);  // 4 – letzte Ziffer

Console.WriteLine(13 % 2 == 0 ? "gerade" : "ungerade"); // ungerade
```

Ein etwas umfangreicheres Beispiel — Sekunden in Stunden, Minuten und Restsekunden umrechnen:

```csharp
int sekunden = 3725;
int stunden  = sekunden / 3600;
int minuten  = (sekunden % 3600) / 60;
int rest     = sekunden % 60;
Console.WriteLine($"{stunden}h {minuten}min {rest}s"); // 1h 2min 5s
```

## Inkrement und Dekrement

```csharp
int i = 5;
i++;  // i = 6  (Postfix-Inkrement)
i--;  // i = 5  (Postfix-Dekrement)
++i;  // i = 6  (Präfix-Inkrement)
```

Übung: Schreibe ein Programm, das die Anzahl der Sekunden in einem Tag einliest und in Stunden, Minuten und Sekunden umrechnet.
{: .notice--info}

## Weitere Quellen

- [Arithmetische Operatoren – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/operators/arithmetic-operators)

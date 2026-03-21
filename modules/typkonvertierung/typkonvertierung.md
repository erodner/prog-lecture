---
title: "Typkonvertierung"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Im Alltag rechnen wir problemlos mit ganzen Zahlen und Dezimalzahlen zusammen – der Computer muss dabei aber immer genau wissen, in welchem Format ein Wert vorliegt. Typkonvertierung beschreibt, wie C# Werte von einem Typ in einen anderen umwandelt, und wann das automatisch passiert oder explizit angefordert werden muss.

## Implizite Konvertierung

Wenn kein Datenverlust möglich ist, konvertiert C# automatisch:

```csharp
int ganzzahl = 42;
double kommazahl = ganzzahl;  // int → double, kein Datenverlust
Console.WriteLine(kommazahl); // 42
```

## Explizite Konvertierung (Cast)

Wenn Datenverlust möglich ist, muss man explizit casten:

```csharp
double pi = 3.14159;
int ganzzahl = (int)pi;  // Nachkommastellen werden abgeschnitten!
Console.WriteLine(ganzzahl); // 3
```

## Konvertierung mit `Convert`

```csharp
string text = "42";
int zahl = Convert.ToInt32(text);
double komma = Convert.ToDouble("3.14");
string zurück = Convert.ToString(42);
```

## Konvertierung mit `Parse`

```csharp
int i = int.Parse("100");
double d = double.Parse("3.14");
bool b = bool.Parse("true");
```

`Parse` wirft eine Exception bei ungültigem Input – für Benutzereingaben daher lieber `TryParse` verwenden.

## `TryParse` – sichere Konvertierung

```csharp
string eingabe = Console.ReadLine();

if (int.TryParse(eingabe, out int ergebnis))
    Console.WriteLine("Zahl: " + ergebnis);
else
    Console.WriteLine("Ungültige Eingabe.");
```

| Methode | Verhalten bei Fehler |
| :--- | :--- |
| `(int)x` | Datenverlust (kein Fehler) |
| `int.Parse(s)` | Exception |
| `int.TryParse(s, out x)` | Gibt `false` zurück |
| `Convert.ToInt32(s)` | Exception |

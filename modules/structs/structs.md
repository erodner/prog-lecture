---
title: "Structs – Werttypen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Nicht jede Datenstruktur braucht die volle Schwergewichtigkeit einer Klasse – für kleine, zusammengehörige Werte wie einen Koordinatenpunkt oder eine RGB-Farbe sind Structs oft die bessere Wahl. Der entscheidende Unterschied liegt im Kopierverhalten: Structs werden als Wert übergeben, Klassen als Referenz.

## Struct vs. Class

Der wesentliche Unterschied: `struct` ist ein **Werttyp**, `class` ist ein **Referenztyp**.

| Eigenschaft | `struct` | `class` |
| :--- | :--- | :--- |
| Speicherort | Stack (meist) | Heap |
| Zuweisung | Kopie des Werts | Kopie der Referenz |
| `null` möglich | Nein (ohne `?`) | Ja |
| Vererbung | Nein | Ja |

## Struct deklarieren

```csharp
struct Punkt
{
    public double X { get; set; }
    public double Y { get; set; }

    public Punkt(double x, double y) { X = x; Y = y; }

    public double Abstand(Punkt anderer)
    {
        double dx = X - anderer.X;
        double dy = Y - anderer.Y;
        return Math.Sqrt(dx * dx + dy * dy);
    }

    public override string ToString() => $"({X}, {Y})";
}

Punkt p1 = new Punkt(0, 0);
Punkt p2 = new Punkt(3, 4);
Console.WriteLine(p1.Abstand(p2)); // 5
```

## Kopieverhalten

```csharp
Punkt a = new Punkt(1, 2);
Punkt b = a;    // Kopie!
b.X = 99;

Console.WriteLine(a.X); // 1 – unverändert
Console.WriteLine(b.X); // 99
```

Mit einer Klasse würde `a.X` ebenfalls 99 sein, da beide auf dasselbe Objekt zeigen.

## Wann `struct`?

Structs eignen sich für **kleine, unveränderliche Datencontainer** ohne komplexes Verhalten: Koordinaten, Farben, Zeitspannen, Vektoren. Für komplexere Modelle mit Vererbung und Identität immer `class` verwenden.

## Weitere Quellen

- [Strukturtypen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/builtin-types/struct)
- [Wählen zwischen Klasse und Struktur – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/standard/design-guidelines/choosing-between-class-and-struct)

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

Nicht jede Datenstruktur braucht die volle Schwergewichtigkeit einer Klasse. Für kleine, zusammengehörige Werte wie einen Koordinatenpunkt oder eine RGB-Farbe sind Structs oft die bessere Wahl. Syntaktisch sehen sie Klassen sehr ähnlich — sie können Properties, Konstruktoren und Methoden haben. Der entscheidende Unterschied liegt im Kopierverhalten.

## Ein Struct am Beispiel

Ein `struct` wird fast genauso deklariert wie eine `class` — nur das Schlüsselwort ändert sich:

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

Auf den ersten Blick kein Unterschied zu einer Klasse. Warum also `struct`? Die Antwort liegt im Speicherverhalten.

## Der Unterschied: Kopieverhalten

Den Unterschied zwischen Werttypen und Referenztypen kennen wir bereits aus dem Modul zu Werttypen und Referenztypen. Klassen sind Referenztypen — bei einer Zuweisung wird nur die Referenz kopiert, beide Variablen zeigen auf dasselbe Objekt. Structs sind **Werttypen** — bei einer Zuweisung wird eine **vollständige Kopie** erzeugt:

```csharp
Punkt a = new Punkt(1, 2);
Punkt b = a;    // vollständige Kopie!
b.X = 99;

Console.WriteLine(a.X); // 1 — unverändert
Console.WriteLine(b.X); // 99
```

Wäre `Punkt` eine `class`, würde `a.X` ebenfalls `99` sein, da beide Variablen auf dasselbe Objekt zeigen würden. Bei einem `struct` hat jede Variable ihre eigene, unabhängige Kopie der Daten — genau wie bei `int` oder `double`.

Dieses Kopieverhalten ist der zentrale Unterschied — die folgende Tabelle fasst alle weiteren Unterschiede kompakt zusammen.

## Struct vs. Class auf einen Blick

| Eigenschaft | `struct` | `class` |
| :--- | :--- | :--- |
| Typ | Werttyp | Referenztyp |
| Zuweisung | Kopie des Werts | Kopie der Referenz |
| `null` möglich | Nein (ohne `?`) | Ja |
| Vererbung | Nein | Ja |
| Typisch für | Kleine Datenbündel | Komplexe Modelle |

## Wann `struct`, wann `class`?

Structs eignen sich für **kleine, zusammengehörige Werte** ohne komplexes Verhalten — Koordinaten, Farben, Zeitspannen, Vektoren. In der .NET-Bibliothek sind `int`, `double`, `bool`, `DateTime` und `TimeSpan` allesamt Structs.

Für alles andere — Objekte mit Identität, komplexes Verhalten, Vererbung — verwendet man `class`. Im Zweifel ist `class` die sichere Wahl; `struct` ist eine bewusste Optimierung für spezielle Fälle.
{: .notice--primary}

Übung: Erstelle ein Struct `Farbe` mit Properties `R`, `G`, `B` (jeweils `byte`). Erstelle zwei Farben, weise eine der anderen zu und ändere einen Wert — prüfe, ob das Original unverändert bleibt.
{: .notice--info}

## Weitere Quellen

- [Strukturtypen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/builtin-types/struct)
- [Wählen zwischen Klasse und Struktur – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/standard/design-guidelines/choosing-between-class-and-struct)

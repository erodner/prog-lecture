---
title: "Mehrdimensionale Arrays"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Manchmal sind Daten von Natur aus tabellarisch – Noten pro Studierendem und Prüfung, Temperaturen nach Tag und Stunde, Spielfelder in einem Brettspiel. Für solche Strukturen bietet C# mehrdimensionale Arrays, die sich wie eine Tabelle mit Zeilen und Spalten verhalten.

## Zweidimensionale Arrays

Ein 2D-Array entspricht einer Tabelle mit Zeilen und Spalten. Der Zugriff erfolgt über zwei Indizes — den ersten für die Zeile, den zweiten für die Spalte. Beide starten wie gewohnt bei 0:

```
          Spalte 0  Spalte 1  Spalte 2
Zeile 0:    1         2         3
Zeile 1:    4         5         6
Zeile 2:    7         8         9
```

Die Deklaration verwendet `[,]` statt `[]`. Statt `Length` verwendet man `GetLength(0)` für die Anzahl der Zeilen und `GetLength(1)` für die Anzahl der Spalten:

```csharp
int[,] tabelle = {
    { 1, 2, 3 },
    { 4, 5, 6 },
    { 7, 8, 9 }
};

Console.WriteLine(tabelle[0, 0]); // 1  — erste Zeile, erste Spalte
Console.WriteLine(tabelle[1, 2]); // 6  — zweite Zeile, dritte Spalte
Console.WriteLine(tabelle.GetLength(0)); // 3 — Anzahl Zeilen
Console.WriteLine(tabelle.GetLength(1)); // 3 — Anzahl Spalten
```

## Durchlaufen mit verschachtelten Schleifen

Um alle Elemente einer Tabelle zu bearbeiten, braucht man eine **verschachtelte Schleife**: Die äußere Schleife durchläuft die Zeilen, die innere die Spalten. Für jede Zeile wird die innere Schleife komplett durchlaufen — insgesamt ergeben sich `Zeilen × Spalten` Durchläufe:

```csharp
for (int zeile = 0; zeile < tabelle.GetLength(0); zeile++)
{
    for (int spalte = 0; spalte < tabelle.GetLength(1); spalte++)
        Console.Write($"{tabelle[zeile, spalte],4}");
    Console.WriteLine();
}
```

Das `Console.WriteLine()` nach der inneren Schleife erzeugt den Zeilenumbruch — ohne es würden alle Werte in einer einzigen Zeile stehen.

## Praxisbeispiel: Notentabelle

Ein realistisches Beispiel: Für mehrere Studierende sollen die Durchschnittsnoten über mehrere Prüfungen berechnet werden. Die erste Dimension (Zeilen) steht für die Studierenden, die zweite (Spalten) für die Prüfungen:

```csharp
// 3 Studierende, 4 Prüfungen
double[,] noten = {
    { 1.7, 2.0, 1.3, 2.3 },
    { 2.7, 3.0, 2.0, 1.7 },
    { 1.0, 1.3, 1.7, 2.0 }
};

string[] namen = { "Anna", "Ben", "Clara" };

for (int s = 0; s < noten.GetLength(0); s++)
{
    double summe = 0;
    for (int p = 0; p < noten.GetLength(1); p++)
        summe += noten[s, p];
    Console.WriteLine($"{namen[s]}: Ø {summe / noten.GetLength(1):F2}");
}
```

Die äußere Schleife iteriert über die Studierenden (`s`), die innere summiert deren Prüfungsnoten (`p`). Am Ende jeder Zeile wird der Durchschnitt ausgegeben. Dieses Muster — zeilenweise aggregieren — ist eines der häufigsten bei 2D-Arrays.

## Gezackte Arrays (Jagged Arrays)

Reguläre 2D-Arrays sind **rechteckig**: Jede Zeile hat gleich viele Spalten. Manchmal braucht man aber Zeilen unterschiedlicher Länge — etwa für ein Dreieck oder wenn Datenreihen unterschiedlich lang sind. Dafür gibt es **gezackte Arrays** (engl. *jagged arrays*): ein Array von Arrays, bei dem jede Zeile ein eigenständiges Array ist.

Die Syntax unterscheidet sich deutlich: `int[][]` statt `int[,]`. Jede Zeile muss einzeln mit `new` angelegt werden:

```csharp
int[][] dreieck = new int[4][];
for (int i = 0; i < dreieck.Length; i++)
{
    dreieck[i] = new int[i + 1];
    for (int j = 0; j <= i; j++)
        dreieck[i][j] = j + 1;
}
// [[1], [1,2], [1,2,3], [1,2,3,4]]
```

Der Zugriff erfolgt mit zwei getrennten Klammerpaaren: `dreieck[2][1]` liefert das zweite Element der dritten Zeile. Bei rechteckigen Arrays wäre es `tabelle[2, 1]` mit Komma.

Für tabellarische Daten mit fester Spaltenanzahl (z.B. Spielfelder, Matrizen) sind rechteckige 2D-Arrays die bessere Wahl. Gezackte Arrays eignen sich, wenn die Zeilen unterschiedlich lang sein können.
{: .notice--primary}

Übung: Erstelle ein 5×5-Array und fülle es mit dem kleinen Einmaleins (Zeile `i`, Spalte `j` enthält `(i+1) * (j+1)`). Gib es als formatierte Tabelle aus.
{: .notice--info}

## Weitere Quellen

- [Mehrdimensionale Arrays – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/arrays/multidimensional-arrays)
- [Gezackte Arrays (Jagged Arrays) – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/arrays/jagged-arrays)

---
title: "LINQ-Grundlagen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Alle Studierenden mit einer Note besser als 2.0, sortiert nach Name – früher brauchte man dafür eine Schleife, eine Bedingung und einen Sortieralgorithmus. Mit LINQ geht das in einer einzigen lesbaren Zeile. LINQ bringt die Ausdruckskraft von Datenbankabfragen direkt in C#.

## Was ist LINQ?

**LINQ** (Language Integrated Query) erlaubt es, Datensammlungen direkt in C# zu filtern, sortieren und transformieren – ohne manuelle Schleifen.

```csharp
using System.Linq;

int[] zahlen = { 3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5 };

// Alle Zahlen > 4, sortiert:
var ergebnis = zahlen.Where(x => x > 4).OrderBy(x => x);

foreach (int z in ergebnis)
    Console.Write(z + " ");
// 5 5 5 6 9
```

Die Syntax `x => x > 4` ist ein **Lambda-Ausdruck** — eine kompakte Schreibweise für eine anonyme Funktion. `x` ist der Parameter (jedes Element der Sammlung), `x > 4` ist die Bedingung. Man liest es als: „für jedes x prüfe, ob x größer als 4 ist". Lambda-Ausdrücke sind das zentrale Werkzeug von LINQ.

## Grundlegende LINQ-Methoden

Die wichtigsten LINQ-Methoden lassen sich in vier Kategorien einteilen: **Filtern** (welche Elemente?), **Transformieren** (in welche Form?), **Sortieren** (in welcher Reihenfolge?) und **Aggregieren** (zu einem einzelnen Wert zusammenfassen):

```csharp
List<string> namen = new List<string> { "Anna", "Ben", "Clara", "David", "Eva" };

// Filtern
var langNamen = namen.Where(n => n.Length > 3);
// ["Anna", "Clara", "David"]

// Transformieren
var grossbuchstaben = namen.Select(n => n.ToUpper());
// ["ANNA", "BEN", "CLARA", "DAVID", "EVA"]

// Sortieren
var sortiert = namen.OrderBy(n => n);
var absteigend = namen.OrderByDescending(n => n.Length);

// Aggregation
int gesamt = namen.Count();
int lang = namen.Count(n => n.Length > 3);
string längster = namen.MaxBy(n => n.Length);

// Prüfen
bool alleKurz = namen.All(n => n.Length < 10);   // true
bool einKurz  = namen.Any(n => n.Length <= 3);    // true (Ben, Eva)
```

## Praxisbeispiel: Notenliste auswerten

Nachdem wir die einzelnen LINQ-Methoden kennengelernt haben, kombinieren wir sie in einem realistischeren Beispiel. Dabei sieht man gut, wie sich mehrere Methoden zu einer lesbaren Abfragekette verketten lassen:

```csharp
var noten = new List<(string Name, double Note)>
{
    ("Anna", 1.7), ("Ben", 2.3), ("Clara", 1.0),
    ("David", 3.0), ("Eva", 1.3)
};

// Bestandene Studierende, nach Note sortiert:
var bestanden = noten
    .Where(s => s.Note <= 4.0)
    .OrderBy(s => s.Note)
    .Select(s => $"{s.Name}: {s.Note}");

foreach (var eintrag in bestanden)
    Console.WriteLine(eintrag);

double durchschnitt = noten.Average(s => s.Note);
Console.WriteLine($"Durchschnitt: {durchschnitt:F2}");
```

LINQ-Methoden lassen sich beliebig verketten – jede Methode gibt wieder eine Sammlung zurück, auf der weitere Methoden aufgerufen werden können.
{: .notice--primary}

**Lazy Evaluation:** LINQ-Methoden wie `Where` oder `Select` führen die Abfrage nicht sofort aus — sie erzeugen nur eine Beschreibung der Abfrage. Erst wenn man die Ergebnisse tatsächlich durchläuft (z.B. mit `foreach`) oder materialisiert, wird die Berechnung ausgeführt. Um das Ergebnis sofort in eine konkrete Sammlung umzuwandeln, verwendet man `ToList()` oder `ToArray()`:
{: .notice--warning}

```csharp
// Lazy – wird erst bei Iteration ausgewertet:
var abfrage = namen.Where(n => n.Length > 3);

// Sofort materialisiert – Ergebnis steht fest:
List<string> ergebnis = namen.Where(n => n.Length > 3).ToList();
```

## Weitere Quellen

- [LINQ-Übersicht – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/linq/)
- [Standard Query Operators – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/linq/standard-query-operators/)
- [101 LINQ-Beispiele – Microsoft Learn](https://learn.microsoft.com/de-de/samples/dotnet/try-samples/101-linq-samples/)

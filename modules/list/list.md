---
title: "List<T> – Dynamische Listen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Im Alltag weiß man selten im Voraus, wie viele Einträge eine Liste haben wird – ob Einkaufszettel, Kontakte oder Suchergebnisse. `List<T>` ist die flexibelste und meistgenutzte Collection in C#: sie wächst und schrumpft dynamisch und bietet alle grundlegenden Operationen zum Verwalten von Sammlungen.

## Wozu `List<T>`?

Arrays haben eine feste Größe — einmal angelegt, kann die Länge nicht mehr geändert werden. Wenn man nicht weiß, wie viele Elemente es werden, müsste man ein Array überdimensionieren oder umständlich ein neues, größeres Array anlegen und die Werte kopieren. `List<T>` übernimmt das automatisch: Sie wächst und schrumpft dynamisch.

Das `<T>` steht für den Elementtyp — man spricht von einem **generischen Typ**. `List<string>` ist eine Liste von Strings, `List<int>` eine von ganzen Zahlen. Der Compiler prüft, dass nur passende Werte eingefügt werden.

```csharp
using System.Collections.Generic;

List<string> namen = new List<string>();
namen.Add("Anna");
namen.Add("Ben");
namen.Add("Clara");
Console.WriteLine(namen.Count); // 3
```

## Wichtige Methoden

`List<T>` bietet eine Vielzahl an Methoden zum Hinzufügen, Entfernen, Suchen und Sortieren. Alle Methoden verändern die Liste direkt (in-place):

```csharp
List<int> zahlen = new List<int> { 5, 2, 8, 1, 9 };

zahlen.Add(3);              // ans Ende anfügen
zahlen.Insert(0, 99);       // an Position 0 einfügen
zahlen.Remove(8);           // ersten Wert 8 entfernen
zahlen.RemoveAt(0);         // Element an Index 0 entfernen
zahlen.Sort();              // sortieren
zahlen.Reverse();           // umkehren

bool hat = zahlen.Contains(5);   // true
int idx = zahlen.IndexOf(9);     // Index von 9
zahlen.Clear();                  // alle entfernen
```

Beachte: Bei `List<T>` heißt die Eigenschaft für die Anzahl `Count`, nicht `Length` wie bei Arrays. Der Indexzugriff funktioniert identisch: `städte[0]` liefert das erste Element.
{: .notice--primary}

## Iteration

Nachdem wir nun Elemente hinzufügen, entfernen und suchen können, schauen wir uns an, wie man eine Liste systematisch durchläuft — das geht genauso wie bei Arrays mit `foreach` oder `for`:

```csharp
List<string> städte = new List<string> { "Berlin", "Hamburg", "München" };

foreach (string stadt in städte)
    Console.WriteLine(stadt);

// Mit Index:
for (int i = 0; i < städte.Count; i++)
    Console.WriteLine($"{i}: {städte[i]}");
```

## Praxisbeispiel: Einkaufsliste

Im folgenden Beispiel setzen wir die bisherigen Bausteine zusammen: Der Benutzer gibt beliebig viele Artikel ein, die dynamisch in einer Liste gesammelt und am Ende ausgegeben werden — genau die Art von Aufgabe, für die `List<T>` gemacht ist.

```csharp
List<string> einkauf = new List<string>();
string eingabe;

do {
    Console.Write("Artikel (leer = fertig): ");
    eingabe = Console.ReadLine();
    if (!string.IsNullOrWhiteSpace(eingabe))
        einkauf.Add(eingabe);
} while (!string.IsNullOrWhiteSpace(eingabe));

Console.WriteLine($"\nEinkaufsliste ({einkauf.Count} Artikel):");
foreach (string artikel in einkauf)
    Console.WriteLine($"  - {artikel}");
```

## `List<T>` vs. Array

Wann sollte man also eine `List<T>` verwenden und wann ein Array? Die folgende Tabelle fasst die wesentlichen Unterschiede zusammen:

| | Array | `List<T>` |
| :--- | :--- | :--- |
| Größe | Fest | Dynamisch |
| Zugriff per Index | Ja | Ja |
| Einfügen/Entfernen | Aufwändig | Einfach |
| Speicher | Etwas effizienter | Etwas mehr Overhead |

## Weitere Quellen

- [List\<T\> – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/api/system.collections.generic.list-1)
- [Generische Collections – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/standard/generics/collections)

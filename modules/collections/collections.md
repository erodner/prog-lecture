---
title: "Weitere Collections"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Neben `List` und `Dictionary` gibt es in C# noch weitere spezialisierte Sammlungstypen, die für bestimmte Situationen besonders gut geeignet sind. Wer für jede Aufgabe das richtige Werkzeug wählt, schreibt klareren und oft auch effizienteren Code.

## Queue\<T\> – Warteschlange (FIFO)

Eine Queue funktioniert wie eine Warteschlange im Supermarkt: **First In, First Out** — wer zuerst ankommt, wird zuerst bedient. Elemente werden hinten angefügt (`Enqueue`) und vorne entnommen (`Dequeue`):

```csharp
Queue<string> warteschlange = new Queue<string>();

warteschlange.Enqueue("Anna");   // hinten anstellen
warteschlange.Enqueue("Ben");
warteschlange.Enqueue("Clara");

Console.WriteLine(warteschlange.Dequeue()); // "Anna" – vorne entnehmen
Console.WriteLine(warteschlange.Peek());    // "Ben"  – nur ansehen, nicht entnehmen
Console.WriteLine(warteschlange.Count);     // 2
```

**Anwendung:** Druckerwarteschlange, Aufgabenpuffer, Ereignisverarbeitung.

## Stack\<T\> – Stapel (LIFO)

Während eine Queue Elemente in der Reihenfolge ihres Eintreffens verarbeitet, funktioniert ein Stack genau umgekehrt. Ein Stack funktioniert wie ein Stapel Teller: **Last In, First Out** — der zuletzt abgelegte Teller wird zuerst wieder genommen. Das kennen wir bereits vom Call Stack bei der Rekursion — dort werden Methodenaufrufe auf genau diese Weise gestapelt:

```csharp
Stack<int> stapel = new Stack<int>();

stapel.Push(1);
stapel.Push(2);
stapel.Push(3);

Console.WriteLine(stapel.Pop());   // 3  – oben entnehmen
Console.WriteLine(stapel.Peek());  // 2  – nur ansehen
Console.WriteLine(stapel.Count);   // 2
```

**Anwendung:** Rückgängig-Funktion (Undo), Navigations-Historie, Call Stack.

## HashSet\<T\> – Menge ohne Duplikate

Queue und Stack steuern die Reihenfolge des Zugriffs — aber manchmal geht es gar nicht um Reihenfolge, sondern nur darum, *ob* ein Element vorhanden ist. Genau dafür gibt es `HashSet`. Ein `HashSet` speichert jedes Element höchstens einmal — Duplikate werden automatisch ignoriert. Das ist nützlich, wenn man nur wissen will, *welche* Werte vorkommen, nicht *wie oft*. Außerdem unterstützt es mathematische Mengenoperationen wie Schnitt- und Vereinigungsmenge:

```csharp
HashSet<string> besucher = new HashSet<string>();

besucher.Add("Anna");
besucher.Add("Ben");
besucher.Add("Anna"); // wird ignoriert – schon vorhanden

Console.WriteLine(besucher.Count); // 2

// Mengenoperationen:
HashSet<int> a = new HashSet<int> { 1, 2, 3, 4 };
HashSet<int> b = new HashSet<int> { 3, 4, 5, 6 };

a.IntersectWith(b);  // Schnittmenge: {3, 4}
a.UnionWith(b);      // Vereinigung
a.ExceptWith(b);     // Differenz
```

## Übersicht

Die folgende Tabelle gibt einen kompakten Überblick über die wichtigsten Collection-Typen und hilft bei der Entscheidung, welche Datenstruktur für eine Aufgabe am besten geeignet ist:

| Collection | Reihenfolge | Duplikate | Zugriff |
| :--- | :--- | :--- | :--- |
| `List<T>` | Ja (eingefügt) | Ja | Index |
| `Queue<T>` | FIFO | Ja | Nur vorne/hinten |
| `Stack<T>` | LIFO | Ja | Nur oben |
| `HashSet<T>` | Nein | Nein | Enthält-Prüfung |
| `Dictionary<K,V>` | Nein | Nein (Keys) | Schlüssel |

**Welche Collection wählen?** Als Faustregel: Verwende `List<T>`, wenn du eine geordnete, veränderbare Sammlung brauchst. Nutze `Dictionary<K,V>`, wenn du Werte über einen Schlüssel nachschlagen willst. `HashSet<T>` eignet sich, wenn nur die Existenz zählt und keine Duplikate erlaubt sind. `Queue<T>` und `Stack<T>` sind die richtige Wahl, wenn die Verarbeitungsreihenfolge (FIFO bzw. LIFO) eine Rolle spielt.
{: .notice--primary}

## Weitere Quellen

- [Collections-Übersicht – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/concepts/collections)
- [Stack\<T\> – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/api/system.collections.generic.stack-1)
- [Queue\<T\> – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/api/system.collections.generic.queue-1)
- [Stack und Queue visualisiert – VisuAlgo](https://visualgo.net/en/list)

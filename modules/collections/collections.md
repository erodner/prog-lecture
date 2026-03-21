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

**First In, First Out** – wer zuerst ankommt, verlässt die Schlange zuerst.

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

**Last In, First Out** – das zuletzt Abgelegte wird zuerst entnommen.

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

| Collection | Reihenfolge | Duplikate | Zugriff |
| :--- | :--- | :--- | :--- |
| `List<T>` | Ja (eingefügt) | Ja | Index |
| `Queue<T>` | FIFO | Ja | Nur vorne/hinten |
| `Stack<T>` | LIFO | Ja | Nur oben |
| `HashSet<T>` | Nein | Nein | Enthält-Prüfung |
| `Dictionary<K,V>` | Nein | Nein (Keys) | Schlüssel |

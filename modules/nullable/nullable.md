---
title: "Nullable Types"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Manchmal gibt es keinen sinnvollen Wert – eine Temperatur wurde nicht gemessen, ein Alter ist unbekannt, eine Datenbankabfrage lieferte kein Ergebnis. Normale Werttypen wie `int` oder `double` können das nicht ausdrücken: sie haben immer einen Wert. Nullable Types lösen dieses Problem, indem sie `null` als zusätzlichen Zustand erlauben.

## Nullable Werttypen

Ein `?` hinter dem Typnamen macht einen Werttyp nullable:

```csharp
int  normale  = 42;    // muss immer einen Wert haben
int? nullable = 42;    // kann einen int-Wert haben – oder null
int? leer     = null;  // gültig!
```

Der Zugriff auf den Wert erfolgt über `.Value`, aber nur wenn tatsächlich ein Wert vorhanden ist. `.HasValue` prüft das:

```csharp
int? alter = null;

if (alter.HasValue)
    Console.WriteLine($"Alter: {alter.Value}");
else
    Console.WriteLine("Alter unbekannt.");
```

Direkter Zugriff auf `.Value` ohne Prüfung wirft eine `InvalidOperationException` wenn der Wert `null` ist.
{: .notice--warning}

## Null-Coalescing-Operator `??`

Der `??`-Operator gibt den linken Wert zurück, falls er nicht `null` ist – andernfalls den rechten Standardwert:

```csharp
int? gemesseneTemp = null;
int anzeigeTemp = gemesseneTemp ?? -999;  // -999 als Kennwert für "kein Wert"

Console.WriteLine(anzeigeTemp); // -999
```

Das vermeidet überall `if (x.HasValue)` und macht Code kürzer:

```csharp
string name = Console.ReadLine();
string anzeigeName = name ?? "Unbekannt";
```

## Null-Conditional-Operator `?.`

Der `?.`-Operator ruft eine Methode oder Property nur auf, wenn das Objekt nicht `null` ist – andernfalls gibt er `null` zurück, ohne eine Exception zu werfen:

```csharp
string? eingabe = Console.ReadLine(); // könnte null sein
int? länge = eingabe?.Length;         // null wenn eingabe null ist, sonst die Länge

Console.WriteLine(länge ?? 0);        // 0 wenn null, sonst die Länge
```

Beide Operatoren lassen sich kombinieren:

```csharp
string[] zeilen = null;
int anzahl = zeilen?.Length ?? 0;  // 0 statt NullReferenceException
```

## Nullable in der Praxis

Nullable Types kommen typischerweise vor bei:
- Datenbankwerten, die leer sein können
- optionalen Eingaben oder Konfigurationswerten
- Rückgabewerten von Suchmethoden, die keinen Treffer liefern könnten

```csharp
static int? SucheIndex(int[] arr, int wert)
{
    for (int i = 0; i < arr.Length; i++)
        if (arr[i] == wert) return i;
    return null; // nicht gefunden
}

int? idx = SucheIndex(new[] { 3, 7, 1, 9 }, 7);
if (idx.HasValue)
    Console.WriteLine($"Gefunden an Index {idx.Value}"); // 1
else
    Console.WriteLine("Nicht gefunden.");
```

Übung: Schreibe eine Methode `LeseInt()`, die eine Konsoleneingabe liest und als `int?` zurückgibt – `null` wenn die Eingabe keine gültige Zahl ist. Nutze `TryParse`.
{: .notice--info}

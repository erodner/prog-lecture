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

## Das Problem: „kein Wert" ausdrücken

Ein `int` hat immer einen Wert — auch wenn man ihm `0` zuweist, ist das ein gültiger Wert, nicht „kein Wert". Aber was, wenn eine Messung fehlgeschlagen ist, eine Datenbankzelle leer ist oder ein Benutzer ein optionales Feld nicht ausgefüllt hat? Man bräuchte eine Möglichkeit zu sagen: „Hier gibt es keinen Wert." Genau dafür gibt es `null` — und Nullable Types erlauben es, diesen Zustand auch für Werttypen auszudrücken.

## Nullable Werttypen

Ein `?` hinter dem Typnamen macht einen Werttyp nullable — er kann dann entweder einen normalen Wert enthalten oder `null`:

```csharp
int  normale  = 42;    // muss immer einen Wert haben
int? nullable = 42;    // kann einen int-Wert haben — oder null
int? leer     = null;  // gültig!
```

`int?` ist eine Kurzschreibweise für `Nullable<int>`. Man kann sie für jeden Werttyp verwenden: `double?`, `bool?`, `char?` usw.

Der Zugriff auf den Wert erfolgt über `.Value`, aber nur wenn tatsächlich ein Wert vorhanden ist. `.HasValue` prüft das:

```csharp
int? alter = null;

if (alter.HasValue)
    Console.WriteLine($"Alter: {alter.Value}");
else
    Console.WriteLine("Alter unbekannt.");
```

Direkter Zugriff auf `.Value` ohne vorherige Prüfung wirft eine `InvalidOperationException`, wenn der Wert `null` ist. Deshalb immer vorher prüfen — oder die im Folgenden vorgestellten Operatoren verwenden.
{: .notice--warning}

## Null-Coalescing-Operator `??`

Die `HasValue`/`Value`-Prüfung ist sicher, aber etwas umständlich. Der `??`-Operator bietet eine elegantere Alternative: Er gibt den linken Wert zurück, falls er nicht `null` ist — andernfalls den rechten Standardwert:

```csharp
int? gemesseneTemp = null;
int anzeigeTemp = gemesseneTemp ?? -999;  // -999 als Ersatzwert

Console.WriteLine(anzeigeTemp); // -999
```

Man liest `??` am besten als „falls null, dann nimm stattdessen". Das funktioniert auch mit Strings und anderen Referenztypen:

```csharp
string name = Console.ReadLine();
string anzeigeName = name ?? "Unbekannt";
```

## Null-Conditional-Operator `?.`

Der `?.`-Operator löst ein anderes Problem: Was, wenn man auf eine Eigenschaft oder Methode zugreifen will, aber das Objekt selbst `null` sein könnte? Normalerweise würde das eine `NullReferenceException` auslösen. `?.` prüft automatisch auf `null` und gibt in dem Fall `null` zurück, statt abzustürzen:

```csharp
string? eingabe = Console.ReadLine(); // könnte null sein
int? länge = eingabe?.Length;         // null wenn eingabe null ist, sonst die Länge

Console.WriteLine(länge ?? 0);        // 0 wenn null, sonst die Länge
```

Beide Operatoren lassen sich kombinieren, um in einer Zeile sowohl auf `null` zu prüfen als auch einen Standardwert zu setzen:

```csharp
string[] zeilen = null;
int anzahl = zeilen?.Length ?? 0;  // 0 statt NullReferenceException
```

Ohne `?.` müsste man hier erst prüfen, ob `zeilen` nicht `null` ist, bevor man auf `Length` zugreift. Der `?.`-Operator macht das in einer Zeile.


## Weitere Quellen

- [Nullable-Werttypen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/builtin-types/nullable-value-types)
- [Null-Operatoren (`??`, `?.`) – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/operators/null-coalescing-operator)
- [Nullable-Referenztypen (Überblick) – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/nullable-references)

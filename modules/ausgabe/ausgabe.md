---
title: "Ausgabe auf der Konsole"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Ein Programm, das rechnet, aber nichts anzeigt, ist für uns nutzlos – wir sehen schlicht nicht, was es tut. Die einfachste Form, ein Ergebnis sichtbar zu machen, ist die Ausgabe auf der Konsole. Die Klasse `Console` aus dem Namensraum `System`, die wir schon in der [Grundstruktur eines C#-Programms](/modules/grundstruktur/grundstruktur.md) gesehen haben, stellt dafür die nötigen Werkzeuge bereit.

## `WriteLine` und `Write`

`Console.WriteLine` gibt eine Zeile aus und setzt danach automatisch einen Zeilenumbruch. `Console.Write` tut dasselbe, aber ohne Umbruch – die nächste Ausgabe landet direkt dahinter:

```csharp
Console.WriteLine("Hallo, Welt!");         // mit Zeilenumbruch
Console.Write("Kein Umbruch hier. ");      // ohne Zeilenumbruch
Console.WriteLine("Weiter auf gleicher Zeile.");
```

Ausgabe:
```
Hallo, Welt!
Kein Umbruch hier. Weiter auf gleicher Zeile.
```

`Write` ist vor allem dann nützlich, wenn eine Zeile aus mehreren Teilen zusammengesetzt wird oder – wie wir beim Einlesen von Eingaben sehen werden – wenn der Cursor hinter einer Frage stehen bleiben soll.

## Text und Zahlen

Alles zwischen Anführungszeichen ist ein **String** – ein Text, der genau so ausgegeben wird, wie er dasteht. Ohne Anführungszeichen interpretiert C# den Inhalt als **Ausdruck** und berechnet ihn zuerst:

```csharp
Console.WriteLine("3 + 4");   // Ausgabe: 3 + 4
Console.WriteLine(3 + 4);     // Ausgabe: 7
Console.WriteLine(10 / 4);    // Ausgabe: 2  (!)
Console.WriteLine(10.0 / 4);  // Ausgabe: 2.5
```

Die dritte Zeile ist kein Tippfehler: Teilt man zwei ganze Zahlen, ist auch das Ergebnis eine ganze Zahl – der Rest wird abgeschnitten. Warum das so ist, klären wir bei den [Datentypen](/modules/datentypen/datentypen.md) und [arithmetischen Operatoren](/modules/operatoren_arithmetik/operatoren_arithmetik.md). Ob in der letzten Zeile `2.5` oder `2,5` erscheint, hängt übrigens von der Spracheinstellung des Rechners ab.

Text und Zahlen lassen sich mit `+` aneinanderhängen. Dabei kommt es auf die Klammern an, denn `+` wird von links nach rechts ausgewertet:

```csharp
Console.WriteLine("Ergebnis: " + 3 + 4);    // Ergebnis: 34
Console.WriteLine("Ergebnis: " + (3 + 4));  // Ergebnis: 7
```

Im ersten Fall wird zuerst `"Ergebnis: " + 3` zu dem Text `"Ergebnis: 3"` verbunden, an den dann die `4` angehängt wird. Erst die Klammern sorgen dafür, dass vorher gerechnet wird.

## Sonderzeichen im String

Innerhalb von Strings haben einige Zeichen eine besondere Bedeutung – sie werden mit `\` eingeleitet und heißen **Escape-Sequenzen**:

```csharp
Console.WriteLine("Zeile 1\nZeile 2");   // \n = Zeilenumbruch
Console.WriteLine("Spalte1\tSpalte2");    // \t = Tabulator
Console.WriteLine("Er sagte \"Hallo\"");  // \" = Anführungszeichen im String
Console.WriteLine("Pfad: C:\\Users");     // \\ = ein einzelner Backslash
```

Ohne `\"` wüsste der Compiler nicht, ob ein Anführungszeichen den String beendet oder zum Text gehört – und meldet einen Fehler.

Übung: Gib mit möglichst wenigen `Console.WriteLine`-Aufrufen eine kleine Tabelle aus, die für die Zahlen 1 bis 4 jeweils die Zahl, ihr Quadrat und ihre Hälfte enthält (Spalten durch Tabulatoren getrennt, mit Überschriftenzeile). Die Werte sollen dabei *berechnet* und nicht als fertiger Text hingeschrieben werden. Achte darauf, dass die Hälfte von 3 tatsächlich als `1.5` (bzw. `1,5`) erscheint.
{: .notice--info}

## Weitere Quellen

- [Console-Klasse – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/api/system.console)
- [Hello World – Erstes Programm – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/inside-a-program/hello-world-your-first-program)

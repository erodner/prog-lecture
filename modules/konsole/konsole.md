---
title: "Ausgabe und Eingabe"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Die einfachste Form der Kommunikation zwischen Programm und Benutzer läuft über die Konsole: Text ausgeben, Text einlesen. Die Klasse `Console` aus dem Namensraum `System` stellt dafür alle nötigen Werkzeuge bereit.

## Ausgabe

`Console.WriteLine` gibt eine Zeile aus und setzt danach automatisch einen Zeilenumbruch. `Console.Write` tut dasselbe, aber ohne Umbruch:

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

Innerhalb von Strings haben einige Zeichen eine besondere Bedeutung — sie werden mit `\` eingeleitet:

```csharp
Console.WriteLine("Zeile 1\nZeile 2");   // \n = Zeilenumbruch
Console.WriteLine("Spalte1\tSpalte2");    // \t = Tabulator
Console.WriteLine("Er sagte \"Hallo\"");  // \" = Anführungszeichen im String
Console.WriteLine("Pfad: C:\\Users");     // \\ = ein einzelner Backslash
```

Variablen lassen sich direkt in die Ausgabe einbetten. Das `$` vor dem String ermöglicht es, Variablen in `{...}` einzubetten — diese Technik heißt **String-Interpolation** und wird später noch im Detail behandelt:

```csharp
string name = "Anna";
int alter = 21;
Console.WriteLine($"Name: {name}, Alter: {alter}");
```

## Eingabe

`Console.ReadLine()` wartet, bis der Benutzer eine Zeile eingibt und Enter drückt, und gibt sie als `string` zurück:

```csharp
Console.Write("Wie heißt du? ");
string name = Console.ReadLine();
Console.WriteLine($"Hallo, {name}!");
```

Da `Console.ReadLine()` immer einen `string` liefert, muss dieser für numerische Berechnungen erst umgewandelt werden:

```csharp
Console.Write("Gib dein Alter ein: ");
int alter = int.Parse(Console.ReadLine());
Console.WriteLine($"In 10 Jahren bist du {alter + 10} Jahre alt.");
```

Dies ist ein erstes Beispiel für eine explizite Typkonvertierung. Wir werden später noch weitere Beispiele sehen.

`int.Parse` schlägt mit einer Exception fehl, wenn die Eingabe keine gültige Zahl ist. Wie man das abfängt, kommt später bei der Ausnahmebehandlung.
{: .notice--warning}

Übung: Schreibe ein Programm, das nach Vorname und Nachname fragt und dann „Guten Tag, [Vorname] [Nachname]!" ausgibt.
{: .notice--info}

## Weitere Quellen

- [Console-Klasse – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/api/system.console)
- [Hello World – Erstes Programm – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/inside-a-program/hello-world-your-first-program)

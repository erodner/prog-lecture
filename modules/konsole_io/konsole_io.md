---
title: "Konsolenein- und -ausgabe"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Die Konsole ist das einfachste Interface zwischen Programm und Benutzer: Daten eingeben, Ergebnisse ausgeben. Auch wenn moderne Anwendungen grafische Oberflächen haben, ist das Konsolenmodell das Fundament – und für viele Lernprogramme vollkommen ausreichend.

## Vollständiges Eingabe-Verarbeitungs-Ausgabe-Muster

Das grundlegende Muster fast jedes Konsolenprogramms:

```csharp
// 1. Eingabe
Console.Write("Radius des Kreises: ");
double radius = double.Parse(Console.ReadLine());

// 2. Verarbeitung
double flaeche = Math.PI * radius * radius;

// 3. Ausgabe
Console.WriteLine($"Fläche: {flaeche:F2}");
```

## Formatierte Ausgabe

```csharp
double preis = 4.5;
Console.WriteLine($"{preis:F2}");    // 4,50  (2 Nachkommastellen)
Console.WriteLine($"{preis:F0}");    // 5     (gerundet, keine Nachkommastellen)
Console.WriteLine($"{1234567:N0}"); // 1.234.567  (Tausendertrennzeichen)
```

## Mehrere Werte einlesen

```csharp
Console.Write("Breite: ");
double breite = double.Parse(Console.ReadLine());

Console.Write("Höhe: ");
double hoehe = double.Parse(Console.ReadLine());

Console.WriteLine($"Fläche: {breite * hoehe:F2}");
```

## Robuste Eingabe mit `TryParse`

```csharp
Console.Write("Gib eine Zahl ein: ");
if (int.TryParse(Console.ReadLine(), out int zahl))
{
    Console.WriteLine($"Das Doppelte ist: {zahl * 2}");
}
else
{
    Console.WriteLine("Das war keine gültige Zahl.");
}
```

Übung: Schreibe einen einfachen Taschenrechner: zwei Zahlen einlesen, addieren, Ergebnis formatiert ausgeben.
{: .notice--info}

## Weitere Quellen

- [Console-Klasse – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/api/system.console)

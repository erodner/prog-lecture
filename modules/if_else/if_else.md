---
title: "if / else – Bedingte Anweisungen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Täglich treffen wir Entscheidungen: wenn es regnet, nehme ich einen Schirm mit – sonst nicht. Genau dieses Denkmuster steckt hinter `if` und `else`: der Code wählt zur Laufzeit zwischen verschiedenen Wegen, je nachdem ob eine Bedingung erfüllt ist oder nicht.

## Grundstruktur

```csharp
if (Bedingung)
{
    // wird ausgeführt, wenn Bedingung true ist
}
else
{
    // wird ausgeführt, wenn Bedingung false ist
}
```

## Einfaches Beispiel

```csharp
Console.Write("Temperatur in °C: ");
double temp = double.Parse(Console.ReadLine());

if (temp >= 20)
    Console.WriteLine("Schönes Wetter!");
else
    Console.WriteLine("Lieber eine Jacke mitnehmen.");
```

Bei einzelner Anweisung können die geschweiften Klammern weggelassen werden – empfohlen ist es trotzdem, sie zu setzen.

## else if – Mehrfache Bedingungen

```csharp
int punkte = 72;
string note;

if (punkte >= 90)       note = "sehr gut";
else if (punkte >= 75)  note = "gut";
else if (punkte >= 60)  note = "befriedigend";
else if (punkte >= 50)  note = "ausreichend";
else                    note = "nicht bestanden";

Console.WriteLine($"Note: {note}");
```

## Verschachtelte if-Anweisungen

```csharp
bool hatTicket = true;
int alter = 15;

if (hatTicket)
{
    if (alter >= 18)
        Console.WriteLine("Einlass gewährt.");
    else
        Console.WriteLine("Nur mit Begleitung.");
}
else
{
    Console.WriteLine("Kein Ticket, kein Einlass.");
}
```

Tiefe Verschachtelung vermeiden – meist gibt es eine klarere Lösung mit `&&`/`||`.
{: .notice--primary}

Übung: Schreibe ein Programm, das eine Zahl einliest und ausgibt, ob sie positiv, negativ oder null ist.
{: .notice--info}

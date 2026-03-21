---
title: "while-Schleife"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Manchmal weiß man nicht im Voraus, wie oft etwas wiederholt werden soll – zum Beispiel beim Einlesen einer Benutzereingabe, die so lange abgefragt wird, bis sie gültig ist. Die `while`-Schleife läuft genau so lange, wie eine Bedingung erfüllt ist, egal wie viele Durchläufe das erfordert.

## Syntax

```csharp
while (Bedingung)
{
    // wird wiederholt ausgeführt, solange Bedingung true ist
}
```

Die Bedingung wird **vor jedem Durchlauf** geprüft. Ist sie beim ersten Mal `false`, wird der Rumpf gar nicht ausgeführt.

## Beispiel: Countdown

```csharp
int n = 5;
while (n > 0)
{
    Console.WriteLine(n);
    n--;
}
Console.WriteLine("Start!");
```

Ausgabe: `5 4 3 2 1 Start!`

## Beispiel: Eingabe validieren

```csharp
int zahl = -1;
while (zahl < 0)
{
    Console.Write("Positive Zahl eingeben: ");
    zahl = int.Parse(Console.ReadLine());
}
Console.WriteLine($"Danke! Deine Zahl: {zahl}");
```

Die Schleife wiederholt die Eingabeaufforderung, bis eine gültige Zahl eingegeben wird.

## Gefahr: Endlosschleife

```csharp
int i = 1;
while (i > 0)  // Bedingung ist immer true!
{
    Console.WriteLine(i);
    i++;         // i wächst ins Unendliche
}
```

Immer sicherstellen, dass die Schleifenbedingung irgendwann `false` wird.
{: .notice--warning}

Übung: Schreibe ein Programm, das Zahlen einliest und aufaddiert, bis der Benutzer 0 eingibt. Gib die Summe aus.
{: .notice--info}

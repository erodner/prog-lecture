---
title: "Schleifen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Stell dir vor, du müsstest 1000 Zahlen manuell addieren oder jede Zeile einer Tabelle einzeln verarbeiten – genau dafür gibt es Schleifen. Sie machen Wiederholung beherrschbar und sind eines der wichtigsten Konzepte in der Programmierung. C# kennt vier Schleifenarten, die sich im Wesentlichen darin unterscheiden, wann und wie oft die Bedingung geprüft wird.

## while – bedingungsgesteuert

Die `while`-Schleife läuft so lange, wie eine Bedingung erfüllt ist. Die Bedingung wird **vor jedem Durchlauf** geprüft – ist sie beim ersten Mal `false`, wird der Rumpf gar nicht ausgeführt.

```csharp
while (Bedingung)
{
    // wird wiederholt, solange Bedingung true ist
}
```

Typisch: Eingabe einlesen, bis sie gültig ist.

```csharp
int zahl = -1;
while (zahl < 0)
{
    Console.Write("Positive Zahl eingeben: ");
    zahl = int.Parse(Console.ReadLine());
}
Console.WriteLine($"Danke! Deine Zahl: {zahl}");
```

Immer sicherstellen, dass die Bedingung irgendwann `false` wird – sonst entsteht eine Endlosschleife.
{: .notice--warning}

## do-while – mindestens einmal

Die `do-while`-Schleife prüft die Bedingung **nach** dem Rumpf. Der Rumpf wird also **mindestens einmal** ausgeführt.

```csharp
do
{
    // mindestens einmal ausgeführt
} while (Bedingung);
```

Typisch: Menü anzeigen, bevor der Benutzer entscheidet.

```csharp
string auswahl;
do
{
    Console.WriteLine("1 - Spielen");
    Console.WriteLine("0 - Beenden");
    Console.Write("Auswahl: ");
    auswahl = Console.ReadLine();
} while (auswahl != "0");
```

## for – zählergesteuert

Wenn man genau weiß, wie viele Durchläufe gewünscht sind, fasst `for` Zähler, Bedingung und Schritt in einer übersichtlichen Zeile zusammen.

```csharp
for (Initialisierung; Bedingung; Aktualisierung)
{
    // Schleifenrumpf
}
```

```csharp
for (int i = 0; i < 5; i++)
    Console.WriteLine($"Durchlauf {i}");

// Summe 1 bis 100:
int summe = 0;
for (int i = 1; i <= 100; i++)
    summe += i;
Console.WriteLine(summe); // 5050
```

Verschachtelte `for`-Schleifen eignen sich für Muster oder Matrizen:

```csharp
for (int zeile = 1; zeile <= 5; zeile++)
{
    for (int stern = 1; stern <= zeile; stern++)
        Console.Write("*");
    Console.WriteLine();
}
// *
// **
// ***
// ****
// *****
```

## break und continue

Manchmal soll eine Schleife vorzeitig enden oder ein einzelner Durchlauf übersprungen werden.

`break` bricht die Schleife sofort ab:

```csharp
for (int i = 0; i < zahlen.Length; i++)
{
    if (zahlen[i] == gesucht)
    {
        gefunden = true;
        break;  // gefunden – kein weiteres Suchen nötig
    }
}
```

`continue` springt direkt zum nächsten Durchlauf:

```csharp
for (int i = 1; i <= 10; i++)
{
    if (i % 2 == 0)
        continue;  // gerade Zahlen überspringen
    Console.Write(i + " ");
}
// 1 3 5 7 9
```

Übermäßiger Einsatz von `break` und `continue` kann Code schwer lesbar machen – im Zweifel lieber die Schleifenbedingung anpassen.
{: .notice--primary}

## Wann welche Schleife?

| Situation | Schleife |
| :--- | :--- |
| Anzahl der Durchläufe bekannt | `for` |
| Abbruchbedingung, Anzahl unbekannt | `while` |
| Mindestens ein Durchlauf garantiert | `do-while` |

`foreach` kommt später bei Arrays und Collections zum Einsatz – dort macht die Syntax am meisten Sinn.

Übung: Schreibe ein Ratespiel – das Programm denkt sich eine Zufallszahl zwischen 1 und 100, der Spieler rät mit Hinweisen „zu groß" / „zu klein", bis er richtig liegt.
{: .notice--info}

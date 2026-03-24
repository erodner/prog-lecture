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

Stell dir vor, du müsstest 1000 Zahlen manuell addieren oder jede Zeile einer Tabelle einzeln verarbeiten — genau dafür gibt es Schleifen. Sie machen Wiederholung beherrschbar und sind eines der wichtigsten Konzepte in der Programmierung. C# kennt vier Schleifenarten, die sich im Wesentlichen darin unterscheiden, wann und wie oft die Bedingung geprüft wird. Eine fünfte Variante — `foreach` — lernen wir später bei Arrays kennen.

## while – bedingungsgesteuert

Die `while`-Schleife ist die einfachste Form: Solange eine Bedingung `true` ist, wird der Schleifenrumpf wiederholt. Die Bedingung wird **vor jedem Durchlauf** geprüft — ist sie beim allerersten Mal schon `false`, wird der Rumpf kein einziges Mal ausgeführt.

```csharp
while (Bedingung)
{
    // wird wiederholt, solange Bedingung true ist
}
```

Man verwendet `while`, wenn man nicht weiß, wie oft die Schleife laufen wird — das Ergebnis hängt von einer Bedingung ab, die sich bei jedem Durchlauf ändern kann. Ein typischer Anwendungsfall: eine Eingabe einlesen und wiederholen, solange sie ungültig ist:

```csharp
int zahl = -1;
while (zahl < 0)
{
    Console.Write("Positive Zahl eingeben: ");
    zahl = int.Parse(Console.ReadLine());
}
Console.WriteLine($"Danke! Deine Zahl: {zahl}");
```

Hier läuft die Schleife so oft, wie der Benutzer negative Zahlen eingibt. Es kann einmal sein oder hundertmal — das Programm muss flexibel sein.

Immer sicherstellen, dass die Bedingung irgendwann `false` wird — sonst entsteht eine **Endlosschleife**: das Programm hängt und reagiert nicht mehr. Im obigen Beispiel ändert sich `zahl` bei jedem Durchlauf durch die Benutzereingabe. Ohne diese Zeile würde `zahl` für immer `-1` bleiben.
{: .notice--warning}

## do-while – mindestens einmal

Die `do-while`-Schleife funktioniert wie `while`, aber mit einem entscheidenden Unterschied: Die Bedingung wird erst **nach** dem Rumpf geprüft. Dadurch wird der Rumpf **mindestens einmal** ausgeführt, selbst wenn die Bedingung von Anfang an `false` ist.

```csharp
do
{
    // mindestens einmal ausgeführt
} while (Bedingung);   // ← Semikolon am Ende nicht vergessen!
```

Das ist immer dann sinnvoll, wenn man eine Aktion mindestens einmal ausführen muss, bevor man entscheiden kann, ob sie wiederholt werden soll. Ein typisches Beispiel ist ein Menü — man muss es erst anzeigen, bevor der Benutzer eine Wahl treffen kann:

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

Der Unterschied zu `while`: Würde man hier `while` verwenden, müsste man `auswahl` vorher mit einem Dummy-Wert initialisieren, damit die Bedingung beim ersten Durchlauf `true` ist. `do-while` macht das überflüssig.

## for – zählergesteuert

Wenn man von vornherein weiß, wie oft eine Schleife laufen soll, ist `for` die richtige Wahl. Der Schleifenkopf bündelt drei Dinge in einer Zeile: einen Zähler anlegen, die Bedingung prüfen und den Zähler verändern:

```csharp
for (Initialisierung; Bedingung; Aktualisierung)
{
    // Schleifenrumpf
}
```

Die drei Teile haben klare Aufgaben:
- **Initialisierung** — wird einmal zu Beginn ausgeführt (z.B. `int i = 0`)
- **Bedingung** — wird vor jedem Durchlauf geprüft (z.B. `i < 5`)
- **Aktualisierung** — wird nach jedem Durchlauf ausgeführt (z.B. `i++`)

```csharp
for (int i = 0; i < 5; i++)
    Console.WriteLine($"Durchlauf {i}");
```

Der Zähler `i` existiert nur innerhalb der Schleife — nach der schließenden Klammer ist er nicht mehr zugänglich. Das ist gewollt: er wird nur für die Schleifensteuerung gebraucht.

Ein klassisches Beispiel: die Summe der Zahlen von 1 bis 100 berechnen (das berühmte Gauß-Problem):

```csharp
int summe = 0;
for (int i = 1; i <= 100; i++)
    summe += i;
Console.WriteLine(summe); // 5050
```

### Verschachtelte Schleifen

`for`-Schleifen lassen sich ineinander verschachteln. Das ist nützlich, wenn man mit zwei Dimensionen arbeitet — etwa Zeilen und Spalten. Die äußere Schleife steuert die Zeilen, die innere die Spalten:

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

Für jede Zeile läuft die innere Schleife komplett durch. Bei Zeile 3 gibt die innere Schleife drei Sterne aus, bei Zeile 5 fünf. Die Anzahl der Gesamtdurchläufe ist das Produkt der beiden Zähler — bei verschachtelten Schleifen kann das schnell groß werden.

## break und continue

Manchmal reicht die Schleifenbedingung allein nicht aus, um den Ablauf zu steuern. Für solche Fälle gibt es zwei Schlüsselwörter:

**`break`** beendet die gesamte Schleife sofort — die Ausführung springt zur ersten Zeile nach der Schleife. Das ist nützlich, wenn ein Ergebnis gefunden wurde und weitere Durchläufe überflüssig sind:

```csharp
for (int i = 0; i < zahlen.Length; i++)
{
    if (zahlen[i] == gesucht)
    {
        gefunden = true;
        break;  // gefunden — kein weiteres Suchen nötig
    }
}
```

**`continue`** überspringt den Rest des aktuellen Durchlaufs und springt direkt zur Bedingungsprüfung des nächsten Durchlaufs. Das ist nützlich, wenn bestimmte Werte ausgelassen werden sollen:

```csharp
for (int i = 1; i <= 10; i++)
{
    if (i % 2 == 0)
        continue;  // gerade Zahlen überspringen
    Console.Write(i + " ");
}
// 1 3 5 7 9
```

Beide Schlüsselwörter sollte man sparsam einsetzen. Oft lässt sich derselbe Effekt durch eine geschicktere Schleifenbedingung erreichen — und das ist in der Regel lesbarer. `break` ist bei der Suche in einer Sammlung aber ein etabliertes Muster und völlig in Ordnung.
{: .notice--primary}

Übung: Schreibe ein Ratespiel — das Programm denkt sich eine Zufallszahl zwischen 1 und 100, der Spieler rät mit Hinweisen „zu groß" / „zu klein", bis er richtig liegt. Welche Schleifenart passt hier am besten?
{: .notice--info}

## Weitere Quellen

- [Iterationsanweisungen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/statements/iteration-statements)

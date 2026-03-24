---
title: "switch – Mehrfachverzweigungen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Wenn ein Wert viele verschiedene konkrete Ausprägungen haben kann – Wochentage, Menüoptionen, Befehle – wird eine lange Kette von `else if` schnell unübersichtlich. `switch` ist genau für solche Situationen gedacht, aber hat auch seine Tücken. Disclaimer: Ich bin kein großer Fan davon.

## Wann `switch` statt `if-else`?

Eine `else if`-Kette prüft beliebige Bedingungen — z.B. Bereiche (`punkte >= 90`) oder zusammengesetzte Ausdrücke. `switch` dagegen vergleicht **einen einzelnen Wert** mit einer Liste konkreter Möglichkeiten. Wenn man also wiederholt dieselbe Variable gegen feste Werte prüft, ist `switch` eine Alternative.

## Klassisches `switch`

Die `switch`-Anweisung erhält in Klammern den zu prüfenden Ausdruck. Im Rumpf folgen `case`-Marken mit den möglichen Werten. Stimmt der Wert überein, wird der zugehörige Code ausgeführt. `default` fängt alle nicht explizit genannten Fälle ab — vergleichbar mit dem abschließenden `else`:

```csharp
int tag = 3;
string name;

switch (tag)
{
    case 1: name = "Montag";     break;
    case 2: name = "Dienstag";   break;
    case 3: name = "Mittwoch";   break;
    case 4: name = "Donnerstag"; break;
    case 5: name = "Freitag";    break;
    default: name = "Wochenende"; break;
}

Console.WriteLine(name); // Mittwoch
```

Jeder `case`-Zweig muss mit `break` enden. In C# ist dies Pflicht — ohne `break` meldet der Compiler einen Fehler. Das unterscheidet C# von Sprachen wie C oder Java, wo das Vergessen von `break` dazu führt, dass die Ausführung stillschweigend in den nächsten Fall „durchfällt" (sogenanntes *fall-through*). Furchtbar!
{: .notice--primary}

Wenn mehrere Werte zum selben Ergebnis führen sollen, kann man die `case`-Marken direkt untereinander schreiben. Nur der letzte Fall in der Gruppe enthält den eigentlichen Code und das `break`:

```csharp
switch (tag)
{
    case 1:
    case 2:
    case 3:
    case 4:
    case 5:
        Console.WriteLine("Werktag");
        break;
    case 6:
    case 7:
        Console.WriteLine("Wochenende");
        break;
}
```

Das ist das einzige erlaubte „Durchfallen" in C#: Ein `case` ohne eigenen Code darf zum nächsten weiterleiten. Sobald ein `case` Code enthält, muss er mit `break` abgeschlossen werden. Schön ist das trotzdem nicht, oder?

## Moderne switch-Expression (C# 8+)

Seit C# 8 gibt es die **switch-Expression** — eine kompaktere Schreibweise, die direkt einen Wert zurückgibt. Sie eignet sich besonders, wenn man einer Variable je nach Fall einen unterschiedlichen Wert zuweisen will:

```csharp
string jahreszeit = monat switch
{
    12 or 1 or 2 => "Winter",
    3 or 4 or 5  => "Frühling",
    6 or 7 or 8  => "Sommer",
    9 or 10 or 11 => "Herbst",
    _ => "Ungültiger Monat"
};
```

Die Unterschiede zur klassischen `switch`-Anweisung: Der geprüfte Wert steht **vor** dem Schlüsselwort `switch`, die Fälle verwenden `=>` statt `case`/`break`, und `_` ersetzt `default`. Das `or`-Schlüsselwort fasst mehrere Werte zusammen. Weil die switch-Expression einen Wert zurückgibt, muss sie alle Fälle abdecken — der `_`-Zweig stellt das sicher. Ist das jetzt wirklich besser zu lesen?

## switch mit Strings

`switch` funktioniert nicht nur mit Zahlen, sondern auch mit Strings. Das ist nützlich für Menüs oder Befehlseingaben. Wie bei allen String-Vergleichen wird zwischen Groß- und Kleinschreibung unterschieden — deshalb empfiehlt sich ein `ToLower()` vor dem Vergleich:

```csharp
string befehl = Console.ReadLine();

switch (befehl.ToLower())
{
    case "start": Console.WriteLine("Programm gestartet."); break;
    case "stop":  Console.WriteLine("Programm gestoppt."); break;
    default:      Console.WriteLine("Unbekannter Befehl."); break;
}
```

Übung: Schreibe mit einer switch-Expression einen einfachen Rechner: Operator (`+`, `-`, `*`, `/`) und zwei Zahlen einlesen, Ergebnis ausgeben.
{: .notice--info}

## Weitere Quellen

- [switch-Anweisung und switch-Ausdruck – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/statements/selection-statements#the-switch-statement)
- [Musterabgleich mit switch – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/fundamentals/functional/pattern-matching)

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

Die `if`-Anweisung prüft eine Bedingung (einen Ausdruck vom Typ `bool`) und führt den nachfolgenden Block nur aus, wenn die Bedingung `true` ergibt. Der optionale `else`-Zweig fängt den gegenteiligen Fall ab:

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

Die Bedingung muss immer in runden Klammern stehen. Der `else`-Zweig ist optional — wenn man nur auf den positiven Fall reagieren will, kann man ihn weglassen.

Hier einmal ein einfaches Beispiel:
```csharp
Console.Write("Temperatur in °C: ");
double temp = double.Parse(Console.ReadLine());

if (temp >= 20)
    Console.WriteLine("Schönes Wetter!");
else
    Console.WriteLine("Lieber eine Jacke mitnehmen.");
```

Hier wird genau einer der beiden `WriteLine`-Aufrufe ausgeführt — nie beide und nie keiner. Das ist ein zentrales Merkmal von `if/else`: Die beiden Zweige schließen sich gegenseitig aus.

Bei einer einzelnen Anweisung pro Zweig können die geschweiften Klammern weggelassen werden. Empfohlen ist es trotzdem, sie immer zu setzen — das beugt Fehlern vor, wenn man später eine zweite Anweisung ergänzt und die Klammern vergisst.
{: .notice--primary}

## Mehrfache Bedingungen

Oft reichen zwei Fälle nicht aus. Mit `else if` lassen sich beliebig viele Bedingungen aneinanderreihen. Die Bedingungen werden **von oben nach unten** geprüft, und der erste zutreffende Zweig wird ausgeführt — alle weiteren werden übersprungen:

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

Die Reihenfolge ist hier entscheidend: Weil `punkte >= 90` die strengste Bedingung ist, steht sie oben. Würde man mit `punkte >= 50` beginnen, würde dieser Zweig bereits für 72 Punkte greifen und „ausreichend" liefern — obwohl „befriedigend" korrekt wäre. Bei `else if`-Ketten immer von der speziellsten zur allgemeinsten Bedingung ordnen.

Der abschließende `else`-Zweig dient als Auffangnetz für alle Fälle, die keine der vorherigen Bedingungen erfüllen. Man sollte ihn nicht weglassen, wenn jeder Eingabewert eine Antwort bekommen soll.
{: .notice--primary}

## Verschachtelte if-Anweisungen

`if`-Anweisungen können ineinander verschachtelt werden, um mehrstufige Entscheidungen abzubilden. Im folgenden Beispiel wird zuerst geprüft, ob ein Ticket vorhanden ist, und nur in diesem Fall wird das Alter weiter untersucht:

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

Verschachtelungen sind manchmal unvermeidlich, aber ab zwei oder drei Ebenen wird der Code schwer lesbar. Oft lässt sich dasselbe mit logischen Operatoren (`&&`, `||`) in einer einzigen Bedingung ausdrücken — das ist in der Regel klarer:

```csharp
if (hatTicket && alter >= 18)
    Console.WriteLine("Einlass gewährt.");
else if (hatTicket)
    Console.WriteLine("Nur mit Begleitung.");
else
    Console.WriteLine("Kein Ticket, kein Einlass.");
```

Tiefe Verschachtelung vermeiden — meist gibt es eine flachere Lösung mit `&&`/`||` oder einem frühzeitigen `return`.
{: .notice--primary}

Übung: Schreibe ein Programm, das eine Zahl einliest und ausgibt, ob sie positiv, negativ oder null ist.
{: .notice--info}

## Weitere Quellen

- [if-Anweisung – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/statements/selection-statements#the-if-statement)
- [Verzweigungen (Tutorial) – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/tour-of-csharp/tutorials/branches-and-loops-local#make-decisions-using-the-if-statement)

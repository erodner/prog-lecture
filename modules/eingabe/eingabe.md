---
title: "Eingabe über die Konsole"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Bisher laufen unsere Programme immer gleich ab: Die Werte stehen fest im Code, und jede Ausführung liefert dieselbe Ausgabe. Interessant wird ein Programm erst, wenn es auf den Benutzer reagiert. Dafür brauchen wir beides, was wir bereits kennen: die [Ausgabe](/modules/ausgabe/ausgabe.md), um eine Frage zu stellen, und eine [Variable](/modules/variablen_intro/variablen_intro.md), um die Antwort festzuhalten.

## Text einlesen

`Console.ReadLine()` wartet, bis der Benutzer eine Zeile eingibt und Enter drückt, und liefert diese Zeile als `string` zurück. Damit die Eingabe nicht sofort wieder verloren geht, speichern wir sie in einer Variablen:

```csharp
Console.Write("Wie heißt du? ");
string name = Console.ReadLine();
Console.WriteLine($"Hallo, {name}!");
```

Hier zahlt sich `Console.Write` aus: Weil kein Zeilenumbruch folgt, tippt der Benutzer seine Antwort direkt hinter die Frage. Die Ausgabe am Ende nutzt die String-Interpolation, die wir bei den Variablen kennengelernt haben.

Je nach Projekteinstellung zeigt der Compiler bei `string name = Console.ReadLine();` eine Warnung an. Sie hat mit sogenannten Nullable-Typen zu tun, die wir später behandeln – für den Moment kannst du sie ignorieren.
{: .notice--primary}

## Zahlen einlesen

`Console.ReadLine()` liefert *immer* einen `string` – auch wenn der Benutzer `21` eintippt. Mit dem Text `"21"` kann man aber nicht rechnen. Er muss erst in eine Zahl umgewandelt werden:

```csharp
Console.Write("Gib dein Alter ein: ");
int alter = int.Parse(Console.ReadLine());
Console.WriteLine($"In 10 Jahren bist du {alter + 10} Jahre alt.");
```

`int.Parse` nimmt einen Text und macht daraus eine ganze Zahl. Das ist ein erstes Beispiel für eine **Typkonvertierung** – in der nächsten Einheit sehen wir, welche weiteren Möglichkeiten es gibt.

Was passiert ohne die Umwandlung? Hängt man an den eingelesenen Text einfach `+ 10` an, bekommt man – wie bei der Ausgabe gesehen – keine Rechnung, sondern eine Textverkettung: Aus `"21"` wird `"2110"`.

Für Kommazahlen gibt es entsprechend `double.Parse`. Dabei kommt es auf die Spracheinstellung des Rechners an: Auf einem deutsch eingestellten System erwartet `double.Parse` ein Komma (`2,5`), auf einem englischen einen Punkt (`2.5`).

```csharp
Console.Write("Preis in Euro: ");
double preis = double.Parse(Console.ReadLine());
Console.WriteLine($"Mit 19 % MwSt.: {preis * 1.19} Euro");
```

`int.Parse` und `double.Parse` brechen das Programm mit einer Exception ab, wenn die Eingabe keine gültige Zahl ist – etwa `"einundzwanzig"` oder ein leerer Text. Wie man das abfängt, kommt später bei der Ausnahmebehandlung.
{: .notice--warning}

Übung: Schreibe ein Programm, das nach Vorname, Nachname und Geburtsjahr fragt und dann z. B. „Guten Tag, Anna Schmidt! Du wirst dieses Jahr 21 Jahre alt." ausgibt. Das aktuelle Jahr darf als Wert im Code stehen. Was passiert, wenn du beim Geburtsjahr Buchstaben eingibst?
{: .notice--info}

Übung: Schreibe einen kleinen Trinkgeldrechner. Das Programm fragt nach dem Rechnungsbetrag, dem gewünschten Trinkgeld in Prozent und der Anzahl der Personen und gibt aus, wie viel Trinkgeld insgesamt anfällt und wie viel jede Person zahlen muss. Teste dein Programm mit einem Betrag wie `47,80` – und überlege, welchen Typ die Anzahl der Personen haben sollte.
{: .notice--info}

## Weitere Quellen

- [Console.ReadLine – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/api/system.console.readline)
- [Int32.Parse – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/api/system.int32.parse)

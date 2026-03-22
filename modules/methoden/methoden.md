---
title: "Methoden definieren und aufrufen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Stell dir vor, du bräuchtest dieselbe Berechnung an zehn verschiedenen Stellen in deinem Programm – und dann ändert sich die Formel. Ohne Methoden müsstest du zehn Stellen anpassen, und du würdest garantiert eine vergessen. Mit einer Methode änderst du einmal, und alles funktioniert. Das ist der Kern von wiederverwendbarem Code.

Methoden sind direkter Ausdruck von **Zerlegung** (*Decomposition*) als einem der Grundprinzipien des Computational Thinking: Ein komplexes Problem wird in kleinere, benennbare Teilprobleme aufgeteilt – jede Methode löst genau eines davon. Wer lernt, Aufgaben sauber in Methoden zu zerlegen, denkt bereits wie eine Softwareentwicklerin.

## Aufbau einer Methode

Eine Methode besteht aus einer **Signatur** (Name, Parameter, Rückgabetyp) und einem **Rumpf** (der eigentliche Code):

```csharp
Rückgabetyp MethodenName(Typ parameter1, Typ parameter2)
{
    // Methodenrumpf
    return ergebnis;
}
```

- **Rückgabetyp**: Was die Methode zurückliefert (`int`, `double`, `string`, …). Gibt sie nichts zurück, steht hier `void`.
- **Parameter**: Die Eingaben, die die Methode braucht, um ihre Aufgabe zu erfüllen. Eine Methode kann beliebig viele Parameter haben – oder gar keine.
- **`return`**: Beendet die Methode und gibt den Wert zurück. Bei `void`-Methoden entfällt `return` (oder wird ohne Wert verwendet).

Das Schlüsselwort `static` bedeutet hier, dass die Methode zur Klasse gehört und nicht an ein Objekt gebunden ist. Was das genau bedeutet, wird im Kapitel zur Objektorientierung klar – für jetzt reicht es, es als festen Bestandteil der Signatur zu akzeptieren.
{: .notice--primary}

## Methoden mit Rückgabewert

Eine Methode, die einen Wert berechnet und zurückgibt:

```csharp
static double Kreisfläche(double radius)
{
    double fläche = Math.PI * radius * radius;
    return fläche;
}
```

Der Aufruf sieht so aus – das Ergebnis kann direkt in einer Variable gespeichert oder weiterverwendet werden:

```csharp
double f = Kreisfläche(5.0);
Console.WriteLine($"Fläche: {f:F2}"); // 78,54

// Oder direkt eingebettet:
Console.WriteLine($"Fläche: {Kreisfläche(3.0):F2}"); // 28,27
```

## Methoden ohne Rückgabewert (`void`)

Manche Methoden führen eine Aktion aus, ohne etwas zu berechnen – z.B. etwas ausgeben oder einen Zustand verändern. Der Rückgabetyp ist dann `void`:

```csharp
static void ZeigeNotenInfo(int punkte)
{
    string beschreibung;
    if (punkte >= 90)      beschreibung = "sehr gut";
    else if (punkte >= 75) beschreibung = "gut";
    else if (punkte >= 60) beschreibung = "befriedigend";
    else if (punkte >= 50) beschreibung = "ausreichend";
    else                   beschreibung = "nicht bestanden";

    Console.WriteLine($"{punkte} Punkte → {beschreibung}");
}

ZeigeNotenInfo(82); // 82 Punkte → gut
ZeigeNotenInfo(47); // 47 Punkte → nicht bestanden
```

## Parameter: Eingaben für die Methode

Parameter machen Methoden flexibel – statt hart codierter Werte arbeitet die Methode mit dem, was beim Aufruf übergeben wird:

```csharp
static double Rechteckfläche(double breite, double höhe)
{
    return breite * höhe;
}

Console.WriteLine(Rechteckfläche(5.0, 3.0));  // 15
Console.WriteLine(Rechteckfläche(4.0, 7.0));  // 28
```

Die Namen der Parameter gelten nur innerhalb der Methode. Was außen `breite` heißt, könnte innen genauso gut `b` heißen – die Verbindung erfolgt über die Position beim Aufruf, nicht über den Namen.

## Kurzform mit `=>`

Für Methoden, deren Rumpf nur aus einem einzigen `return`-Ausdruck besteht, gibt es eine kompaktere Schreibweise mit `=>`:

```csharp
// Standardform:
static double Kreisfläche(double radius)
{
    return Math.PI * radius * radius;
}

// Kurzform (expression-bodied method):
static double Kreisfläche(double radius) => Math.PI * radius * radius;
```

Beide Varianten sind identisch – die Kurzform ist bei einfachen Berechnungen lesbarer, die Standardform besser geeignet, wenn mehrere Schritte nötig sind.

## Variable Parameteranzahl mit `params`

Manchmal weiß man beim Schreiben einer Methode nicht, wie viele Argumente übergeben werden – man will eine Methode aufrufen wie `Summe(1, 2, 3)` oder `Summe(1, 2, 3, 4, 5)`, ohne für jede Anzahl eine eigene Überladung zu schreiben. Dafür gibt es das Schlüsselwort `params`:

```csharp
static int Summe(params int[] zahlen)
{
    int summe = 0;
    foreach (int z in zahlen)
        summe += z;
    return summe;
}
```

Der Aufrufer kann beliebig viele Argumente übergeben – oder auch ein Array:

```csharp
Console.WriteLine(Summe(1, 2, 3));          // 6
Console.WriteLine(Summe(10, 20, 30, 40));   // 100
Console.WriteLine(Summe());                 // 0 – auch kein Argument ist erlaubt

int[] werte = { 5, 10, 15 };
Console.WriteLine(Summe(werte));            // 30 – Array direkt übergeben
```

`params` muss der letzte Parameter in der Signatur sein und darf nur einmal vorkommen. Ein bekanntes Beispiel aus der BCL: `Console.WriteLine("{0} und {1}", a, b)` nutzt intern `params object[]`.
{: .notice--primary}

Übung: Schreibe Methoden `Maximum(int a, int b)`, `Minimum(int a, int b)` und `Durchschnitt(params double[] werte)`. Nutze wo sinnvoll die Kurzform mit `=>`.
{: .notice--info}

## Weitere Quellen

- [Methoden – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/classes-and-structs/methods)
- [params-Schlüsselwort – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/keywords/params)

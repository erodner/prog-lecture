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

Tatsächlich arbeiten wir schon seit der ersten Vorlesung mit Methoden: `Console.WriteLine` und `int.Parse` sind Methoden, die andere für uns geschrieben haben, und selbst `Main` ist eine Methode – diejenige, mit der jedes Programm startet. Jetzt lernen wir, eigene zu schreiben.

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

Im einfachsten Fall bekommt eine Methode Eingaben (Parameter) und liefert ein Ergebnis zurück — genau wie eine mathematische Funktion:

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
Console.WriteLine($"Fläche: {f}"); // 78,53981633974483

// Oder direkt eingebettet – hier gleich als Argument einer anderen Methode:
Console.WriteLine($"Fläche: {Math.Round(Kreisfläche(3.0), 2)}"); // 28,27
```

Im zweiten Aufruf wird zuerst `Kreisfläche(3.0)` ausgewertet, und deren Ergebnis wird dann an `Math.Round` übergeben, das auf zwei Nachkommastellen rundet. Methodenaufrufe lassen sich also genauso verschachteln wie Rechenausdrücke.

## Methoden ohne Rückgabewert (`void`)

Nicht jede Methode berechnet ein Ergebnis. Manche führen eine Aktion aus — etwas ausgeben, eine Datei schreiben, einen Zustand verändern. Das folgende Beispiel verpackt eine `if`/`else`-Kette, wie wir sie bei den [Verzweigungen](/modules/if_else/if_else.md) geschrieben haben, in eine Methode, die man mit beliebigen Punktzahlen aufrufen kann. Solche Methoden haben den Rückgabetyp `void` (englisch für „leer") und kein `return` mit Wert:

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

Die Namen der Parameter gelten nur innerhalb der Methode. Was außen `breite` heißt, könnte innen genauso gut `b` heißen — die Verbindung erfolgt über die **Position** beim Aufruf, nicht über den Namen. Im Aufruf `Rechteckfläche(5.0, 3.0)` wird `5.0` dem ersten Parameter `breite` zugeordnet und `3.0` dem zweiten Parameter `höhe`.

Methodennamen werden in C# per Konvention in **PascalCase** geschrieben (z.B. `Kreisfläche`, `ZeigeNotenInfo`), Parameter in **camelCase** (z.B. `radius`, `breite`).
{: .notice--primary}

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

Übung: Schreibe Methoden `Maximum(int a, int b)` und `Minimum(int a, int b)` mit dem ternären Operator und der Kurzform `=>`. Schreibe dann `Maximum3(int a, int b, int c)` und `Median3(int a, int b, int c)` für drei Zahlen, und zwar so, dass du möglichst viel von den bereits geschriebenen Methoden wiederverwendest. Teste `Median3` mit allen sechs Reihenfolgen von `1, 2, 3`.
{: .notice--info}

## Weitere Quellen

- [Methoden – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/classes-and-structs/methods)
- [Expression-bodied Members – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/statements-expressions-operators/expression-bodied-members)

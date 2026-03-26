---
title: "Methoden in Klassen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Objekte haben nicht nur Daten – sie können auch etwas tun. Instanzmethoden beschreiben das Verhalten eines Objekts und haben dabei direkten Zugriff auf seine eigenen Felder und Properties. Statische Methoden gehören hingegen zur Klasse selbst, nicht zu einer konkreten Instanz.

## Instanzmethoden vs. statische Methoden

Bisher haben wir Methoden immer mit `static` geschrieben — sie gehörten zur Klasse und hatten keinen Zugriff auf ein konkretes Objekt. In der objektorientierten Programmierung kommen nun **Instanzmethoden** hinzu: Methoden ohne `static`, die auf einem bestimmten Objekt arbeiten und dessen Felder und Properties lesen und verändern können.

**Instanzmethoden** werden auf einem Objekt aufgerufen (`konto.Einzahlen(100)`) und haben Zugriff auf die Daten dieses Objekts.

**Statische Methoden** gehören zur Klasse selbst (`Konto.EuroInDollar(100)`) und können nicht auf Instanzdaten zugreifen — sie brauchen kein Objekt.

```csharp
class Konto
{
    public double Stand { get; private set; }

    // Instanzmethode – arbeitet auf diesem Objekt
    public void Einzahlen(double betrag) => Stand += betrag;
    public void Abheben(double betrag)
    {
        if (betrag > Stand)
            throw new InvalidOperationException("Nicht genug Guthaben.");
        Stand -= betrag;
    }

    // Statische Methode – kein Zugriff auf Stand nötig
    public static double EuroInDollar(double euro) => euro * 1.08;
}

var k = new Konto();
k.Einzahlen(500);
k.Abheben(200);
Console.WriteLine(k.Stand); // 300

Console.WriteLine(Konto.EuroInDollar(100)); // 108
```

Der Unterschied zeigt sich im Aufruf: `k.Einzahlen(500)` wirkt auf das konkrete Objekt `k`, während `Konto.EuroInDollar(100)` keinen Bezug zu einem bestimmten Konto hat. Instanzmethoden werden auf einer **Variable** aufgerufen (`k.Einzahlen`), statische Methoden auf dem **Klassennamen** (`Konto.EuroInDollar`).

Eine statische Methode kann keine Instanzmethode aufrufen und nicht auf `Stand` oder andere Instanzdaten zugreifen — sie hat kein `this`. Umgekehrt kann eine Instanzmethode problemlos statische Methoden der eigenen Klasse aufrufen.
{: .notice--primary}

## Statische Methoden — wozu?

Statische Methoden kennen wir bereits aus dem gesamten bisherigen Kurs: Alle Methoden, die wir bisher geschrieben haben, waren `static`, weil es noch keine Objekte gab. Auch viele Methoden der .NET-Bibliothek sind statisch — `Math.Sqrt()`, `int.Parse()`, `Array.Sort()`. Sie gehören zur Klasse, nicht zu einem einzelnen Objekt, weil sie keine Instanzdaten brauchen.

Statische Methoden eignen sich für:
- **Hilfsfunktionen**, die nur mit ihren Parametern arbeiten (z.B. `Math.Max`, `EuroInDollar`)
- **Factory-Methoden**, die neue Objekte erzeugen und zurückgeben
- **Einstiegspunkte** wie `Main` (siehe unten)

Das erklärt auch, warum unser bisheriger Code immer `static` verwendete — es gab schlicht keine Objekte, auf denen Instanzmethoden hätten arbeiten können. Das folgende Pattern zeigt genau diesen Zusammenhang.

## Das klassische Programm-Pattern

Mit Top-Level Statements schreibt man Code direkt in die Datei. Hinter den Kulissen generiert der Compiler daraus eine Klasse mit einer statischen `Main`-Methode. Vor C# 9 musste man dieses Gerüst selbst schreiben — und in größeren Projekten sieht man es immer noch:

```csharp
class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Hallo Welt!");
    }
}
```

`Main` muss `static` sein, weil beim Programmstart noch kein Objekt existiert — es gibt keine Instanz von `Program`, auf der man eine Methode aufrufen könnte. Das ist genau der Grund, warum wir bisher alle Methoden als `static` deklariert haben: Sie wurden direkt oder indirekt aus `Main` aufgerufen, und eine statische Methode kann nur andere statische Methoden aufrufen (oder erst ein Objekt erzeugen).

Mit der Objektorientierung ändert sich das: Man erzeugt in `Main` (oder im Top-Level Code) Objekte und ruft deren Instanzmethoden auf. Die Programmlogik wandert zunehmend in Klassen, und `Main` wird zum schlanken Einstiegspunkt.

Sobald die Logik in Klassen lebt, stellt sich die nächste Frage: Wie strukturiert man die Methoden innerhalb einer Klasse? Genau wie bei statischen Methoden gilt auch hier — große Aufgaben lassen sich in kleinere, verständliche Schritte zerlegen.

## Methoden, die andere Methoden aufrufen

Innerhalb einer Klasse können Methoden sich gegenseitig aufrufen. Das erlaubt es, komplexe Berechnungen in kleinere, verständliche Schritte zu zerlegen — ein Prinzip, das wir bereits von statischen Methoden kennen:

```csharp
class Bestellung
{
    List<double> positionen = new List<double>();

    public void HinzufügenPosition(double preis) => positionen.Add(preis);

    private double Nettobetrag() => positionen.Sum();
    private double Mehrwertsteuer() => Nettobetrag() * 0.19;

    public double Gesamtbetrag() => Nettobetrag() + Mehrwertsteuer();

    public void Ausgeben()
    {
        Console.WriteLine($"Netto:  {Nettobetrag(),8:F2} €");
        Console.WriteLine($"MwSt.:  {Mehrwertsteuer(),8:F2} €");
        Console.WriteLine($"Gesamt: {Gesamtbetrag(),8:F2} €");
    }
}
```

Die privaten Methoden `Nettobetrag()` und `Mehrwertsteuer()` sind Implementierungsdetails — von außen sieht man nur `Gesamtbetrag()` und `Ausgeben()`. Das ist Kapselung in der Praxis: Die interne Aufteilung kann sich ändern, ohne dass der Aufrufer etwas davon merkt.

Übung: Erweitere die Klasse `Konto` aus dem vorherigen Modul um eine Methode `Überweisen(Konto ziel, double betrag)`, die vom eigenen Konto abhebt und auf das Zielkonto einzahlt. Füge eine statische Methode `Konto.Wechselkurs(string währung)` hinzu, die für verschiedene Währungen einen festen Kurs zurückgibt.
{: .notice--info}

## Weitere Quellen

- [Instanzmethoden – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/classes-and-structs/methods)
- [Statische Klassen und Member – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/classes-and-structs/static-classes-and-static-class-members)
- [Main und Befehlszeilenargumente – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/fundamentals/program-structure/main-command-line)

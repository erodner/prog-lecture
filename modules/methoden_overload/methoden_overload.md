---
title: "Methodenüberladung"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

`Console.WriteLine` kann eine ganze Zahl, eine Kommazahl, einen Text oder einen booleschen Wert ausgeben – immer mit demselben Methodennamen. Wäre das nicht möglich, müsste man `WriteLineInt`, `WriteLineDouble`, `WriteLineString` und so weiter aufrufen. Das wäre umständlich und schwer zu merken. Dieses Prinzip heißt **Überladung**: mehrere Methoden teilen sich einen Namen, unterscheiden sich aber in ihren Parametern.

## Das Problem ohne Überladung

Angenommen, man möchte prüfen, ob ein bestimmter Wert in einem Array vorkommt. Ohne Überladung bräuchte man für jeden Typ einen eigenen Methodennamen:

```csharp
static bool EnthältInt(int[] arr, int wert) { ... }
static bool EnthältString(string[] arr, string wert) { ... }
static bool EnthältDouble(double[] arr, double wert) { ... }
```

Das funktioniert, aber der Aufrufer muss sich drei verschiedene Namen merken — obwohl die Absicht immer dieselbe ist: prüfen, ob ein Wert enthalten ist.

## Überladung: ein Name, mehrere Signaturen

Mit Überladung können alle Varianten `Enthält` heißen. Der Compiler entscheidet **zur Compilierzeit** anhand der Typen der übergebenen Argumente, welche Version gemeint ist:

```csharp
static bool Enthält(int[] arr, int wert)
{
    for (int i = 0; i < arr.Length; i++)
        if (arr[i] == wert) return true;
    return false;
}

static bool Enthält(string[] arr, string wert)
{
    for (int i = 0; i < arr.Length; i++)
        if (arr[i] == wert) return true;
    return false;
}

static bool Enthält(double[] arr, double wert)
{
    for (int i = 0; i < arr.Length; i++)
        if (arr[i] == wert) return true;
    return false;
}
```

```csharp
int[] noten = { 1, 2, 3, 4, 5 };
string[] farben = { "rot", "grün", "blau" };

Console.WriteLine(Enthält(noten, 3));        // True
Console.WriteLine(Enthält(farben, "gelb"));  // False
```

Der Aufrufer schreibt einfach `Enthält` — der Compiler wählt die richtige Variante anhand des Array-Typs. Die Regel: Zwei Methoden dürfen denselben Namen haben, wenn sie sich in **Anzahl oder Typ der Parameter** unterscheiden. Der Rückgabetyp allein reicht nicht — zwei Methoden mit identischen Parametern aber unterschiedlichem Rückgabetyp sind ein Compilerfehler.
{: .notice--warning}

Man sieht: Der Code der drei Varianten ist fast identisch. Das ist ein Hinweis darauf, dass es elegantere Lösungen gibt (Stichwort *Generics*) — aber dafür braucht man Konzepte, die erst später kommen. Überladung ist der einfachste Weg, dieses Problem jetzt zu lösen.
{: .notice--primary}

## Überladung in der BCL

Überladung ist kein exotisches Feature — man nutzt sie ständig, ohne es zu merken. `Console.WriteLine` selbst hat über ein Dutzend Überladungen in der .NET-Klassenbibliothek (BCL):

```csharp
Console.WriteLine(42);       // void WriteLine(int value)
Console.WriteLine(3.14);     // void WriteLine(double value)
Console.WriteLine("Hallo");  // void WriteLine(string value)
Console.WriteLine(true);     // void WriteLine(bool value)
```

Dasselbe gilt für `Math.Max`, `Math.Min`, `Math.Abs` und viele weitere — jeweils in Varianten für `int`, `double`, `float` usw.

## Optionale Parameter (Standardwerte)

Nicht immer braucht man eine eigene Überladung. Wenn dieselbe Logik mit oder ohne einen zusätzlichen Wert funktionieren soll, kann man stattdessen **optionale Parameter** mit Standardwerten verwenden. Der Aufrufer kann sie dann weglassen — in dem Fall greift der Standardwert:

```csharp
static void Begrüße(string name, string anrede = "Hallo")
{
    Console.WriteLine($"{anrede}, {name}!");
}

Begrüße("Anna");                       // Hallo, Anna!
Begrüße("Prof. Müller", "Guten Tag"); // Guten Tag, Prof. Müller!
```

Der Standardwert wird direkt in der Signatur mit `=` angegeben. Der Compiler setzt den Standardwert automatisch ein, wenn der Aufrufer das Argument weglässt. Das spart eine zweite Überladung, die ohnehin nur den Standardwert weiterreichen würde.

Optionale Parameter müssen immer **am Ende** der Parameterliste stehen — ein Parameter mit Standardwert darf nicht vor einem ohne stehen. Außerdem müssen Standardwerte **Kompilierzeitkonstanten** sein (also Zahlenliterale, Strings oder `null` — keine Methodenaufrufe oder berechnete Werte).
{: .notice--primary}

Optionale Parameter eignen sich auch gut, um eine Methode schrittweise zu erweitern, ohne bestehende Aufrufe anpassen zu müssen:

```csharp
static void ZeigeTabelle(int[] werte, string titel = "Ergebnis", int spaltenBreite = 8)
{
    Console.WriteLine(titel);
    foreach (int w in werte)
        Console.Write($"{w,spaltenBreite}");
    Console.WriteLine();
}

int[] noten = { 1, 2, 3, 2, 1 };
ZeigeTabelle(noten);                              // Standardtitel und -breite
ZeigeTabelle(noten, "Notenliste");                // eigener Titel, Standardbreite
ZeigeTabelle(noten, "Notenliste", 12);            // alles angegeben
```

Übung: Schreibe eine Methode `Wiederhole(string text)`, die den Text 3-mal ausgibt, und eine Überladung `Wiederhole(string text, int anzahl)` mit wählbarer Anzahl. Überlege dann, ob sich das Problem eleganter mit einem optionalen Parameter lösen lässt.
{: .notice--info}

## Weitere Quellen

- [Methodenüberladung – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/standard/design-guidelines/member-overloading)
- [Optionale und benannte Argumente – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/classes-and-structs/named-and-optional-arguments)

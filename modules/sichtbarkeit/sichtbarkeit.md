---
title: "Sichtbarkeit – Zugriffsmodifizierer"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Im [Klassen-Modul](/modules/klassen/klassen.md) stand vor jedem Member ein `public` — ohne dass wir darüber gesprochen haben. Doch nicht alles in einer Klasse soll von außen zugänglich sein – interne Hilfsmethoden, Rohwerte und Implementierungsdetails bleiben besser verborgen. Zugriffsmodifizierer legen fest, wer auf was zugreifen darf, und sind das Werkzeug, mit dem man Kapselung in der Praxis umsetzt.

## Übersicht

| Modifizierer | Zugreifbar von |
| :--- | :--- |
| `public` | Überall |
| `private` | Nur innerhalb der Klasse |
| `protected` | Innerhalb der Klasse und Unterklassen |
| `internal` | Nur innerhalb des gleichen Projekts (Assembly) |

## Kapselung mit `private`

Die Tabelle zeigt die vier Stufen — in der Praxis ist `private` die mit Abstand wichtigste, denn sie setzt das zentrale OOP-Prinzip der Kapselung um.

Das Prinzip der **Kapselung** (*Encapsulation*) ist eines der Grundpfeiler der Objektorientierung: Interne Daten werden verborgen, der Zugriff läuft ausschließlich über kontrollierte Schnittstellen — vorerst über Methoden, ab dem nächsten Modul auch über Properties. So kann die interne Darstellung geändert werden, ohne dass Code außerhalb der Klasse betroffen ist:

```csharp
class Temperatur
{
    private double kelvin;  // intern immer in Kelvin speichern

    public bool SetzeCelsius(double celsius)
    {
        if (celsius < -273.15)
            return false;   // unter dem absoluten Nullpunkt – Wert ablehnen
        kelvin = celsius + 273.15;
        return true;
    }

    public double GibCelsius() => kelvin - 273.15;
    public double GibFahrenheit() => GibCelsius() * 9 / 5 + 32;
}

var t = new Temperatur();
t.SetzeCelsius(100);
Console.WriteLine(t.GibFahrenheit());  // 212
Console.WriteLine(t.GibCelsius());     // 100

bool ok = t.SetzeCelsius(-500);        // false – Wert wird abgelehnt
// t.kelvin = -500; // Compilerfehler – private!
```

Im Beispiel speichert die Klasse die Temperatur intern in Kelvin, bietet aber nach außen Celsius und Fahrenheit an. Der Aufrufer merkt nichts von der internen Darstellung — und man könnte sie ändern (z.B. auf Celsius intern), ohne dass sich die Schnittstelle ändert. Weil `kelvin` `private` ist, führt jeder Schreibzugriff zwangsläufig über `SetzeCelsius` — und dort wird geprüft. Ein physikalisch unmöglicher Wert kann so gar nicht erst in das Objekt gelangen; über den `bool`-Rückgabewert erfährt der Aufrufer, ob der Wert übernommen wurde.

Die Methoden `SetzeCelsius` und `GibCelsius` erfüllen ihren Zweck, wirken aber etwas umständlich: Statt `t.Celsius = 100` muss man `t.SetzeCelsius(100)` schreiben. Genau für dieses Muster — privates Feld plus kontrollierter Zugriff — bietet C# eine elegantere Syntax: [Properties](/modules/properties/properties.md), die wir als Nächstes kennenlernen.

Der Modifizierer `protected` spielt vor allem bei Vererbung eine Rolle: Eine `protected`-Methode oder ein `protected`-Feld ist für Code außerhalb der Klasse unsichtbar, kann aber von Unterklassen (abgeleiteten Klassen) genutzt werden. Das wird relevant, sobald wir Vererbung im Detail behandeln.
{: .notice--info}

Kapselung schützt also die internen Details einer Klasse. Daraus ergibt sich eine einfache Grundregel für den Alltag:

## Faustregel

Im Zweifel `private` — nur das sichtbar machen, was wirklich von außen gebraucht wird:

- **Felder immer `private`** — Zugriff über Methoden bzw. (ab dem nächsten Modul) Properties kontrollieren
- **Methoden `public`**, wenn sie von außen genutzt werden sollen
- **Hilfsmethoden `private`**, wenn sie nur intern gebraucht werden

```csharp
class Passwortmanager
{
    public bool Anmelden(string passwort)
        => HashPasswort(passwort) == gespeicherterHash;

    private string HashPasswort(string pw)  // Implementierungsdetail – verborgen
        => /* ... */ pw;

    private string gespeicherterHash = "...";
}
```

## Weitere Quellen

- [Zugriffsmodifizierer – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/programming-guide/classes-and-structs/access-modifiers)
- [Zugriffsebenen – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/keywords/accessibility-levels)
- [Encapsulation (OOP) – Wikipedia](https://de.wikipedia.org/wiki/Datenkapselung_(Programmierung))

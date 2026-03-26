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

Nicht alles in einer Klasse soll von außen zugänglich sein – interne Hilfsmethoden, Rohwerte und Implementierungsdetails bleiben besser verborgen. Zugriffsmodifizierer legen fest, wer auf was zugreifen darf, und sind das Werkzeug, mit dem man Kapselung in der Praxis umsetzt.

## Übersicht

| Modifizierer | Zugreifbar von |
| :--- | :--- |
| `public` | Überall |
| `private` | Nur innerhalb der Klasse |
| `protected` | Innerhalb der Klasse und Unterklassen |
| `internal` | Nur innerhalb des gleichen Projekts (Assembly) |

## Kapselung mit `private`

Die Tabelle zeigt die vier Stufen — in der Praxis ist `private` die mit Abstand wichtigste, denn sie setzt das zentrale OOP-Prinzip der Kapselung um.

Das Prinzip der **Kapselung** (*Encapsulation*) ist eines der Grundpfeiler der Objektorientierung: Interne Daten werden verborgen, der Zugriff läuft ausschließlich über kontrollierte Schnittstellen (Properties, Methoden). So kann die interne Darstellung geändert werden, ohne dass Code außerhalb der Klasse betroffen ist:

```csharp
class Temperatur
{
    private double kelvin;  // intern immer in Kelvin speichern

    public double Celsius
    {
        get => kelvin - 273.15;
        set
        {
            if (value < -273.15) throw new ArgumentException("Unter dem absoluten Nullpunkt.");
            kelvin = value + 273.15;
        }
    }

    public double Fahrenheit => Celsius * 9 / 5 + 32;
}

var t = new Temperatur();
t.Celsius = 100;
Console.WriteLine(t.Fahrenheit);  // 212
Console.WriteLine(t.Celsius);     // 100
// t.kelvin = -500; // Compilerfehler – private!
```

Im Beispiel speichert die Klasse die Temperatur intern in Kelvin, bietet aber nach außen `Celsius` und `Fahrenheit` an. Der Aufrufer merkt nichts von der internen Darstellung — und man könnte sie ändern (z.B. auf Celsius intern), ohne dass sich die Schnittstelle ändert.

Der Modifizierer `protected` spielt vor allem bei Vererbung eine Rolle: Eine `protected`-Methode oder ein `protected`-Feld ist für Code außerhalb der Klasse unsichtbar, kann aber von Unterklassen (abgeleiteten Klassen) genutzt werden. Das wird relevant, sobald wir Vererbung im Detail behandeln.
{: .notice--info}

Kapselung schützt also die internen Details einer Klasse. Daraus ergibt sich eine einfache Grundregel für den Alltag:

## Faustregel

Im Zweifel `private` — nur das sichtbar machen, was wirklich von außen gebraucht wird:

- **Felder immer `private`** — Zugriff über Properties kontrollieren
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

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

Das Prinzip der **Kapselung** (Encapsulation) schützt interne Daten vor unkontrolliertem Zugriff:

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

## Faustregel

- **Felder immer `private`** – Zugriff über Properties kontrollieren
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

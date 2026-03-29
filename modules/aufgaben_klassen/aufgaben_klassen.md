---
title: "🧩 Aufgaben und Beispiele: Klassen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Programmieren lernt man nicht nur durch Codezeilen tippen — sondern auch durch **Nachdenken**. Die folgenden Aufgaben trainieren *Computational Thinking*: die Fähigkeit, Probleme so zu strukturieren, dass ein Computer sie lösen kann. Dazu gehören Abstraktion, Zerlegung, Mustererkennung und Algorithmenentwurf. Nimm dir für jede Aufgabe Zeit, bevor du die Lösung aufklappst.

## Aufgabe 1 — Abstraktion

Modelliere einen Getränkeautomaten als Klasse(n). Der Automat hat eine Auswahl an Getränken mit Namen und Preisen, ein Guthaben (das der Benutzer einwirft), und kann Getränke ausgeben.

Skizziere auf Papier (oder im Kopf):
- Welche Klasse(n) brauchst du?
- Welche Properties hat jede Klasse?
- Welche Methoden hat jede Klasse? (Name, Parameter, Rückgabetyp)
- Was passiert, wenn das Guthaben nicht reicht?
- Was passiert, wenn ein Getränk ausverkauft ist?

<details markdown="1">
<summary>Lösung anzeigen</summary>

**Schritt 1 — Klassen identifizieren:**

Wir brauchen zwei Klassen: ein **Getränk** (Datencontainer für ein Produkt) und den **Automaten** selbst (die Maschine mit Logik).

**Schritt 2 — Klasse `Getraenk`:**

Ein Getränk hat einen Namen, einen Preis und einen Bestand (wie viele Flaschen noch da sind).

```csharp
class Getraenk
{
    public string Name { get; set; }
    public double Preis { get; set; }
    public int Bestand { get; set; }

    public Getraenk(string name, double preis, int bestand)
    {
        Name = name;
        Preis = preis;
        Bestand = bestand;
    }
}
```

**Schritt 3 — Klasse `Automat`:**

Der Automat verwaltet das Sortiment und das eingeworfene Guthaben. Er hat Methoden zum Geld-Einwerfen, Getränk-Wählen und Rückgeld-Ausgeben.

```csharp
class Automat
{
    public double Guthaben { get; private set; }
    private Getraenk[] sortiment;

    public Automat(Getraenk[] sortiment)
    {
        this.sortiment = sortiment;
        Guthaben = 0;
    }

    public void Einwerfen(double betrag)
    {
        if (betrag > 0)
        {
            Guthaben += betrag;
        }
    }

    public bool Waehlen(string name)
    {
        // Getränk im Sortiment suchen
        Getraenk gewaehlt = null;
        for (int i = 0; i < sortiment.Length; i++)
        {
            if (sortiment[i].Name == name)
            {
                gewaehlt = sortiment[i];
                break;
            }
        }

        if (gewaehlt == null)
        {
            Console.WriteLine("Getränk nicht gefunden.");
            return false;
        }

        if (gewaehlt.Bestand <= 0)
        {
            Console.WriteLine("Ausverkauft!");
            return false;
        }

        if (Guthaben < gewaehlt.Preis)
        {
            Console.WriteLine("Guthaben reicht nicht.");
            return false;
        }

        // Alles okay — Getränk ausgeben
        Guthaben -= gewaehlt.Preis;
        gewaehlt.Bestand--;
        Console.WriteLine($"{name} ausgegeben.");
        return true;
    }

    public double Rueckgeld()
    {
        double betrag = Guthaben;
        Guthaben = 0;
        return betrag;
    }
}
```

**Zentrale Designentscheidungen:**

- **Validierung gehört in die Methoden:** `Waehlen` prüft drei Fehlerfälle (nicht gefunden, ausverkauft, zu wenig Guthaben), bevor die eigentliche Aktion stattfindet. Das schützt den inneren Zustand des Objekts.
- **`Guthaben` hat `private set`:** Nur der Automat selbst darf das Guthaben ändern — von außen kann man nur über `Einwerfen` und `Rueckgeld` darauf zugreifen. Das ist *Kapselung*.
- **`Waehlen` gibt `bool` zurück:** So kann der Aufrufer erkennen, ob die Ausgabe geklappt hat, ohne eine Exception behandeln zu müssen.

</details>

## Aufgabe 2 — Zerlegung

Eine Klasse `Bankkonto` hat bereits die Methoden `Einzahlen(double betrag)` und `Abheben(double betrag)`. Jetzt soll `Ueberweisen(Bankkonto ziel, double betrag)` hinzukommen.

Zerlege `Ueberweisen` in Teilschritte:
1. Welche Prüfungen sind nötig?
2. Welche bestehenden Methoden kannst du wiederverwenden?
3. In welcher Reihenfolge müssen die Schritte passieren — und warum?

<details markdown="1">
<summary>Lösung anzeigen</summary>

**Schritt 1 — Teilschritte identifizieren:**

Eine Überweisung besteht aus:
1. **Prüfen**, ob der Betrag positiv ist
2. **Abheben** vom eigenen Konto
3. **Einzahlen** auf das Zielkonto

**Schritt 2 — Bestehende Methoden wiederverwenden:**

Wir haben bereits `Abheben` und `Einzahlen`. `Abheben` prüft intern, ob genug Geld auf dem Konto ist (z.B. durch eine Exception oder einen `bool`-Rückgabewert). Diese Logik müssen wir **nicht** duplizieren — wir rufen einfach `Abheben` auf.

**Schritt 3 — Reihenfolge ist entscheidend:**

```
❌ Falsche Reihenfolge:
   1. ziel.Einzahlen(betrag)   // Geld „erscheint" auf dem Zielkonto
   2. this.Abheben(betrag)     // Schlägt fehl → Geld wurde erschaffen!

✅ Richtige Reihenfolge:
   1. this.Abheben(betrag)     // Geld wird abgezogen (oder Fehler)
   2. ziel.Einzahlen(betrag)   // Erst jetzt kommt es beim Ziel an
```

Wenn man zuerst einzahlt und das Abheben dann fehlschlägt (z.B. wegen unzureichender Deckung), hätte man Geld aus dem Nichts erzeugt. Deshalb muss **zuerst** abgehoben werden.

**Schritt 4 — Implementierung:**

```csharp
class Bankkonto
{
    public double Kontostand { get; private set; }

    public void Einzahlen(double betrag)
    {
        if (betrag <= 0)
            throw new ArgumentException("Betrag muss positiv sein.");
        Kontostand += betrag;
    }

    public void Abheben(double betrag)
    {
        if (betrag <= 0)
            throw new ArgumentException("Betrag muss positiv sein.");
        if (betrag > Kontostand)
            throw new InvalidOperationException("Deckung reicht nicht.");
        Kontostand -= betrag;
    }

    public void Ueberweisen(Bankkonto ziel, double betrag)
    {
        // Schritt 1: Prüfung (implizit durch Abheben)
        // Schritt 2: Erst abheben — schlägt fehl, wenn Deckung fehlt
        this.Abheben(betrag);
        // Schritt 3: Dann einzahlen — nur wenn Abheben geklappt hat
        ziel.Einzahlen(betrag);
    }
}
```

Das Prinzip: `Ueberweisen` enthält fast keine eigene Logik — sie **orchestriert** nur die bestehenden Methoden in der richtigen Reihenfolge. Die Validierung steckt in `Abheben` und `Einzahlen` und muss nicht wiederholt werden.

</details>

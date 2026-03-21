---
title: "Das this-Schlüsselwort"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Innerhalb einer Klasse muss man manchmal explizit auf das eigene Objekt verweisen – zum Beispiel wenn ein Parameter denselben Namen trägt wie ein Feld. Das Schlüsselwort `this` ist genau diese Selbstreferenz und löst solche Mehrdeutigkeiten auf elegante Weise.

## Was ist `this`?

`this` ist eine Referenz auf das **aktuelle Objekt**, also die Instanz, auf der eine Methode gerade ausgeführt wird.

## Namenskollision auflösen

Der häufigste Anwendungsfall: wenn Parameter denselben Namen haben wie Felder:

```csharp
class Rechteck
{
    double breite;
    double höhe;

    public Rechteck(double breite, double höhe)
    {
        this.breite = breite;  // this.breite = Feld, breite = Parameter
        this.höhe = höhe;
    }
}
```

Ohne `this` würde `breite = breite` dem Parameter sich selbst zuweisen – das Feld bliebe unverändert.

## Konstruktor-Verkettung mit `this(...)`

Ein Konstruktor kann einen anderen Konstruktor derselben Klasse aufrufen:

```csharp
class Verbindung
{
    string host;
    int port;
    int timeout;

    public Verbindung(string host, int port, int timeout)
    {
        this.host = host;
        this.port = port;
        this.timeout = timeout;
    }

    // Kurzform: Standard-Timeout von 30 Sekunden
    public Verbindung(string host, int port) : this(host, port, 30) { }

    // Kurzform: Standard-Port und Standard-Timeout
    public Verbindung(string host) : this(host, 443) { }
}

var v = new Verbindung("api.htw-berlin.de");
// host = "api.htw-berlin.de", port = 443, timeout = 30
```

## Methode gibt sich selbst zurück (Fluent API)

```csharp
class QueryBuilder
{
    string tabelle = "";
    string bedingung = "";

    public QueryBuilder Von(string tabelle)  { this.tabelle = tabelle; return this; }
    public QueryBuilder Wo(string bed)       { bedingung = bed;        return this; }
    public string Build() => $"SELECT * FROM {tabelle} WHERE {bedingung}";
}

string query = new QueryBuilder()
    .Von("Studierende")
    .Wo("Semester > 3")
    .Build();
```

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
Der häufigste Anwendungsfall tritt auf wenn Parameter denselben Namen haben wie Felder:

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

Ohne `this` würde `breite = breite` dem Parameter sich selbst zuweisen — das Feld bliebe unverändert. `this.breite` sagt explizit: „Ich meine das Feld der Klasse, nicht den Parameter."

In vielen Fällen kann man die Namenskollision vermeiden, indem man Parameter und Felder unterschiedlich benennt (z.B. `_breite` für das Feld). In C# ist es aber üblich, `this` zu verwenden. Außerdem wird so klar ersichtlich, welche Variable ein Parameter ist und welche Variable zum Objektzustand gehört.
{: .notice--primary}

## Konstruktor-Verkettung mit `this(...)`

Wenn eine Klasse mehrere Konstruktoren hat, möchte man die Initialisierungslogik nicht duplizieren. Mit `: this(...)` kann ein Konstruktor einen anderen derselben Klasse aufrufen und ihm Argumente übergeben. So steht die eigentliche Logik nur einmal — im umfangreichsten Konstruktor:

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

Die Kette geht von unten nach oben: `Verbindung("api.htw-berlin.de")` ruft `Verbindung("api.htw-berlin.de", 443)` auf, der wiederum `Verbindung("api.htw-berlin.de", 443, 30)` aufruft. Das erinnert an optionale Parameter — tatsächlich sind Konstruktor-Verkettung und optionale Parameter oft austauschbar.

Neben der Konstruktor-Verkettung hat `this` noch eine weitere elegante Anwendung: Wenn eine Methode das aktuelle Objekt als Rückgabewert liefert, lassen sich Aufrufe zu lesbaren Ketten verbinden.

## Methode gibt sich selbst zurück (Fluent API)

Eine fortgeschrittene Anwendung von `this`: Wenn eine Methode `return this` zurückgibt, kann der Aufrufer mehrere Aufrufe hintereinander verketten. Dieses Muster heißt **Fluent API** und macht Code besonders lesbar:

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

## Weitere Quellen

- [this-Schlüsselwort – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/keywords/this)

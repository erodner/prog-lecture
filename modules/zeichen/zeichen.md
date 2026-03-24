---
title: "Zeichenoperationen – char"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Strings bestehen aus einzelnen Zeichen – und manchmal muss man genau auf dieser Ebene arbeiten, zum Beispiel um zu prüfen ob ein Buchstabe ein Vokal ist, ob eine Eingabe nur Ziffern enthält oder um ein Zeichen direkt in einen Zahlenwert umzuwandeln.

## `char` – ein einzelnes Zeichen

Während `string` eine ganze Zeichenkette darstellt, repräsentiert `char` genau **ein** Zeichen. Der wichtigste Unterschied in der Syntax: Strings stehen in doppelten Anführungszeichen (`"Hallo"`), einzelne Zeichen in einfachen (`'A'`). Ein `char` kann ein Buchstabe, eine Ziffer, ein Leerzeichen oder ein Sonderzeichen sein:

```csharp
char buchstabe = 'A';      // Einfache Anführungszeichen!
char ziffer    = '7';
char leerzeichen = ' ';
```

Ein häufiger Fehler: `"A"` (doppelte Anführungszeichen) ist ein `string` der Länge 1, kein `char`. Der Compiler meldet einen Fehler, wenn man einen String einer `char`-Variable zuweisen will.
{: .notice--warning}

## Zeichen und ihr numerischer Wert (ASCII/Unicode)

Jedes Zeichen hat intern einen Zahlenwert — den sogenannten Unicode-Codepunkt. Die Zuordnung von Zeichen zu Zahlen folgt dem Unicode-Standard, dessen erste 128 Einträge mit dem älteren ASCII-Standard identisch sind. Durch einen Cast kann man zwischen `char` und `int` hin- und herwechseln:

```csharp
char c = 'A';
int code = (int)c;
Console.WriteLine(code);          // 65

char zurueck = (char)66;
Console.WriteLine(zurueck);       // B
```

Das Alphabet ist im ASCII/Unicode-Standard in aufeinanderfolgenden Zahlen codiert: `'A'` = 65, `'Z'` = 90, `'a'` = 97, `'0'` = 48. Dass die Zeichen fortlaufend nummeriert sind, kann man sich zunutze machen — zum Beispiel lässt sich die Position eines Großbuchstabens im Alphabet mit `c - 'A'` berechnen, was für `'C'` den Wert `2` ergibt (da `67 - 65 = 2`).

## Unicode — mehr als ASCII

ASCII wurde in den 1960er Jahren entwickelt und deckt mit 128 Zeichen nur das englische Alphabet, Ziffern und einige Sonderzeichen ab. Für Umlaute (ä, ö, ü), Zeichen anderer Schriftsysteme (中, あ, العربية) oder Emojis reicht das nicht aus. Unicode löst dieses Problem: Der Standard definiert über 150.000 Zeichen aus praktisch allen Schriftsystemen der Welt.

In C# ist `char` ein 16-Bit-Wert und speichert ein Zeichen in der Kodierung **UTF-16**. Damit lassen sich die meisten Zeichen direkt darstellen — auch deutsche Umlaute oder Zeichen mit Akzenten:

```csharp
char umlaut = 'ü';              // UTF-16: 0x00FC
char euro = '€';                 // UTF-16: 0x20AC
Console.WriteLine((int)umlaut);  // 252
Console.WriteLine((int)euro);    // 8364
```

Manche Unicode-Zeichen (z.B. Emojis wie 😀) benötigen mehr als 16 Bit und werden in UTF-16 als **Surrogat-Paar** aus zwei `char`-Werten dargestellt. Deshalb kann `"😀".Length` den Wert `2` liefern, obwohl es visuell nur ein Zeichen ist. Für die meisten Alltagsanwendungen mit lateinischen Schriften spielt das keine Rolle, aber es ist gut zu wissen, dass `char` nicht immer einem sichtbaren Zeichen entspricht.
{: .notice--primary}

Man kann Unicode-Zeichen auch direkt über ihren Codepunkt angeben — mit der `\u`-Schreibweise:

```csharp
char omega = '\u03A9';           // Ω (griechischer Großbuchstabe Omega)
Console.WriteLine(omega);        // Ω
```

## Nützliche `char`-Methoden

Die `char`-Klasse bietet statische Methoden, um Zeichen zu klassifizieren und umzuwandeln. „Statisch" bedeutet hier, dass man die Methoden auf der Klasse `char` aufruft und das zu prüfende Zeichen als Argument übergibt — nicht auf dem Zeichen selbst:

```csharp
char c = 'A';

char.IsLetter(c)      // true  – ist ein Buchstabe?
char.IsDigit('5')     // true  – ist eine Ziffer?
char.IsWhiteSpace(' ')// true  – ist ein Leerzeichen?
char.IsUpper('A')     // true  – Großbuchstabe?
char.IsLower('a')     // true  – Kleinbuchstabe?
char.ToUpper('a')     // 'A'
char.ToLower('Z')     // 'z'
```

Diese Methoden sind besonders nützlich bei der Zeichenvalidierung — etwa um zu prüfen, ob eine Eingabe nur aus Buchstaben oder Ziffern besteht.

## Zeichen in Strings

Ein String ist intern eine Folge von `char`-Werten. Man kann auf einzelne Zeichen per **Index** zugreifen (beginnend bei 0) oder mit `foreach` über alle Zeichen iterieren:

```csharp
string wort = "Hallo";

// Einzelne Zeichen per Index:
char erster = wort[0];   // 'H'
char letzter = wort[4];  // 'o'

// Alle Zeichen iterieren:
foreach (char c in wort)
    Console.Write(c + " ");
// H a l l o
```

Der Index `[0]` liefert das erste Zeichen, `[wort.Length - 1]` das letzte. Greift man auf einen Index außerhalb des gültigen Bereichs zu (z.B. `wort[5]` bei einem 5-Zeichen-String), erhält man eine `IndexOutOfRangeException` — einen Laufzeitfehler, den wir bereits kennengelernt haben.

Übung: Zähle in einem eingelesenen Satz die Anzahl der Vokale (a, e, i, o, u – Groß und Klein).
{: .notice--info}

## Weitere Quellen

- [char-Typ – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/builtin-types/char)
- [Char-Klasse (Methoden) – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/api/system.char)
- [ASCII-Tabelle – asciitable.com](https://www.asciitable.com/) – Übersicht aller 128 ASCII-Zeichen mit Dezimal-, Hex- und Oktalwerten
- [Unicode-Zeichensuche – unicode.org](https://www.unicode.org/charts/) – offizielle Zeichentabellen nach Schriftsystem
- [UTF-16 in .NET – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/standard/base-types/character-encoding-introduction)

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

```csharp
char buchstabe = 'A';      // Einfache Anführungszeichen!
char ziffer    = '7';
char leerzeichen = ' ';
```

## Zeichen und ihr numerischer Wert (ASCII/Unicode)

Jedes Zeichen hat intern einen Zahlenwert (Unicode-Codepunkt):

```csharp
char c = 'A';
int code = (int)c;
Console.WriteLine(code);          // 65

char zurueck = (char)66;
Console.WriteLine(zurueck);       // B
```

Das Alphabet ist im ASCII/Unicode-Standard in aufeinanderfolgenden Zahlen codiert: `'A'` = 65, `'Z'` = 90, `'a'` = 97, `'0'` = 48.

## Nützliche `char`-Methoden

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

## Zeichen in Strings

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

Übung: Zähle in einem eingelesenen Satz die Anzahl der Vokale (a, e, i, o, u – Groß und Klein).
{: .notice--info}

## Weitere Quellen

- [char-Typ – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/csharp/language-reference/builtin-types/char)
- [Char-Klasse (Methoden) – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/api/system.char)

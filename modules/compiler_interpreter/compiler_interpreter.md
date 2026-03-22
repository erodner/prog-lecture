---
title: "Interpreter- vs. Compilersprachen"
layout: single
author_profile: true
author: Erik Rodner
licence: "CC-BY"
licence_desc: 2025 | HTW Berlin
toc: false
classes: wide
---

Programmiersprachen unterscheiden sich grundlegend darin, wie Quellcode in ausführbare Anweisungen umgewandelt wird.

## Interpretersprachen

Bei Interpretersprachen liest ein **Interpreter** den Quellcode zur Laufzeit und führt ihn direkt aus – Zeile für Zeile, ohne vorherige Übersetzung. Dies sei hier einmal anhand der Programmiersprache Python gezeigt:

```python
# hello.py
print("Hallo, Welt!")
```

```bash
$ python3 hello.py
Hallo, Welt!
```

Der Python-Interpreter (`python3`) liest `hello.py` und führt es sofort aus. Es entsteht keine separate ausführbare Datei.

Weitere Beispiele für Interpretersprachen sind: JavaScript, Ruby, PHP, Bash.

## Compilersprachen

Bei Compilersprachen übersetzt ein **Compiler** den gesamten Quellcode vorab in Maschinencode. Das Ergebnis ist eine ausführbare Datei, die direkt auf der Hardware läuft – ohne den ursprünglichen Quellcode. Wir zeigen dies einmal anhand der Programmiersprache C:

```c
// hello.c
#include <stdio.h>
int main() {
    printf("Hallo, Welt!\n");
    return 0;
}
```

Dieses Programm wird jetzt mit dem Compiler ``gcc`` in Maschinencode übersetzt und dann ausgeführt:

```bash
$ gcc hello.c -o hello   # Compiler erzeugt ausführbare Datei
$ ./hello                # Direktes Ausführen – kein Interpreter nötig
Hallo, Welt!
```

Die obigen Befehle zeigen das Vorgehen in einem Unix-Terminal, dies ist unter Windows ähnlich.
Im obigen Beispiel werden sogar zwei Schritte kombiniert: Kompilieren (Übersetzung in Maschinencode) und Linken (Zusammenführen von unterschiedlichen Teilen von Maschinencode).

Weitere Beispiele: C++, Rust, Go (erzeugen ebenfalls native Binaries).

## C# und .NET: Ein Mittelweg

C# verfolgt einen zweistufigen Ansatz:
Der Quellcode wird nicht direkt in Maschinencode übersetzt, sondern zunächst in eine plattformunabhängige Zwischensprache: die **CIL** (Common Intermediate Language), gespeichert in einer `.dll`-Datei.

Ein neues Projekt anlegen und ausführen:

```bash
$ dotnet new console -n hello   # Projektordner mit hello.csproj und Program.cs anlegen
$ cd hello
$ dotnet run                    # Kompilieren + Starten in einem Schritt
Hallo, Welt!
```

Was steckt in einem frischen Projekt? Genau zwei Dateien:

```
hello/
├── hello.csproj     ← Projektdatei (Zielframework, Einstellungen)
└── Program.cs       ← der eigentliche Quellcode
```

Nach `dotnet build` kommen zwei neue Verzeichnisse hinzu:

```
hello/
├── hello.csproj
├── Program.cs
├── obj/             ← Zwischenergebnisse des Compilers (temporär, nicht relevant)
│   └── Debug/net9.0/
│       ├── hello.dll              (CIL – wird noch nicht verwendet)
│       ├── hello.AssemblyInfo.cs  (automatisch generiert)
│       └── ...
└── bin/             ← fertiges Build-Ergebnis
    └── Debug/net9.0/
        ├── hello.dll              ← CIL-Assembly – das eigentliche Programm
        ├── hello                  ← nativer Starter: hello.exe (Windows) / hello (Unix)
        ├── hello.pdb              ← Debug-Symbole (Zeilennummern für Fehlermeldungen)
        └── hello.runtimeconfig.json
```

`obj/` ist ein Arbeitsordner für den Compiler – er merkt sich dort, welche Dateien sich geändert haben, damit nur das neu übersetzt werden muss, was nötig ist. `bin/` enthält das, was tatsächlich ausgeführt wird.

Der native Starter (`hello.exe` bzw. `hello`) ist eine plattformspezifische ausführbare Datei, die die .NET Runtime lädt und dann die `.dll` ausführt. Er lässt sich direkt starten – genau wie ein klassisches Programm:

```bash
# Unix/macOS:
$ ./bin/Debug/net9.0/hello
Hallo, Welt!
```

```bat
rem Windows:
> bin\Debug\net9.0\hello.exe
Hallo, Welt!
```

Alternativ kann die `.dll` direkt mit `dotnet` gestartet werden – plattformunabhängig, aber ohne den nativen Starter:

```bash
$ dotnet bin/Debug/net9.0/hello.dll
Hallo, Welt!
```

Der native Starter existiert, weil Benutzer ein Programm einfach per Doppelklick oder Konsolenaufruf starten wollen – ohne `dotnet` kennen zu müssen. Die eigentliche Logik steckt aber immer in der `.dll`.
{: .notice--primary}

Die **.NET Runtime** (CLR – Common Language Runtime) übersetzt die CIL beim Start per **JIT-Compiler** (Just-In-Time) in nativen Maschinencode für die aktuelle Plattform.

```
Quellcode (.cs)
      │
      ▼  dotnet build
   CIL (.dll)  →  bin/Debug/net9.0/hello.dll
      │
      ▼  JIT-Compiler (CLR zur Laufzeit)
Maschinencode
      │
      ▼
  Ausführung
```

`obj/` und `bin/` gehören nicht ins Versionskontrollsystem – sie werden von `.gitignore` ausgeschlossen und lassen sich jederzeit mit `dotnet clean` löschen und neu erzeugen.
{: .notice--primary}

## Vergleich auf einen Blick

| Eigenschaft | Interpreter (Python) | Compiler (C) | C# / .NET |
| :--- | :--- | :--- | :--- |
| Übersetzung | Keine vorab | Vollständig vorab | Zweistufig (CIL + JIT) |
| Portabilität | Hoch (Interpreter nötig) | Gering (plattformspezifisch) | Hoch (Runtime nötig) |
| Startgeschwindigkeit | Schnell | Sehr schnell | Schnell (JIT) |
| Ausführungsgeschwindigkeit | Langsamer | Sehr schnell | Sehr schnell |
| Fehler erkennbar | Erst zur Laufzeit | Bereits beim Kompilieren | Bereits beim Kompilieren |

Der letzte Punkt ist für Einsteiger besonders relevant: C# meldet viele Fehler **bereits beim Kompilieren**, bevor das Programm überhaupt gestartet wird.
{: .notice--primary}

## Weitere Quellen

- [.NET Runtime-Architektur – Microsoft Learn](https://learn.microsoft.com/de-de/dotnet/core/introduction)
- [Roslyn – der C#-Compiler als Open Source (GitHub)](https://github.com/dotnet/roslyn)

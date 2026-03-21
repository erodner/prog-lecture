# Programmierung 1 – HTW Berlin

Kurswebseite für **Programmierung 1 (C#)** an der HTW Berlin.

Gebaut mit [Jekyll](https://jekyllrb.com/) und dem [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/)-Theme (Contrast-Skin).

## Voraussetzungen

- Ruby (empfohlen via Homebrew)
- Bundler

## Lokale Entwicklung

```bash
bundle install
bundle exec jekyll serve
```

Die Seite ist dann unter `http://localhost:4000` erreichbar.

> **Hinweis für macOS mit Homebrew:** Falls beim Build Linker-Fehler auftreten (`ld: unsupported tapi file type`), den Xcode-Toolchain-Pfad vorschalten:
> ```bash
> export PATH="/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin:$PATH"
> export SDKROOT=$(xcrun --show-sdk-path)
> ```

## Struktur

```
lectures/       # Eine Datei pro Vorlesungseinheit (00–13)
modules/        # Einzelne Lehrmodule mit Inhalt und Codebeispielen
assets/         # Bilder und statische Dateien
_data/          # Navigation und Autorenprofile
```

## Lizenz

Inhalte stehen unter [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) – Erik Rodner, HTW Berlin.

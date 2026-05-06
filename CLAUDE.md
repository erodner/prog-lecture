# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Course website for **Programmierung 1 (C#)** at HTW Berlin. Built with Jekyll 4.2 + Minimal Mistakes theme (contrast skin), deployed via GitHub Pages. All content is in German.

## Build & Deploy

```bash
bundle install                    # Install dependencies
bundle exec jekyll serve          # Local dev server at http://localhost:4000
```

Deployment happens automatically via GitHub Actions on push to `main` (`.github/workflows/pages.yml`). To deploy: commit and `git push` — that's it.

macOS Homebrew users may need:
```bash
export PATH="/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin:$PATH"
export SDKROOT=$(xcrun --show-sdk-path)
```

## Content Architecture

**Two-level content model:**
- `lectures/NN/NN.md` — 14 lecture units (00–13), each a navigational hub linking to modules
- `modules/<name>/<name>.md` — 50+ standalone learning modules with explanations, C# code examples, and exercises

Lectures are the "table of contents"; modules contain the actual teaching content. Lectures reference modules via relative Markdown links like `[Title](/modules/name/name.md)`.

**Course progression:**
- Lectures 00–01: Setup, Hello World
- Lectures 02–04: Types (incl. enums), debugging, operators
- Lectures 05–06: Control flow (if/else, loops)
- Lectures 07–08: Strings, arrays, value/reference types, nullable
- Lecture 09: Methods, scope, overloading, recursion
- Lectures 10–11: OOP (classes, properties, constructors, instance/static methods, this, structs, namespaces, visibility)
- Lecture 12: Collections (List, Dictionary, Queue, Stack, HashSet)
- Lecture 13: Exception handling (try/catch, custom exceptions, design guidelines)

**Computational Thinking modules:**
- `modules/aufgaben_<topic>/aufgaben_<topic>.md` — Exercises with collapsible solutions for lectures 02, 04, 05, 06, 08, 09, 10, 12
- Linked in lecture tables with 🧩 emoji prefix
- Solutions use `<details markdown="1"><summary>Lösung anzeigen</summary>` (the `markdown="1"` is required for kramdown to render Markdown inside HTML)

## Content Conventions

- Front matter: `layout: single`, `author_profile: true`, `toc: false`, `classes: wide`
- Modules include `licence: "CC-BY"` and `licence_desc: 2025 | HTW Berlin`
- Exercises use `{: .notice--info}` blocks
- Important notes use `{: .notice--primary}`, warnings use `{: .notice--warning}`
- External references (Microsoft Learn, etc.) go in a `## Weitere Quellen` section at the bottom
- Code examples are C# with German variable/method names (PascalCase methods, camelCase parameters)
- Cross-module references should mention previously learned concepts by name
- Connecting prose between code blocks is expected — no "code dump" style
- No LINQ module (was removed intentionally)
- Enums belong in lecture 02 (types), not in the OOP section

## Writing Style Preferences

- Motivating intro paragraphs that explain *why* a concept matters
- Verbindungssätze (connecting sentences) between code examples — explain transitions
- Real-world analogies where helpful (Schließfächer for arrays, Supermarkt for Queue, etc.)
- Back-references to earlier modules when building on prior concepts
- "Im Zweifel..." guidance for students (e.g., "Im Zweifel class statt struct")
- The user (professor) dislikes: artificial overloading examples (no `bool` flags), summaries at end of responses, overly simple exercises

## Jekyll Configuration

- Markdown: kramdown with GFM input
- Relative links enabled for collections (`jekyll-relative-links` plugin)
- `modules/` and `lectures/` are explicitly included in the build
- `slides/` and `example/` are symlinks excluded from the build
- Collapsible boxes require `<details markdown="1">` (not plain `<details>`)

<div align="center">
  <img src="../../assets/logo.svg" width="96" alt="AI Trinity Logo" />

  # AI Trinity

  **Ein gutes Modell allein reicht nicht. Das hier macht Claude wirklich verlässlich — und so baust du es dir selbst.**

  [English](../../README.md) · 🌐 **Deutsch**

  [Problem](01-problem.md) · [These](02-thesis.md) · [Bau-Guide](03-build-guide.md) · [Windows-Realität](04-windows-reality.md) · [FAQ](05-faq.md)
</div>

---

## Kurzfassung

Nach Monaten täglicher, intensiver Nutzung hielt eine Erkenntnis stand: ein brillantes Modell für sich allein ist ein **brillanter Amnesie-Patient**. Das Modell ist notwendig, aber nicht hinreichend. Drei Dinge müssen **gleichzeitig** wahr sein, bevor der echte Quality-of-Life-Sprung passiert:

| Säule | Welches Problem sie löst | Referenz-Projekt |
|-------|--------------------------|------------------|
| **1. Ein nachweislich nicht dummes Modell** | Die Fehlerquote entscheidet, nicht Benchmarks auf dem Papier. Ein halluzinierendes Modell vergiftet alles Nachgelagerte. | [bullshit-benchmark](https://github.com/petergpt/bullshit-benchmark) |
| **2. Ein diszipliniertes Fundament / Harness** | Roher Chat hat keine Methode. Skills, Sub-Agenten, Hooks und Review-Gates machen aus einem Chatbot ein Werkzeug. | [ECC](https://github.com/affaan-m/ECC) |
| **3. Ein persistentes Gehirn** | Session-Handoff und Compaction sind **architektonisch verlustbehaftet** — Details werden still gedroppt. Ohne externes Gedächtnis startet jede Session bei null. | [MemPalace](https://github.com/MemPalace/mempalace) |

Fehlt auch nur eine Säule, degradiert das System leise. Dieses Repo ist die **Problemdarstellung + eine baubare Lösung** — kein Produkt zum Installieren, sondern eine Landkarte, die du reproduzieren kannst.

## Warum es das gibt

Es gibt viel selbstbewussten Lärm zum Thema „das Maximum aus Claude holen". Das meiste ist Bauchgefühl. Das hier ist das Gegenteil: eine konkrete, reproduzierbare Darstellung, was scheiterte, warum, und was es löste — mit **Vorher/Nachher-Beweis** in [`examples/`](../../examples/), nicht mit Behauptungen.

## Fang hier an

1. **[Das Problem](01-problem.md)** — warum ein gutes Modell allein dich mit einem Amnesie-Genie zurücklässt.
2. **[Die These](02-thesis.md)** — die drei Säulen und warum alle drei Pflicht sind.
3. **[Der Bau-Guide](03-build-guide.md)** — Schritt für Schritt, mit Verweis auf die drei kanonischen Projekte.
4. **[Windows-Realität](04-windows-reality.md)** — die ehrlichen Lücken, die kein anderer dokumentiert.

## Credits

Das ist das gelebte System eines einzelnen Operators, das auf drei unabhängigen Open-Source-Projekten steht. Voller Dank an deren Autoren — siehe [CREDITS](../../CREDITS.md). Dieses Repo steuert nur die **Synthese, den Bau-Pfad und die Feldnotizen** bei.

## Lizenz

Dokumentation unter [CC BY 4.0](../../LICENSE). Gepflegt von **KeilerHirsch**.

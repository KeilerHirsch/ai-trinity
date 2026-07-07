[English](../05-faq.md) · 🌐 **Deutsch**

[← Zurück zum README](README.md) · [Problem](01-problem.md) · [These](02-thesis.md) · [Bau-Guide](03-build-guide.md) · [Windows-Realität](04-windows-reality.md) · **FAQ** · [In der Praxis](06-in-the-wild.md)

---

# FAQ

**Ist das ein Produkt, das ich installiere?**
Nein. Es ist eine These plus ein Bau-Pfad. Die drei Säulen sind unabhängige Open-Source-Projekte; dieses Repo ist die Landkarte, die sie verbindet, plus die Feldnotizen aus dem Betrieb.

**Warum nicht einfach größere Context-Fenster nutzen?**
Ein größeres Fenster verzögert die Amnesie, es beseitigt sie nicht. Compaction passiert weiterhin, Details werden weiterhin gedroppt — nur später. Persistentes externes Gedächtnis ist ein anderer Mechanismus, kein größerer Puffer. Siehe [das Problem](01-problem.md).

**Reicht nicht „Handoff-Notizen / für nächstes Mal zusammenfassen"?**
Nein. Eine Zusammenfassung ist per Definition verlustbehaftet; ein Handoff ist die Zusammenfassung einer Zusammenfassung. Genau das Detail, das die letzte Session wertvoll machte, droppt ein Handoff. Du willst die Session *bewahrt und zurückgeladen*, nicht in einen Absatz komprimiert.

**Muss ich genau diese drei Projekte nutzen?**
Nein. Sie sind die Referenzen, die gut zusammenspielen, aber die These ist tool-agnostisch: Fehlerquote deines Modells screenen, ein diszipliniertes Harness drumherum, persistentes externes Gedächtnis dazu. Tausche Äquivalente ein, wenn du willst — lass nur keine Säule weg.

**Warum gilt die Modell-Fehlerquote als Säule eins?**
Weil Gedächtnis + Methode auf einem Modell, das selbstbewusst Fakten erfindet, dir ein dauerhaftes, gut organisiertes Archiv von Unsinn liefert. Die Fehlerquote ist das Fundament, auf dem die anderen zwei Säulen sitzen. Siehe [These, Säule 1](02-thesis.md).

**Wo ist der Beweis, dass es funktioniert?**
In [`examples/`](../../examples/): Vorher/Nachher-Transcripts — eine Session, die ohne Kontext startet, vs. derselbe Start mit geladenem persistentem Gehirn. Behauptungen sind billig; die Transcripts sind der Punkt.

**Funktioniert das nur auf Windows / nur mit Claude?**
Es wurde auf Windows mit Claude Code gebaut, aber die These ist allgemein. Windows-Nutzer sollten [Windows-Realität](04-windows-reality.md) für die plattformspezifischen Lücken und Workarounds lesen.

**Sind meine Daten sicher / verlässt etwas meine Maschine?**
Die persistente-Gehirn-Säule ist by-design ein *lokaler* Store — nichts an diesem Ansatz erfordert, dein Gedächtnis dem öffentlichen Internet auszusetzen. Wie du es betreibst, ist deine Sache; halte es lokal.

**Wer pflegt das?**
KeilerHirsch. Beiträge und Korrekturen willkommen via Issues/PRs.

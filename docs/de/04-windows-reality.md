[English](../04-windows-reality.md) · 🌐 **Deutsch**

[← Zurück zum README](README.md) · [Problem](01-problem.md) · [These](02-thesis.md) · [Bau-Guide](03-build-guide.md) · **Windows-Realität** · [FAQ](05-faq.md)

---

# Windows-Realität — die Lücken, die keiner dokumentiert

Das meiste Agent-Tooling wird auf macOS/Linux geschrieben und getestet. Auf Windows funktioniert es meistens — bis es still nicht mehr tut. Das sind echte Feldnotizen vom Bau der drei Säulen auf Windows 11. Nichts davon ist ein K.O.-Kriterium; für alles gibt es einen Workaround. Sie vorher zu kennen spart dir Tage.

## 1. „Continuous Learning" sammelt, destilliert aber nie

Manche Harnesses liefern einen Lern-Loop: Tool-Nutzung beobachten → wiederkehrende Muster zu wiederverwendbaren „Instincts" destillieren. Auf Windows **läuft die Sammel-Hälfte sauber, aber die Destillations-Hälfte kann by-design abgeschaltet sein** (ein Headless-Sub-Prozess, der nicht-interaktiv hängt). Symptom: Tausende Observations aufgezeichnet, **null** Instincts produziert — und es sieht kaputt aus, obwohl es das nicht ist.

**Workaround:** die Destillation manuell laufen lassen — den Observation-Log aggregieren, Muster mit 3+ Vorkommen rausziehen, die Instinct-Dateien selbst schreiben. Die Sammel-Daten sind alle da; du machst nur den Schritt, den der Daemon verweigert.

## 2. OAuth-Logins sind nicht agent-steuerbar

Jedes MCP/Tool, dessen Auth ein **Browser-OAuth-Consent** ist (Gmail, Kalender, etc.), kann der Agent nicht abschließen. Startest du den Auth-Flow aus der Agent-Shell, spawnt er eine abgekoppelte Konsole, die du nie siehst — sie endet „erfolgreich", schreibt aber nichts.

**Workaround:** den Auth-Befehl selbst in einem echten Terminal ausführen, den Browser-Consent durchklicken, dann den Agenten verifizieren lassen (mtime der Credential-Datei geändert + ein Live-Call). Nach Re-Auth den MCP-Server reconnecten (er cached den toten Token im Speicher).

## 3. `jq` und andere Unix-Annahmen

Viele Skill-Scripts setzen `jq`, `find -printf` oder andere GNU/Unix-Tools voraus. Auf einem Standard-Windows + Git-Bash sind die oft nicht da, und das Script stirbt mittendrin.

**Workaround:** das fehlende Tool installieren, oder den Schritt mit einem kleinen Python-/Node-Snippet machen. Wenn ein „deterministisches" Script auf Windows failt, erst eine fehlende Unix-Abhängigkeit vermuten, bevor du die Logik verdächtigst.

## 4. GPU-Embeddings laufen still auf der CPU

Installierst du das `[gpu]`-Extra eines Vektor-Stores, kannst du **beide** Runtime-Pakete (CPU und GPU) installiert haben — und das CPU-Wheel überschattet das GPU-eine. Resultat: es läuft, nur langsam, auf der CPU, während du denkst, es sei auf der GPU. Bei einem Memory-Backfill kann der CPU-Pfad zudem den RAM hochziehen, bis die Maschine OOMt.

**Workaround:** nach der GPU-Installation das CPU-Runtime-Paket explizit deinstallieren und das GPU-eine neu installieren; verifizieren, dass der GPU-Provider wirklich in der Provider-Liste auftaucht, bevor du ihm traust.

## 5. Git-Bash zerlegt native Windows-Befehle

`.exe`-Tools durch Git-Bash, zwei unabhängige Fallen: MSYS schreibt `/flag`-Argumente in falsche Windows-Pfade um (`taskkill /PID` → `…/Git/PID`), und Bash frisst `$`-Variablen in gequoteten `powershell -Command "..."`-Strings.

**Workaround:** für Prozess-/Dienst-/Registry-Arbeit PowerShell direkt aufrufen; für alles `$`- oder `/flag`-lastige eine Script-Datei schreiben und ausführen, statt mit Inline-Quoting zu kämpfen.

## 6. MCP-Server timeouten beim Boot (Contention, nicht Defekt)

Beim Session-Start feuern alle MCP-Server + Hooks + (auf Windows) logon-getriggerte Scheduled Tasks in derselben Sekunde. Langsam importierende Server sprengen das 30-Sekunden-Startfenster und werden „failed" markiert — obwohl gesund. Verräter: *mehrere unzusammenhängende Server failen gemeinsam*.

**Workaround:** das Startup-Timeout in der Env-Config deiner Runtime erhöhen und den Boot-Verstärker entfernen (keine schwere Arbeit beim Logon/Session-Start triggern). Beweise mit einer direkten Handshake-Probe, dass ein Server gesund ist, bevor du ihn für kaputt erklärst.

---

**Der Punkt:** Die drei Säulen funktionieren auf Windows absolut — dieses Repo wurde darauf gebaut. Aber der Feinschliff ist dünner als auf Unix, und die Fehler sind oft *still*. Behandle „es lief und produzierte nichts" als zu untersuchenden Bug, nicht als Erfolg.

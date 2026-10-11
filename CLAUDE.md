# CLAUDE.md – Arbeitsanweisungen für Agenten

Diese Datei lesen Coding-Agenten (Claude Code u. a.) zu Beginn jeder Sitzung. Andere Agenten finden denselben Inhalt in `AGENTS.md` (Symlink auf diese Datei).

## Worum es geht

Bomkeeper ist das Beweisarchiv für SBOMs und CRA-Nachweise: Original-SBOMs werden signiert, mit Zeitstempel versehen und in unveränderbarem Speicher (WORM) zehn Jahre lang aufbewahrt, mit Bezug zum ausgelieferten Produkt-Release.

**Bevor du Code schreibst, lies `docs/VISION.md`.** Dort steht, warum es Bomkeeper gibt, für wen, was es bewusst *nicht* ist und welche Leitprinzipien gelten. Fachbegriffe stehen in `docs/GLOSSARY.md`, getroffene Entscheidungen in `docs/adr/`.

## Die Regeln, die du nie brichst

Sie folgen aus den Leitprinzipien in `docs/VISION.md`. Bei Konflikt mit einem Ticket: anhalten und nachfragen, nicht umsetzen.

1. **Kein Löschen, kein Überschreiben archivierter Daten.** Keine Delete-/Update-Endpunkte für Bundles, keine Migration, die Bundles umschreibt, kein Testcode, der gegen echte WORM-Speicher löscht.
2. **SBOM byte-genau speichern.** Nicht parsen-und-neu-serialisieren, nicht formatieren, keine Zeilenenden ändern. Hash immer über die Original-Bytes.
3. **Speicher ist die Wahrheit.** Alles, was in der Datenbank steht, muss sich aus den Bundles rekonstruieren lassen (`reindex`). Keine fachlichen Daten nur in der Datenbank.
4. **Verifikation offline.** `bomkeeper verify` darf keinen Bomkeeper-Server und kein Netzwerk benötigen.
5. **Abwärtskompatibel lesen.** Jede Version liest alle früheren Bundle-Formate. Neue Felder sind optional; bestehende Felder ändern nie ihre Bedeutung.
6. **Kein Code ohne Lizenzklarheit.** Offener Teil: Apache-2.0. Keinen Code aus sbomify oder anderen Projekten übernehmen, ohne die Lizenz zu prüfen und es im PR zu nennen. Keine Abhängigkeit mit Copyleft-Lizenz (GPL/AGPL) im offenen Teil.
7. **Keine Compliance-Versprechen** in Code, UI-Texten oder Doku („CRA-konform“, „rechtssicher“). Formulierung: „unterstützt die Aufbewahrungsanforderungen des CRA“.
8. **Keine Secrets** in Code, Tests, Logs oder Beispielen.

## Offen vs. kommerziell

Jedes Issue trägt das Label `oss` oder `commercial`.

- `oss`: Format, CLI, Speicher-Adapter, Verifier, GitHub Action. Darf **nicht** von kommerziellem Code abhängen.
- `commercial`: Server, Index, UI, Dependency-Track-Anbindung, Freigaben, bomkeeper.cloud. Darf offenen Code nutzen.

Die Trennung muss so sauber sein, dass der offene Teil später ohne Umbau in ein eigenes öffentliches Repo verschoben werden kann.

## Wie du arbeitest

- **Ein Issue pro Branch und PR.** Branch-Name `<issue-nr>-<kurzer-titel>`. Im PR auf das Issue verweisen (`Closes #123`).
- **Akzeptanzkriterien sind der Vertrag.** Die Checkboxen im Issue müssen erfüllt und getestet sein. Ist ein Kriterium unklar oder widerspricht der Vision: nachfragen.
- **Spikes** (Label `spike`) enden mit einem ADR in `docs/adr/`, nicht mit Produktivcode.
- **Größere Entscheidungen** (neue Abhängigkeit, Formatänderung, neue Speicherart, Sicherheitsrelevantes) als ADR festhalten. Vorlage: `docs/adr/0000-template.md`.
- **Tests:** Jede Story bringt Tests mit. Speicher-Adapter werden gegen MinIO (S3 mit Object Lock) bzw. Azurite getestet, nie gegen produktive Konten.
- **Kleine Schritte:** Lieber drei kleine PRs als einen großen.

## Sprache

- Code, Kommentare, Commit-Messages, CLI-Ausgaben und öffentliche Doku: **Englisch** (internationales Open-Source-Projekt).
- Issues, Vision, ADRs und interne Planung: **Deutsch**.

## Projektstand

Aktueller Meilenstein: **M0 Fundament & Validierung** (bis 30.11.2026). Es gibt noch keinen Produktivcode. Lizenzmodell und Technik-Stack sind in ADR-0004 und ADR-0005 entschieden. Bis Produktcode existiert, keine Build- oder Testbefehle erfinden.

<!-- Sobald der Stack feststeht, hier ergänzen: Verzeichnisstruktur, Build-, Test- und Lint-Befehle. -->

# ADR-0003: Archivierte Daten werden nie geändert oder vorzeitig gelöscht

- **Status:** Akzeptiert
- **Datum:** 2026-10-07

## Kontext

Der Wert von Bomkeeper ist der Beweis, dass eine SBOM seit Auslieferung unverändert vorliegt. Jede Möglichkeit, Bundles zu ändern oder zu löschen – auch für Admins –, entwertet diesen Beweis.

## Entscheidung

- Es gibt keine API, keinen CLI-Befehl und keine UI-Funktion zum Ändern oder Löschen von Bundles.
- Speicher-Adapter verweigern das Schreiben, wenn der Zielspeicher nicht unveränderbar konfiguriert ist (Ausnahme: lokaler Test-Adapter, deutlich gekennzeichnet).
- Aufbewahrungsfristen werden nur verlängert, nie verkürzt.
- Die SBOM wird byte-genau gespeichert; der Hash wird über die Original-Bytes gebildet.

## Begründung

Unveränderbarkeit, die sich per Knopfdruck aufheben lässt, ist keine. Auditoren und Behörden müssen sich darauf verlassen können, dass auch der Betreiber nichts nachträglich anpassen konnte.

## Folgen

- Fehler werden durch ein neues, korrigierendes Bundle behoben (siehe ADR-0002).
- Kündigt ein Kunde von bomkeeper.cloud, werden seine Daten vollständig exportiert. Im gehosteten Speicher bleiben sie bis Fristende gesperrt; das muss in AGB und AVV klar stehen. Bei Bring-your-own-Storage liegen sie ohnehin beim Kunden.
- Testcode schreibt nie in produktive WORM-Speicher, weil sich dort nichts wieder entfernen lässt.

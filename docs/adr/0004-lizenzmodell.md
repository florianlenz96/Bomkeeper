# ADR-0004: Lizenzmodell

- **Status:** Akzeptiert
- **Datum:** 2026-10-11
- **Issue:** #4

## Kontext

Bomkeeper trennt einen offenen, unabhängig nutzbaren Beweiskern von
kommerziellen Betriebs- und Komfortfunktionen (siehe ADR-0001). Das
Lizenzmodell muss diese Trennung sichtbar machen und darf den offenen Teil
nicht von kommerziellem Code abhängig machen.

## Entscheidung

- Archivformat, CLI, Speicher-Adapter, Verifier und GitHub Action werden unter
  Apache-2.0 veröffentlicht.
- Server und kommerzielle Dienste werden unter FSL-1.1-MIT veröffentlicht.
  Für jede Version wird die in FSL-1.1-MIT vorgesehene MIT-Lizenz am zweiten
  Jahrestag dieser Version wirksam.
- Die Lizenz eines Teils wird im zugehörigen Lizenzdokument und an den
  entsprechenden Dateien ausgewiesen.
- Der FSL-1.1-MIT-Lizenztext verwendet die
  [offizielle Lizenzvorlage](https://github.com/getsentry/fsl.software/blob/main/FSL-1.1-MIT.template.md)
  mit ausgefülltem Lizenzgeber und Jahr.

## Begründung

Apache-2.0 ermöglicht die Nutzung des offenen Kerns ohne Abhängigkeit von
Bomkeeper. FSL-1.1-MIT erlaubt Quellcodeeinsicht und Weiterentwicklung für
zulässige Zwecke, begrenzt aber zunächst die konkurrierende Bereitstellung
kommerzieller Funktionen. Nach zwei Jahren gilt die MIT-Lizenz.

Eine proprietäre Lizenz wurde verworfen, weil sie die Prüfbarkeit und das
Vertrauen in die kommerziellen Teile einschränken würde.

## Folgen

- Der offene Teil bleibt ohne Abhängigkeit vom kommerziellen Teil nutzbar.
- Die Projektübersicht nennt die Lizenzgrenzen; Dateien des kommerziellen
  Teils müssen die zutreffende Lizenz kenntlich machen.
- Vor dem ersten kommerziellen Release sind die Lizenzdatei und die
  Lizenzzuordnung nochmals zu prüfen.

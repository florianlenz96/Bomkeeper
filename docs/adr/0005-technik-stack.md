# ADR-0005: Technik-Stack

- **Status:** Akzeptiert
- **Datum:** 2026-10-11
- **Issue:** #5

## Kontext

Der offene Kern benötigt eine CLI und Speicher-Adapter für S3-kompatible und
Azure-Speicher. Ein möglichst einfacher, einheitlicher Stack soll die
Entwicklung und langfristige Wartung des Archivs unterstützen.

## Entscheidung

- Go wird für die CLI als einzelnes verteilbares Binary, Speicher-Adapter und
  den kommerziellen Server verwendet.
- Go wird auch für weitere plattformübergreifende Kommandozeilenwerkzeuge
  eingesetzt, sofern keine begründete Ausnahme in einem ADR festgehalten wird.
- Konkrete SDKs und Bibliotheksversionen werden bei ihrer Einführung separat
  geprüft; der offene Teil erhält keine Abhängigkeiten mit GPL- oder
  AGPL-Lizenz.

## Begründung

Go eignet sich für verteilbare CLI-Binaries und hat SDKs für die geplanten
Speicheranbieter. Eine Sprache für CLI, Adapter und Server vermeidet einen
zweiten Toolchain- und Wartungsaufwand. Rust und TypeScript wurden als
Alternativen betrachtet; derzeit gibt es keinen projektspezifischen Bedarf,
der den zusätzlichen Stack rechtfertigt.

## Folgen

- Build-, Test- und Release-Automatisierung kann auf Go ausgerichtet werden,
  sobald Produktcode existiert.
- Die konkrete Go-Version wird im Build-Setup festgelegt und regelmäßig
  aktualisiert.
- Neue Anforderungen, die einen anderen Stack nahelegen, erfordern eine
  begründete ADR.

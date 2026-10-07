# Bomkeeper – Vision

> Bomkeeper ist das Beweisarchiv für Software-Stücklisten: unveränderbar, signiert, zehn Jahre lang – selbst gehostet oder als bomkeeper.cloud.

Dieses Dokument erklärt, **warum** es Bomkeeper gibt. Wer an Bomkeeper arbeitet – Mensch oder Agent – liest es zuerst. Bei jeder Abwägung gilt: Die Entscheidung, die dem hier beschriebenen Zweck besser dient, gewinnt.

## Das Problem

Hersteller erzeugen SBOMs heute meist in der CI/CD-Pipeline (Syft, Trivy, cdxgen). Dort sind sie Build-Artefakte, und Pipelines löschen Artefakte nach Tagen oder Wochen. Damit gehen drei Dinge verloren:

1. **Die SBOM selbst.** Nach ein paar Monaten ist sie weg.
2. **Der Beweis.** Niemand kann zeigen, dass eine SBOM unverändert ist und zu genau dem Produktstand gehört, der ausgeliefert wurde.
3. **Die Antwortfähigkeit.** Fragt ein Kunde, Auditor oder eine Behörde nach einem alten Stand, dauert die Antwort Tage – oder es gibt keine.

Analyse-Werkzeuge wie OWASP Dependency-Track lösen ein anderes Problem: Sie bewerten den *aktuellen* Stand und räumen alte Versionen bewusst auf (Dependency-Track 5 hat dafür eine konfigurierbare Retention). Für ein Zehn-Jahres-Archiv sind sie nicht gebaut.

## Warum jetzt

Der EU Cyber Resilience Act (Verordnung (EU) 2024/2847) macht die SBOM für Produkte mit digitalen Elementen zur Pflicht:

- **Seit 11.09.2026:** Meldepflichten für aktiv ausgenutzte Schwachstellen (Frühwarnung in 24 Stunden). Ohne zu wissen, welche ausgelieferten Stände betroffen sind, ist das kaum machbar.
- **Ab 11.12.2027:** volle Anwendung inkl. technischer Dokumentation mit maschinenlesbarer SBOM.
- **Aufbewahrung:** technische Dokumentation (inkl. SBOM) mindestens **10 Jahre** nach Inverkehrbringen oder für den Unterstützungszeitraum, je nachdem, was länger ist (Art. 13 CRA). Der Unterstützungszeitraum beträgt in der Regel mindestens 5 Jahre.
- **Marktüberwachung:** Behörden (in Deutschland das BSI) können die SBOM anfordern.

Der CRA ist der Anlass, nicht der einzige Grund. Bomkeeper hilft auch bei Kundenanforderungen in Ausschreibungen, bei Haftungsfragen (neue EU-Produkthaftungsrichtlinie erfasst Software), bei Due Diligence und bei jedem Wechsel von CI- oder Analyse-Werkzeugen.

## Für wen

**Primär:** Hersteller, deren Produkte lange beim Kunden laufen – Geräte, Maschinen, Embedded-, Industrie- und On-Premises-Software, oft deutscher Mittelstand mit kleinem Security-Team.

Typische Personen:

| Rolle | Will | Bomkeeper liefert |
| --- | --- | --- |
| Entwicklung / DevOps | Kein Zusatzaufwand | Ein Pipeline-Schritt, danach nichts mehr |
| Security | Schnelle Antwort bei neuer Schwachstelle | Suche über alle ausgelieferten Releases |
| Compliance / Geschäftsführung | Nachweis gegenüber Behörde und Auditor | Unveränderbares, prüfbares Archiv mit Produktbezug |
| Auditor / Behörde / Großkunde | Genau die angefragten Unterlagen | Zeitlich begrenzte, protokollierte Freigabe |

**Nicht primär:** reine SaaS-Anbieter, die nur eine aktuelle Version betreiben. Für sie ist der Nutzen gering.

## Was Bomkeeper ist – und was nicht

**Bomkeeper ist:**
- das Gedächtnis: Original-SBOMs byte-genau, signiert, mit Zeitstempel, in unveränderbarem Speicher (WORM)
- der Produktbezug: welche SBOM gehört zu welchem ausgelieferten Release, wann in Verkehr gebracht, wie lange unterstützt
- der Nachweis: jederzeit ohne Bomkeeper prüfbar

**Bomkeeper ist nicht:**
- ein SBOM-Generator (dafür gibt es Syft, Trivy, cdxgen)
- ein Schwachstellen-Scanner oder Analyse-Tool (dafür gibt es Dependency-Track; Bomkeeper speist es)
- ein Lizenz-Compliance-Tool
- ein öffentliches Trust Center (wie bei sbomify) – Bomkeeper teilt vorsichtig und protokolliert
- ein CRA-Compliance-Wizard – den Prozess liefert die Beratung

## Positionierung

- **Gegenüber Dependency-Track:** Ergänzung, keine Konkurrenz. „Dependency-Track für den Alltag, Bomkeeper für den Nachweis.“
- **Gegenüber sbomify / ReARM / CRANIS.Cloud:** Differenzierung über Beweisfähigkeit (Unveränderbarkeit, Signatur, Zeitstempel, Exit-Garantie) und Datenhoheit (Bring-your-own-Storage, EU-Hosting), nicht über Funktionsbreite.

## Geschäftsmodell

Open Core plus Beratung:

- **Offen (Apache-2.0):** Archivformat, CLI, Speicher-Adapter, Verifier. Alles, was ein Kunde braucht, um seine Nachweise ohne Bomkeeper zu lesen und zu prüfen.
- **Kommerziell:** Server mit Produktmodell, Index, UI, Dependency-Track-Anbindung, Freigaben, Audit-Log; gehostet als bomkeeper.cloud.
- **Beratung:** CRA-Readiness und Einführung. Bomkeeper ist auch Schaufenster und Türöffner dafür.

## Leitprinzipien

Diese Prinzipien sind nicht verhandelbar. Wenn ein Ticket ihnen widerspricht, ist das Ticket falsch.

1. **Nichts geht verloren.** Archivierte Daten werden nie überschrieben oder vor Fristende gelöscht – weder über API, UI, CLI noch Migration. Es gibt keinen Lösch-Endpunkt für Bundles.
2. **Original bleibt Original.** Die SBOM wird byte-genau gespeichert. Normalisieren, Umformatieren oder Anreichern passiert nur in abgeleiteten Daten (Index), nie im Bundle.
3. **Der Speicher ist die Wahrheit.** Datenbank und Index sind Cache und jederzeit aus den Bundles neu aufbaubar. Was nur in der Datenbank steht, gilt als nicht archiviert.
4. **Prüfbar ohne uns.** Jedes Bundle muss sich mit offenen Werkzeugen und ohne Netzwerkzugriff auf Bomkeeper verifizieren lassen – auch in zehn Jahren, auch wenn es uns nicht mehr gibt.
5. **Lesen, was wir je geschrieben haben.** Neue Versionen lesen alle alten Bundle-Formate. Formatänderungen sind additiv.
6. **Für Entwickler unsichtbar.** Archivieren ist ein Pipeline-Schritt. Wenn es mehr Aufwand ist, ist es falsch gebaut.
7. **Ehrlich in der Sprache.** Bomkeeper „unterstützt die Aufbewahrungsanforderungen des CRA“. Es macht niemanden „CRA-konform“. Keine Versprechen in UI, Doku oder Marketing, die wir nicht halten können.

## Woran wir Erfolg messen

- **M1:** Eine fremde Person archiviert in unter 10 Minuten eine signierte SBOM in einen gesperrten Bucket und prüft sie offline.
- **M2:** 3–5 Design-Partner archivieren produktiv; mindestens einer würde zahlen.
- **M3:** bomkeeper.cloud ist buchbar, rechtzeitig vor dem 11.12.2027.
- **Nebenbei:** Talks, Podcast-Folgen und Beratungsanfragen, die über Bomkeeper entstehen.

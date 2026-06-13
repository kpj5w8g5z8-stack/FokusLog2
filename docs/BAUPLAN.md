# FocusLog v2 – Bauplan & Session-Prompts für Claude Code

Ergänzt `docs/KONZEPT.md` (Anforderungen F/D/T/U gelten unverändert). Dieses Dokument legt die Reihenfolge fest und enthält pro Session den fertigen Prompt.

## 1. Finale Entscheidungen (vormals O-1…O-5)

|Nr |Entscheidung                                                                                                                                                                                                                                                                                                                        |
|---|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|O-1|Modulare Quellen in `src/`, Build per `build.js` über GitHub Actions. **Präzisierung O-1b:** `dist/` besteht aus wenigen Dateien (index.html, app.js, styles.css, sw.js, manifest, Icons) statt einer einzigen HTML – nötig für eine strenge CSP ohne `unsafe-inline` und für den Service Worker. Deployment bleibt vollautomatisch.|
|O-2|Check-in als **Kompaktkarte** (alle Kernmetriken auf einem Screen), kein Swipe-Flow.                                                                                                                                                                                                                                                |
|O-3|Kerntrio **Fokus · Stimmung · Energie**; Unruhe/Impulsivität/Hunger als zuschaltbares Modul unter „Mehr“.                                                                                                                                                                                                                           |
|O-4|Anker-Zeiten fest **9 / 13 / 17 / 21 Uhr** im MVP (Konstante in settings-Defaults). Einnahme-relative Anker = spätere Ausbaustufe, nicht jetzt bauen.                                                                                                                                                                               |
|O-5|Erinnerungen via **iOS-Kurzbefehle-Anleitung + Deep-Links** (`#checkin`, `#morgen`, `#abend`). Kein Web-Push, kein Server.                                                                                                                                                                                                          |

## 2. Sessionplan (je Session = 1 Claude-Code-Task = 1 PR)

### S0 · Grundgerüst (nur nötig, wenn das Repo außer CLAUDE.md und docs/ noch leer ist)

**Scope:** Scaffold exakt nach den Verträgen in CLAUDE.md erstellen: `build.js` (konkateniert src-Module in fester ORDER-Reihenfolge nach `dist/app.js`, ersetzt `__BUILD__`, kopiert index.html/styles.css/sw.js/manifest, dekodiert Icons aus `tools/icon-data.js`; fehlende Module nur warnen) · `.github/workflows/deploy.yml` (Node 20 → `node --test` → `node build.js` → Pages-Deploy von `dist/` via upload-pages-artifact + deploy-pages, permissions pages/id-token) · `src/`: index.html-Shell mit strikter CSP (`default-src 'none'; script-src 'self'; style-src 'self'; img-src 'self' data:; manifest-src 'self'; connect-src 'self'; base-uri 'none'; form-action 'none'`), manifest.webmanifest (standalone, Theme #0a0e1a, Icons 180/512), sw.js (Cache-first eigene Origin, Cache-Version `__BUILD__`), styles.css mit den v1-Design-Tokens (KONZEPT-Farbpalette), app.js-Boot (SW registrieren, `navigator.storage.persist()`), model.js mit **implementiertem** `localDateStr` (lokale Zeit!), `isoWeek` (ISO-8601) und `sleepHours` (über Mitternacht), db.js/store.js als UMD-Lite-Stubs · `tools/icon-data.js`: zwei PNG-Icons (180/512 px: dunkles abgerundetes Quadrat #0a0e1a, blaue Pulslinie #60a5fa, grüner Punkt #4ade80) mit Bordmitteln der VM erzeugen und als Base64-Konstanten ablegen · `test/model.test.js` (UTC-Mitternachtsfalle; ISO-KW-Grenzen: 30.12.2024→KW1/2025, 31.12.2026→KW53/2026; sleepHours-Grenzfälle) · README + .gitignore (dist/, node_modules/, *backup*.json, focuslog-*.json).
**DoD:** `node --test` und `node build.js` laufen grün; dist/ enthält index.html, app.js, styles.css, sw.js, manifest, beide Icons.

> **Prompt S0:** Lies zuerst CLAUDE.md und docs/BAUPLAN.md vollständig. Setze dann exakt Session S0 aus dem BAUPLAN um – nicht mehr. Führe am Ende `node --test` und `node build.js` aus; beides muss fehlerfrei sein.

### S1 · Datenfundament

**Scope:** `src/db.js` vollständig (open mit onupgradeneeded; Stores: meta/settings→keyPath `key`, days→`date`, checkins→`ts`, events→`id`; get/put/del/getAll mit Range; `exportAll()`/`importAll()`; `requestPersistence()`), Migrations-Mapper `migrateFromV1(v1Data, v1Settings)` in eigener Datei `src/migrate.js` (rein, testbar; Mapping s. KONZEPT §10: Stundenraster→Epoch-ts, Check-in-Events→Event-Records, sleepTags/schlafschlecht→`day.sleep`, healthFlags sport/depression→`day.context`, gelenke+painLocations→pain-Events, ibuprofenMg→ibuprofen-Events, Tagesfelder 1:1), Fixture-Generator `tools/fixture.js` (30 synthetische Tage, deterministischer Seed), Tests für db-Roundtrip (mit In-Memory-Fake oder direktem Mapper-Test), Migration (3 repräsentative v1-Tage als Inline-Fixture) und Export→Import-Identität.
**DoD:** `node --test` grün; `migrate.js` deckt alle v1-Felder aus KONZEPT §2.1 ab; keine UI-Änderung.

> **Prompt S1:** Lies CLAUDE.md, docs/BAUPLAN.md (Session S1) und docs/KONZEPT.md (§2.1, §8.3, §10). Implementiere Session S1 vollständig: db.js, migrate.js, tools/fixture.js und die zugehörigen Tests. Halte dich exakt an Schema und Mapping; ändere nichts außerhalb des S1-Scopes.

### S2 · App-Shell & Heute-Grundgerüst

**Scope:** `store.js` fertig (State, Aktionen, Pub/Sub), Hash-Router in `app.js` (#today/#checkin/#history/#stats + Deep-Links aus O-5), `ui/components.js` (el-Helper ohne innerHTML, Karte, Button, Toast, Undo-Snackbar, Bottom-Nav), `ui/today.js` v1: Tagesring (4 Anker), Primärbutton „Check-in jetzt“, Quick-Event-Leiste (Buttons vorhanden, Aktionen aus S3 dürfen stubben), kompakte Timeline-Anzeige der heutigen Einträge, Datumsleiste. Styles je View in `styles.css` ergänzen (Tokens nutzen). Boot: db öffnen → Migration falls v1-Daten in localStorage → heutigen Tag laden.
**DoD:** App lädt offline nach erstem Besuch (SW), installierbar, zeigt Heute-Screen mit echten Daten aus IndexedDB; Lighthouse-PWA-Kriterien sinngemäß erfüllt; Tests weiter grün.

> **Prompt S2:** Lies CLAUDE.md und BAUPLAN S2 (+ KONZEPT §6.2). Implementiere Session S2 vollständig. UI streng nach Regeln 2/5/8 in CLAUDE.md; Quick-Event-Logik nur als no-op-Stubs, die S3 füllt.

### S3 · Erfassung: Check-in & Quick-Events

**Scope:** `ui/checkin.js` (Kompaktkarte: 3 Kernmetriken mit Wortlabels & Farbskala, „Mehr“-Aufklapper für Modul-Metriken + Notiz + freies Ereignis; Zeit-Stepper fürs Rückdatieren; Entwurfs-Persistenz in settings/meta; Speichern→Toast+Haptik `navigator.vibrate?.(10)`), Quick-Events live: caffeine/food (1 Tap = zuletzt, Long-Press = Auswahl), ibuprofen (Dosiswahl 200–800), rebound (1 Tap), pain (Silhouette aus v1 als createElementNS-Port in `ui/components.js`), Einträge editier-/löschbar mit Undo.
**DoD:** Kern-Check-in in ≤3 Taps + Speichern; alle Events landen mit Epoch-ts in IndexedDB; kein Fokusverlust beim Tippen; Tests für Draft-Restore + Event-Erzeugung.

> **Prompt S3:** Lies CLAUDE.md und BAUPLAN S3 (+ KONZEPT §5 Ebene 1+2, §6.3). Implementiere Session S3 vollständig inkl. Silhouetten-Port ohne innerHTML.

### S4 · Tagesrahmen & Einstellungen

**Scope:** Morgenkarte (Schlaf vorbefüllt aus Vortag, Qualität+Subtags; Medikation „Wie gestern“-1-Tap / Pause / Detailformular mit Name/Hersteller/Dosis/Zeit/Baseline; Gewicht als Modul), Abendkarte (Kontext-/Stress-Tags vorausgewählt wie gestern, Tagesflags sport/depressiveThoughts/sick, Tagesnotiz), Sichtbarkeitslogik (morgens/abends, nachholbar über Datumsleiste), `ui/settings.js` als Vollseite: Modul-Profile „Einfach/Erweitert“ + Einzelmodule, Medikamenten-/Herstellerlisten, Stammdaten, Behandlungsstatus, Anker-Anzeige, RR-Serie-Schalter (blendet RR-Felder in Morgenkarte für X Tage ein).
**DoD:** KONZEPT §5.1-Mapping vollständig erfüllt (jede v1-Eingabe hat ihren neuen Ort); „Wie gestern“ schreibt korrekten day.med-Datensatz; Tests für Vorbefüllung & Modul-Sichtbarkeit.

> **Prompt S4:** Lies CLAUDE.md und BAUPLAN S4 (+ KONZEPT §5 Ebene 3+4, §6.5). Implementiere Session S4 vollständig.

### S5 · Auswertung

**Scope:** `stats.js` (reine Funktionen + zentrale Konstanten `MIN_N=14`, Wirkdauer 2–12 h; Trends, Stundenmittel, Koffein-Fenster 5 h, Schlaftag→Folgetag, Baseline-Vergleich, Med-Vergleich inkl. Hersteller-Dimension, Ereigniszählung, Ibuprofen je ISO-KW), `charts.js` (SVG via createElementNS: Tagesgraph mit Schlafschattierung/Einnahmemarker/Event-Icons – Logik aus v1 portieren, Trend-/Balken-/Stundengraph, Schmerz-Heatmap), `ui/trends.js` + `ui/patterns.js` + `ui/history.js` gemäß KONZEPT §6.4 (Bereiche Tag/Trends/Muster; History-Karten wie v1). Jedes Insight zeigt n.
**DoD:** Mit Fixture-Daten zeigen alle Graphen plausible Bilder; stats.js vollständig unit-getestet (deterministische Fixture-Erwartungswerte).

> **Prompt S5:** Lies CLAUDE.md und BAUPLAN S5 (+ KONZEPT §6.4, F-10…F-13). Implementiere Session S5 vollständig; nutze tools/fixture.js für Plausibilitätstests.

### S6 · Bericht, Backup, Onboarding, Migration live

**Scope:** `report.js` (Arztbericht-Text + Druck-Overlay wie v1 inkl. aller Kennzahlen, CSV de-DE Semikolon), `ui/reportview.js` (Bereich „Bericht“: Export JSON via Web Share API mit Download-Fallback, Import mit Validierung + v1-Erkennung→migrate.js, Backup-Status „zuletzt gesichert“, Erinnerung >7 Tage als Karte auf Heute), Onboarding 3 Screens (Idee · Zum-Home-Bildschirm-Anleitung · Kurzbefehle-Erinnerungen mit Deep-Links), optional D-06 verschlüsseltes Backup (WebCrypto AES-GCM) falls Zeit.
**DoD:** Druckreport optisch ≈ v1 (ohne Ibuprofen); Export→Import-Roundtrip identisch; v1-JSON importierbar; F-14/F-15/D-04 erfüllt.

> **Prompt S6:** Lies CLAUDE.md und BAUPLAN S6 (+ KONZEPT §7.3, §10, F-14/F-15). Implementiere Session S6 vollständig.

## 3. Nach jeder Session (deine 5 Minuten am iPhone)

1. PR in GitHub öffnen → Actions-Häkchen grün? → Diff überfliegen → **Merge**.
1. 1–2 Min warten, dann App-URL öffnen (nach S2: vom Home-Bildschirm) und die DoD-Punkte der Session antesten.
1. Probleme? Neue Claude-Code-Session auf dem PR-Branch bzw. Task: „Auf main: Bug X aus Session Sn fixen: …“.

## 4. Leitplanken für alle Sessions

- Eine Session = ein PR. Kein Scope-Creep; Ideen als TODO-Kommentar + Notiz in PR-Beschreibung.
- v1-Quellcode liegt bewusst **nicht** im Repo; fachliche Referenz ist KONZEPT §2. Wo „Port aus v1“ steht, neu implementieren nach Beschreibung, nicht raten.
- Bei Widerspruch zwischen Dokumenten gilt: CLAUDE.md > BAUPLAN.md > KONZEPT.md – und im Zweifel nachfragen statt annehmen.

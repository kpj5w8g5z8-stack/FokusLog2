# FocusLog v2 – Projektregeln für Claude Code

Lokales ADHS-/Wellness-Tagebuch als installierbare PWA für iOS Safari. Alle Nutzerdaten bleiben in IndexedDB auf dem Gerät; GitHub Pages liefert nur statischen Code. Fachkonzept: `docs/KONZEPT.md` · Arbeitsplan mit Sessions: `docs/BAUPLAN.md` – **vor jeder Aufgabe die zugehörige Session im BAUPLAN lesen.**

## Befehle

- Tests: `node --test`  (müssen vor jedem Commit grün sein)
- Build: `node build.js` → `dist/` (Deploy übernimmt GitHub Actions, nie `dist/` committen)
- Es gibt **kein** `package.json` und **keine** Dependencies – `npm install` ist immer falsch.

## Harte Regeln (nicht verhandelbar)

1. **Null Laufzeit-Dependencies.** Kein npm-Paket, kein CDN, keine Webfonts, keine externen Requests. Die CSP in `src/index.html` (`default-src 'none'` …) bleibt unangetastet.
1. **Kein `innerHTML` / `insertAdjacentHTML`.** DOM nur über `createElement`/`createElementNS` + `textContent` (XSS-Schutz, auch bei Backup-Import). SVG-Charts ebenso.
1. **Zeitregeln:** Kalendertage immer über `FL.model.localDateStr()` (lokale Zeit) – **niemals** `toISOString().slice(0,10)` (UTC-Falle nach Mitternacht). Kalenderwochen nur über `FL.model.isoWeek()`. Zeitstempel = Epoch-Millisekunden (Number).
1. **Speicherzugriff nur in `src/db.js`.** UI und Stats kennen kein IndexedDB/localStorage.
1. **Kein globales Re-Rendern.** Views abonnieren `FL.store` (Pub/Sub) und rendern nur sich selbst; Eingabefelder dürfen beim Tippen nie den Fokus verlieren. Keine `window.*`-Handler.
1. **Modulmuster (UMD-Lite),** damit Module sowohl in Node-Tests als auch konkateniert im Browser laufen:
   
   ```js
   (function (g) { "use strict";
     const api = { /* … */ };
     if (typeof module !== "undefined") module.exports = api;
     (g.FL = g.FL || {}).modulname = api;
   })(globalThis);
   ```
   
   Neue Datei ⇒ in `ORDER` in `build.js` eintragen (Abhängigkeiten zuerst).
1. **Statistik-Ehrlichkeit:** Korrelationen/Insights nur ab den zentralen Mindest-n aus `stats.js` (Standard n≥14), n immer mit ausgeben. Wirkdauer-Ausreißerfilter 2–12 h beibehalten.
1. **UI-Sprache Deutsch**, du-Form, keine Schuld-Sprache („keine Daten“, nie „verpasst“). Eine Primäraktion pro Screen. Skalen 1–5 mit metrik-eigenen Wortlabels, nie zweideutige Richtung.
1. **Datenschutz:** Niemals echte Nutzerdaten/Backups ins Repo (`.gitignore` beachten). Beispieldaten nur synthetisch über die Fixture. Ibuprofen erscheint nicht im Druckreport.
1. **iOS Safari ist die Zielplattform** (430 px max-width, safe-area-insets, Home-Screen-PWA). Web-APIs vorher auf iOS-Verfügbarkeit prüfen; im Zweifel Fallback.

## Arbeitsweise

- Pro Session genau den Scope aus `docs/BAUPLAN.md` umsetzen – nicht mehr. Definition of Done dort gilt.
- Erst Tests/Verträge in `test/` ergänzen, dann implementieren; `node --test` und `node build.js` lokal ausführen, bevor du die Arbeit abschließt.
- Bestehende Funktionen aus v1 (siehe KONZEPT Abschnitt 2) dürfen nie ersatzlos entfallen.
- Commits klein und thematisch; PR-Beschreibung: Was, Warum, wie getestet, offene Punkte.

# FocusLog v2 – Konzept für den Neuaufbau

**Analyse des Prototyps · ADHS-gerechtes Erfassungskonzept · Anforderungskatalog**
Stand: 12.06.2026 · Basis: index.html (FocusLog v1, ~1.540 Zeilen)

-----

## 1. Kernaussage vorab

Der Prototyp ist fachlich erstaunlich tief (Baseline-Tage, Pause-Tage, Hersteller-Tracking, Rebound-Fenster, n≥14-Schwellen) – das sind Konzepte, die viele kommerzielle ADHS-Apps nicht haben. **Das Problem ist nicht der Funktionsumfang, sondern wo die Eingabelast liegt.**

Die zentrale Erkenntnis aus v1: Eine App gegen Vergesslichkeit darf nicht selbst Disziplin voraussetzen. Der stündliche Check-in verlangt aktuell bis zu 6 Skalen + 4 Ereigniskategorien + Gesundheitsflags + Ibuprofen + Schmerzlokalisation + Schlaf + Notiz. Das ist ein Forschungsfragebogen, kein Moment-Check-in. Die EMA-Forschung (Ecological Momentary Assessment – genau die Methode, die du hier nachbaust) zeigt klar: Compliance bricht mit der Eingabelast pro Abfrage ein, nicht mit der Studiendauer.

**Designziel für v2:** *Der häufigste Vorgang dauert unter 10 Sekunden. Alles, was nicht in 10 Sekunden passt, wandert auf eine seltenere Ebene (täglich, situativ, wöchentlich) – ohne dass Daten verloren gehen.*

-----

## 2. Funktionsinventar v1 (damit nichts verloren geht)

Diese Liste ist die Vollständigkeits-Checkliste für v2. Jede Funktion bekommt in Abschnitt 5/6 einen neuen Platz – nichts wird ersatzlos gestrichen.

### 2.1 Erfassung

|Bereich            |Inhalt in v1                                                                                                                                          |Erfassungsort v1        |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
|Kernmetriken       |Fokus, Stimmung, Energie, Innere Unruhe (invertiert), Impulsivität (invertiert), Hunger – je 1–5                                                      |stündlicher Check-in    |
|Schlafqualität     |Gut/Schlecht + Subtags (Gedankenkarussell, Nachts wach, Alpträume)                                                                                    |stündlicher Check-in (!)|
|Ereignisse         |Positiv (10 Presets), Negativ (15 Presets inkl. Rebound/Crash), Koffein (6), Essen (8), eigene Einträge                                               |stündlicher Check-in    |
|Gesundheitsflags   |Verdauung, Gelenkschmerzen, Depressive Gedanken, Sport                                                                                                |stündlicher Check-in    |
|Ibuprofen          |0/200/400/600/800 mg                                                                                                                                  |stündlicher Check-in    |
|Schmerzlokalisation|11 Punkte auf SVG-Silhouette (bei Gelenke-Flag)                                                                                                       |stündlicher Check-in    |
|Notiz              |Freitext                                                                                                                                              |stündlicher Check-in    |
|Medikation         |Medikament (Liste), Hersteller (Liste), Dosis, Einnahmezeit, Pause-Tag, Baseline-Markierung                                                           |Tagesformular           |
|Schlafzeiten       |Aufgestanden / Schlafen gegangen + Dauerberechnung (über Mitternacht)                                                                                 |Tagesformular           |
|Vitalwerte         |Gewicht + Uhrzeit, Systole/Diastole/Puls + Uhrzeit                                                                                                    |Tagesformular           |
|Tageskontext       |Kontext-Tags (Arbeit, Urlaub, Familie, Freizeit), Stress-Tags (Viel Stress, Bildschirm überzogen, Routine eingehalten, Zu wenig getrunken), Krank-Flag|Tagesformular           |

### 2.2 Ansichten & Auswertung

- **Heute:** 14-Tage-Datumsleiste, 24h-Stunden-Timeline (Tap = Check-in), Schlaf-Zusammenfassungskarte, Eintragsliste mit Chips
- **Verlauf:** Tageskarten mit Ø Fokus, Stunden-Sparkline, Med-/Schlaf-/Stress-/Sport-Kurzinfo, Badges (Baseline/Pause)
- **Auswertung (8 Tabs):** Graph (Tagesverlauf mit Schlafschattierung + Einnahmelinie, 14-Tage-Trends mit Stress-/Med-Punkten, Schlaftrend, Ø Stundenverlauf), Körper (Gewicht, RR/Puls, Schlafzeiten; 7/30/365 Tage), Symptome (Ibuprofen mg/Woche, Gelenk-/Verdauungsfrequenz, Schmerz-Detailgraphen je Körperstelle, Heatmap-Silhouette, Flag-Summen), Schlüsse (beste Wirkzeit, Rebound-Quote, Sport↔Rebound, Stress↔Unruhe, Schlaftags↔Folgetag-Fokus, Koffein-Effekt mit 5h-Fenster, 14-Tage-Trend, Baseline-Vergleich), Vergleich (Medikamente Tag/Woche/Monat), Ereignisse (Häufigkeiten), Backup, Einstellungen
- **Berichte/Export:** Arztbericht als Text (mit r-Werten, n-Angaben, Wirkdauer 2–12h-Filter, Rebound-Fenster, letzte 3 Tage), Druck-/PDF-Report (A4, Tabellen, ohne Ibuprofen), CSV (Semikolon, deutsch), JSON-Backup + Import, Mail, Zwischenablage
- **Einstellungen:** Stammdaten (Größe/Gewicht), Therapie-/Medikations-Checkboxen, Therapie-Modus (Hersteller an/aus), Medikamenten- & Herstellerlisten, Behandlungsstatus (Standard/Unbehandelt/Ohne Meds → Baseline-Logik)

### 2.3 Fachlich wertvolle Konzepte, die unbedingt erhalten bleiben

1. **Baseline-Kontrollgruppe** (medikationsfreie Tage als Vergleich) – sauberes n-of-1-Design
1. **Pause-Tage** explizit getrennt von „vergessen einzutragen”
1. **Hersteller-Tracking** – Generika-Wechsel sind bei MPH/Lisdexamfetamin real spürbar, das erfasst kaum jemand
1. **Wirkdauer-Schätzung** über Rebound-Ereignisse mit Ausreißerfilter (2–12h)
1. **Mindest-n für Korrelationen** (n≥14) statt Scheinpräzision
1. **Datenschutz-Detail:** Ibuprofen nicht im Druckreport

-----

## 3. Learnings aus v1 – was beim Neuaufbau anders sein muss

### L1 · Eingabelast falsch verteilt (wichtigstes Learning)

Schlafqualität und Schlaf-Subtags sind **Tageseigenschaften**, werden aber in jedem stündlichen Check-in abgefragt und gespeichert (pro Check-in im Datenmodell, in der Auswertung aber als Tagesmerkmal interpretiert). Depressive Gedanken, Sport, Verdauung sind ebenfalls eher Tages- oder Ereignis-, keine Stundenmerkmale. Folge in v1: Der Check-in-Screen ist überladen, und das Datenmodell ist semantisch inkonsistent. → v2: striktes Ebenenmodell (Abschnitt 5).

### L2 · „Tagesformular” ist eine Sammelschublade

Medikation + Schlaf + Gewicht + Blutdruck + Kontext + Krank + Baseline in einem Modal mit 7 Anliegen. Für den Standardfall („gleiche Medikation wie gestern”) sind das viel zu viele sichtbare Felder. → v2: Aufteilen in Morgenkarte (Schlaf + Medikament als 1-Tap-Wiederholung) und Abendkarte (Kontext/Stress/Flags); Vitalwerte werden situative Quick-Aktionen.

### L3 · Architektur skaliert nicht mehr

1.540 Zeilen in einer Datei ohne Trennung von Datenzugriff, Statistik und UI. Jede Änderung rendert die komplette App neu (`set()` → `render()`), Eingabefelder müssen deshalb als unkontrollierte DOM-Inseln (IIFE-erzeugte Inputs) am Re-Render vorbeigeschleust werden – zwei Paradigmen gemischt, Fokusverlust-Bugs vorprogrammiert. SVG kommt per `innerHTML`, der Schmerz-Klick läuft über ein globales `window._painClick`. → v2: modulare Quellstruktur, gezieltes Rendern, Event-Delegation statt Globals (Abschnitt 8).

### L4 · Konkrete Bugs, die v2 per Design ausschließt

- **Zeitzonenfehler:** `todayStr()` nutzt `toISOString()` = UTC. Zwischen 00:00 und 01:00 (Winter) bzw. 02:00 (Sommer) deutscher Zeit landen Einträge am **Vortag**. Genau die Uhrzeit, zu der ADHSler noch wach sind. → v2: lokale Datumsfunktionen, niemals UTC für Kalendertage.
- `nowHour()` = `getHours()||24` mappt Mitternacht auf Stunde 24 des laufenden Tages – zusammen mit dem UTC-Bug doppelt verschoben.
- **Kalenderwochen falsch:** `Math.ceil(dt.getDate()/7)` ist keine KW (Ibuprofen-Wochenstatistik, Med-Vergleich „Woche”). → v2: ISO-8601-Wochenberechnung.
- **Stunden-Schlüssel statt Zeitstempel:** Einnahme „8:30” wird zu Stunde 8 gerundet, zwei Check-ins in einer Stunde überschreiben sich. → v2: Zeitstempel als Primärschlüssel.
- **dayCtx-Merge fragil:** Tags werden per Spread zusammengeführt, Initialisierung nur „wenn leer” – Abwahl/Re-Edit-Zustände sind schwer nachvollziehbar. → v2: ein Tag-Objekt als Single Source of Truth, explizite true/false-Werte.
- **Insights ohne Mindest-n:** Einige Schlüsse feuern ab n=2–3 (Sport↔Rebound ab 2 Tagen, Schlaftags ab 2 Werten), während der Report sauber n≥14 verlangt. → v2: einheitliche, zentrale n-Schwellen + n-Anzeige an jedem Insight.
- **Hardcodierte Defaults:** „Elvanse” steht zweimal fest im Code. → v2: Defaults nur aus Einstellungen.

### L5 · Speicher ist der größte Einzelrisikofaktor

localStorage ist synchron, auf ~5 MB begrenzt und auf iOS am fragilsten Ende der Speicher-Hierarchie. Die App ist außerdem keine echte PWA (kein Service Worker, kein Manifest) – sie lebt davon, dass Safari die Seite gecacht hat. Backup ist rein manuell. Für eine App, deren Wert mit jedem Tag Datenhistorie steigt, ist das die gefährlichste Stelle. → v2: IndexedDB + `navigator.storage.persist()` + installierbare PWA + automatische Backup-Erinnerung (Abschnitt 7).

### L6 · Die App erinnert nicht – sie wartet

v1 setzt voraus, dass du daran denkst, sie zu öffnen. Das widerspricht dem Kernproblem (Vergesslichkeit). → v2: Erinnerungsstrategie als eigenes Anforderungspaket (Abschnitt 7.4), ehrlich mit den iOS-Grenzen.

### L7 · UI-Inkonsistenzen mit Kognitionskosten

- Skalenrichtung wechselt: bei Unruhe/Impulsivität ist 1 gut, sonst 5 – jedes Mal Umdenken nötig. Die Beschriftungen („Sehr wenig”…„Sehr stark”) passen zudem nicht auf alle Metriken (Stimmung „sehr stark”?).
- 8 Auswertungs-Tabs, horizontal scrollend – Tabs außerhalb des Sichtfelds existieren für ADHS-Nutzer faktisch nicht.
- Einstellungen doppelt (Modal **und** Stats-Tab) mit teils identischen, teils abweichenden Feldern.
- 24 Stundenkreise als primärer Einstieg: „Welche Stunde ist jetzt? Wo tippe ich?” ist ein Suchproblem vor jeder Eingabe.

-----

## 4. Was die Forschung sagt: ADHS, Selbsterfassung & UI

### 4.1 Erfassungsfrequenz (EMA-Forschung)

Deine App ist methodisch ein EMA-Tagebuch. Relevante Befunde aus Studien und Meta-Analysen:

- In EMA-Studien liegt der Median bei **~5 Abfragen pro Tag** – nicht 24. Studien mit nur **einer täglichen Abfrage erreichen ~91 % Compliance, häufigere Abfragen nur ~77 %**. Die Studienlänge beeinflusst die Compliance kaum, die Abfragefrequenz und -last dagegen deutlich.
- Junge Erwachsene mit ADHS schaffen in Studien 85 %+ Antwortquoten – **aber mit Vergütung und Studienrahmen**. Ohne externe Anreize fällt die Quote; die App muss den Anreiz also selbst liefern (sofortiges Feedback, sichtbarer Fortschritt, minimale Reibung).
- Zwei Sampling-Strategien sind etabliert: **zeitbasiert** (feste/zufällige Abfragezeitpunkte) und **ereignisbasiert** (Nutzer loggt, wenn etwas passiert). Für Konsum (Koffein, Essen, Ibuprofen) und Symptome (Rebound, Schmerz) ist ereignisbasiert die korrekte Methode – nicht das stündliche Raster.

**Übersetzung für FocusLog:** 3–5 strukturierte Mini-Check-ins pro Tag + jederzeit mögliche 1-Tap-Ereignisse liefern statistisch bessere Daten als 24 theoretische Stundenslots, von denen real 3–6 gefüllt werden – und zwar mit systematisch verzerrter Auswahl (man trägt ein, wenn es einem auffällt → Negativ-Bias).

### 4.2 ADHS-gerechte UI (kognitive Barrierefreiheit)

Konsens über Quellen hinweg (Neurodivergent-UX-Leitfäden, WCAG-2.2-Kognitionskriterien):

1. **Hick’s Law ernst nehmen:** Je mehr sichtbare Optionen, desto schwerer die Entscheidung – Optionen pro Screen radikal begrenzen, Rest hinter „Mehr”.
1. **Eine Primäraktion pro Screen,** hoher Kontrast, groß, immer an derselben Stelle.
1. **Vorhersehbare, identische Navigation** auf jeder Ansicht; nichts springt, nichts scrollt sich weg.
1. **Vergebende Flows:** Abbrechen ohne Datenverlust, Entwürfe bleiben erhalten, alles editierbar, Undo statt „Löschen?”-Dialog.
1. **Gedächtnis entlasten:** Die App merkt sich Zustände („wie gestern”), statt Wiedereingabe zu verlangen.
1. **Sofortige Belohnung:** direktes visuelles Feedback nach jedem Eintrag (Haken, Streak-Punkt, Tagesring füllt sich) – Dopamin-gerechtes Design ohne Druckmechaniken (keine Countdowns, keine „verpasst!”-Schuld).
1. **Ruhige Optik:** keine konkurrierenden Animationen, klare Hierarchie, viel Weißraum (bzw. Dunkelraum). Das dunkle, ruhige Design von v1 ist hier schon richtig.

### 4.3 Konsequenz: das 2-Schichten-Prinzip

Jede Information bekommt zwei Wege: **Schnellpfad** (Standardwert, 1 Tap, Default „wie zuletzt”) und **Detailpfad** (aufklappbar, optional). v1 zeigt immer den Detailpfad. v2 zeigt immer den Schnellpfad und macht Details erreichbar – Funktionsumfang bleibt identisch, sichtbare Komplexität sinkt um ~70 %.

-----

## 5. Neues Erfassungskonzept: das 4-Ebenen-Modell

Deine Kernfrage „Was brauche ich wirklich stündlich, was nur einmal am Tag?” – hier die Antwort als System:

### Ebene 1 · Moment-Check-in (mehrmals täglich, situativ · Ziel: ≤ 10 Sekunden)

**Nur:** Fokus · Stimmung · Energie (je 1–5, ein Tap pro Metrik, große Buttons).
Das sind die drei Metriken, an denen Medikamentenwirkung, Tagesform und Rebound ablesbar sind – und genau die drei, die v1 in Vergleich/Report ohnehin als Kerntrio auswertet.

- **Optional aufklappbar („Mehr”):** Innere Unruhe, Impulsivität, Hunger – als abschaltbares Modul in den Einstellungen (Standard: aus im Einfach-Modus, an im Erweitert-Modus).
- **Skalenrichtung vereinheitlicht:** intern wird alles als „höher = belastender/stärker ausgeprägt” ODER pro Metrik mit eigener Wortskala gespeichert; im UI bekommt jede Stufe ein passendes Wortlabel je Metrik (Fokus: „zerstreut … glasklar”; Unruhe: „ruhig … getrieben”). Nie wieder „ist 5 hier gut oder schlecht?”.
- **Zeitstempel statt Stundenslot:** Check-in gilt für „jetzt” (minutengenau), rückdatierbar über einen kleinen Zeit-Stepper. Mehrere Check-ins pro Stunde möglich.
- **Empfohlener Rhythmus statt Pflichtraster:** Die App schlägt 4 Anker vor (nach dem Aufstehen, Vormittag, Nachmittag/Wirkende, Abend) und zeigt sie als füllbaren Tagesring. 24h-Vollständigkeit ist explizit kein Ziel mehr.

### Ebene 2 · Quick-Events (jederzeit, 1–2 Taps, ereignisbasiert)

Dauerhaft sichtbare Leiste auf dem Heute-Screen – **ohne** einen Check-in zu öffnen:

|Button         |Verhalten                                                                                  |
|---------------|-------------------------------------------------------------------------------------------|
|☕ Koffein      |1 Tap = Standardgetränk (zuletzt genutztes), langer Druck = Auswahl (Kaffee, Matcha, …)    |
|🍽️ Essen        |1 Tap = Mahlzeit jetzt, langer Druck = Typ (Frühstück…Süßes)                               |
|💊 Ibuprofen    |1 Tap = letzte Dosis wiederholen, langer Druck = 200/400/600/800                           |
|⚠️ Crash/Rebound|1 Tap = Rebound-Ereignis jetzt (das wichtigste Einzelereignis für die Wirkdauer-Statistik!)|
|🦵 Schmerz      |öffnet direkt die Körpersilhouette, Tap auf Stelle, fertig                                 |
|＋              |freies Ereignis (Positiv/Negativ-Presets aus v1 + Freitext)                                |

Jedes Quick-Event speichert einen Zeitstempel – dadurch werden Koffein-Fenster (5h) und Rebound-Zeitpunkte präziser als im Stundenraster von v1.

### Ebene 3 · Tagesrahmen (2 kurze Karten, je ≤ 30 Sekunden)

**Morgenkarte** (erscheint beim ersten Öffnen des Tages, bis ~12 Uhr):

1. Schlaf: Zeiten (vorbefüllt mit gestern ± Anpassung), Qualität Gut/Schlecht, bei Schlecht: Subtags (Gedankenkarussell, Nachts wach, Alpträume). *Schlaf verschwindet komplett aus dem Moment-Check-in.*
1. Medikament: **ein Tap „Wie gestern: Elvanse 30 mg, 8:00”** (Name/Dosis/Hersteller/Zeit übernommen, Zeit auf jetzt korrigierbar) · daneben „Pause” · „Ändern” öffnet das Detailformular (Medikament, Hersteller, Dosis, Zeit, Baseline). Der 90-%-Fall wird damit 1 Tap statt 5 Felder.
1. Gewicht: optionales Feld, nur sichtbar wenn Modul aktiv.

**Abendkarte** (erscheint ab ~18 Uhr, optional):

1. Tageskontext: Kontext-Tags (Arbeit/Urlaub/Familie/Freizeit) + Stress-Tags – vorausgewählt wie gestern, nur Abweichungen antippen.
1. Tagesflags: Sport gemacht? Depressive Gedanken heute? Krank? (wandern aus dem Stunden-Check-in hierher – das sind Tagesurteile).
1. Tagesnotiz (Freitext).

Beide Karten sind überspringbar und jederzeit nachholbar (auch für vergangene Tage über den Verlauf).

### Ebene 4 · Selten & auf Anlass

- **Blutdruck/Puls:** Quick-Aktion im ＋-Menü oder über die Morgenkarte einblendbar – nicht täglich erzwungen. Wer eine Messreihe macht (z. B. nach Dosisänderung), aktiviert „RR-Serie: 7 Tage” und bekommt die Felder solange morgens angezeigt.
- **Stammdaten** (Größe, Therapie-Status, Listenpflege): Einstellungen.
- **Backup:** wöchentliche Erinnerung mit 1-Tap-Export (Abschnitt 7.3).
- *(Optionale Idee für später: monatlicher Mini-Selbstcheck mit 5–6 festen Fragen als Langzeit-Anker – bewusst nicht Teil des MVP.)*

### 5.1 Mapping v1 → v2 (Vollständigkeitsnachweis)

|v1-Funktion                                |Neuer Ort                                                       |
|-------------------------------------------|----------------------------------------------------------------|
|6 Metriken stündlich                       |3 im Moment-Check-in, 3 als optionales Modul                    |
|Schlafqualität + Subtags je Stunde         |Morgenkarte (1× täglich)                                        |
|Schlafzeiten im Tagesformular              |Morgenkarte                                                     |
|Medikation im Tagesformular                |Morgenkarte („Wie gestern” / Pause / Ändern)                    |
|Ereignisse (pos/neg/Koffein/Essen)         |Quick-Events + ＋-Menü, zeitstempelgenau                         |
|Ibuprofen je Stunde                        |Quick-Event                                                     |
|Gelenke-Flag + Schmerzlokalisation         |Quick-Event „Schmerz” → Silhouette                              |
|Verdauung                                  |Quick-Event im ＋-Menü                                           |
|Depressive Gedanken, Sport                 |Abendkarte (Tagesflags)                                         |
|Kontext-/Stress-Tags                       |Abendkarte (vorausgewählt wie gestern)                          |
|Krank-Flag                                 |Abendkarte                                                      |
|Gewicht, Blutdruck                         |Morgenkarte (Modul) bzw. RR-Serie / ＋-Menü                      |
|Notiz je Stunde                            |bleibt im Check-in als optionales Feld **und** Tagesnotiz abends|
|Baseline/Pause/Hersteller/Behandlungsstatus|unverändert, im Medikations-Detail bzw. Einstellungen           |

-----

## 6. Informationsarchitektur & UI-Struktur v2

### 6.1 Navigation (unverändert 3 Tabs – aber neu gefüllt)

**Heute · Verlauf · Auswertung** bleiben. Einstellungen werden eine eigene, von oben rechts erreichbare Vollseite – und existieren nur noch **einmal** (der doppelte Settings-Tab in der Auswertung entfällt).

### 6.2 Heute-Screen (Aktionszentrale statt Stundensuche)

Aufbau von oben nach unten:

1. **Tagesring** (Kopfbereich): füllt sich mit jedem Check-in der 4 Ankerzeiten – sofortige Belohnung, Status auf einen Blick.
1. **Primärbutton „Check-in jetzt”** – groß, immer an derselben Stelle. Kein Suchen der richtigen Stunde mehr; die Zeit ist automatisch „jetzt”.
1. **Quick-Event-Leiste** (☕ 🍽️ 💊 ⚠️ 🦵 ＋).
1. **Kontextkarte:** morgens die Morgenkarte, abends die Abendkarte, dazwischen kompakte Tageszusammenfassung (Med-Status, Schlaf, bisherige Check-ins).
1. **Timeline** (komprimiert): die 24h-Leiste aus v1 bleibt als *Visualisierung* mit minutengenauen Punkten – primär zum Lesen und für nachträgliche Einträge (Tap auf Zeitpunkt = rückdatierter Check-in). Sie ist nicht mehr der Haupteinstieg.
1. Datumsleiste zum Zurückblättern wie in v1.

### 6.3 Check-in-Screen

Eine Karte, drei Metrik-Zeilen, ein Speichern-Button. Darunter eingeklappt: „Mehr erfassen” (Zusatzmetriken, Ereignis hinzufügen, Notiz). Speichern zeigt ein kurzes, ruhiges Erfolgs-Feedback und schließt sofort – kein 1,2-Sekunden-Zwangsbildschirm wie in v1, der bei schnellen Nutzern Doppeltipps provoziert. Entwurf bleibt bei Abbruch/App-Wechsel erhalten (ADHS-Realität: man wird unterbrochen).

### 6.4 Auswertung: 8 Tabs → 4 Bereiche

|Neu        |Enthält (aus v1)                                                                                                             |Bemerkung                                                                                  |
|-----------|-----------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
|**Tag**    |Tagesgraph (Schlafschattierung, Einnahmelinie, Event-Icons), Eintragsliste des Tages                                         |minutengenaue Punkte statt Stundenraster                                                   |
|**Trends** |Metrik-Trends, Schlaftrend, Ø Stundenverlauf, Gewicht, RR/Puls, Ibuprofen/Woche (ISO-KW!), Symptomfrequenzen, Schmerz-Heatmap|ein Bereich mit Unterauswahl (Segmente: Metriken · Schlaf · Körper · Symptome) statt 3 Tabs|
|**Muster** |Insights, Baseline-Vergleich, Medikamentenvergleich, Ereignis-Häufigkeiten                                                   |jedes Insight zeigt n und Konfidenz-Wortlabel; einheitliche Mindest-n zentral definiert    |
|**Bericht**|Arztbericht (Text + Druck/PDF), CSV, JSON-Backup/Import, Backup-Status („zuletzt gesichert vor X Tagen”)                     |unverändert im Umfang                                                                      |

Vier Bereiche passen ohne Scrollen nebeneinander – nichts ist mehr „hinter dem Rand”.

### 6.5 Einfach/Erweitert systematisch statt als Schalter-Flickwerk

v1 hat den Advanced/Normal-Toggle nachträglich eingebaut. v2 macht daraus **Modul-Profile**: In den Einstellungen gibt es eine Modulliste (Zusatzmetriken, Gewicht, Blutdruck, Schmerztracking, Hersteller, Baseline-Modus, Abendkarte …), jedes Modul einzeln an/aus. „Einfach” und „Erweitert” sind nur zwei Voreinstellungen dieser Liste. Die Erfassungs-UI rendert sich komplett aus dieser Konfiguration – ein Mechanismus statt vieler Sonderfälle.

### 6.6 Mikro-Interaktionen (ADHS-Details)

- Defaults überall „wie zuletzt”: Medikament, Koffein-Getränk, Ibu-Dosis, Kontext-Tags.
- Undo-Snackbar statt Bestätigungsdialoge („Eintrag gelöscht – Rückgängig”).
- Keine Schuld-Sprache: leere Tage heißen „keine Daten”, nie „verpasst”.
- Rückdatieren immer möglich, max. 3 Taps entfernt.
- Haptisches Feedback (Vibration API) bei Speichern – kurzes physisches „erledigt”.

-----

## 7. Daten, Speicherung & „100 % auf meinem iPhone”

### 7.1 Bewertung deines Plans (Code auf GitHub, Nutzung per Link)

Der Plan ist genau richtig und bleibt die Empfehlung: **GitHub Pages liefert nur statischen Code; alle Daten entstehen und bleiben im Browser-Speicher deines iPhones.** Es gibt keinen Server, der je ein Datum sieht. Damit das verifizier­bar ist, bekommt v2 zwei Absicherungen:

- **Content-Security-Policy** im HTML-Head, die externe Verbindungen technisch unterbindet (`connect-src 'none'` o. ä.) – die App *kann* dann nicht nach Hause telefonieren, das ist im Quellcode für jeden prüfbar.
- Keinerlei externe Ressourcen (Fonts, CDNs, Analytics) – wie in v1 schon, jetzt als harte Regel dokumentiert.

### 7.2 Speicher-Upgrade: localStorage → IndexedDB + Persistenz

Fakten zur iOS-Lage:

- Safaris 7-Tage-Löschung für skriptbeschreibbaren Speicher (ITP) betrifft Webseiten im Browser; **als Home-Bildschirm-App installierte Web-Apps führen einen eigenen Nutzungszähler und sind davon ausgenommen** – Apple erwartet ausdrücklich nicht, dass deren Daten gelöscht werden.
- Trotzdem kann iOS bei Speicherdruck Daten verdrängen („best effort”), solange die Origin nicht im **Persistent-Modus** ist. Deshalb: `navigator.storage.persist()` bei jedem Start anfordern + `storage.estimate()` anzeigen.
- localStorage bleibt zusätzlich limitiert (~5 MB, synchron/blockierend). IndexedDB hat großzügige Quoten und ist asynchron.

**v2-Architektur:**

1. **IndexedDB als Primärspeicher** (kleiner eigener Wrapper, ~60 Zeilen, keine Bibliothek nötig) mit Stores: `days`, `checkins`, `events`, `settings`, `meta`.
1. **Installation als PWA wird aktiv beworben** (Onboarding-Hinweis „Zum Home-Bildschirm hinzufügen” mit Anleitung) – das ist auf iOS der wichtigste einzelne Schutz für die Daten *und* Voraussetzung für Push.
1. **Service Worker + Manifest:** App lädt offline, eigenes Icon, Vollbild – v1 hat nur die Meta-Tags, keinen echten Offline-Betrieb.
1. **localStorage nur noch als Notfallspiegel** der letzten ~30 Tage (falls IndexedDB je korrumpiert) + Migrationsquelle für v1-Daten.

### 7.3 Backup-Strategie (Pflicht, nicht Kür)

Datenverlust ist bei Gesundheits-Langzeitdaten der Worst Case, also dreifach absichern:

1. **Backup-Erinnerung:** Beim Öffnen prüft die App „letztes Backup > 7 Tage?” → dezente Karte mit 1-Tap-Export.
1. **Web Share API:** Export öffnet das iOS-Share-Sheet → direkt in „Dateien”/iCloud Drive sichern (komfortabler als der Download-Umweg von v1). JSON bleibt das Format, weiterhin + CSV.
1. **Optional verschlüsseltes Backup:** WebCrypto (AES-GCM, Passphrase) für alle, die das Backup z. B. per Mail verschicken – die Klartextoption bleibt.
1. **Backup-Status sichtbar** im Bericht-Bereich („Zuletzt gesichert: vor 3 Tagen · 412 Einträge”).

### 7.4 Erinnerungen – ehrliche Optionen ohne Datenabfluss

Web-Apps können auf iOS **keine lokalen geplanten Benachrichtigungen ohne Push-Server** auslösen. Die Optionen, sortiert nach Datensparsamkeit:

1. **iOS-Kurzbefehle-Automation (Empfehlung für v2.0):** „Täglich 9/13/17/21 Uhr → Benachrichtigung zeigen / URL öffnen”. Komplett lokal, null Infrastruktur. Die App liefert dafür eine Anleitung + tiefe Links (`…/index.html#checkin`), die direkt den Check-in öffnen.
1. **iOS-Erinnerungen/Wecker** mit der App-URL in der Notiz – simpel, robust.
1. **Web Push (v2.x, optional):** Funktioniert auf iOS nur für installierte Home-Bildschirm-Apps und braucht einen sendenden Server. Machbar mit generischen Inhalten („Zeit für deinen Check-in” – keinerlei Gesundheitsdaten verlassen das Gerät), aber es ist Infrastruktur + Wartung. Bewusst als spätere Option markiert, nicht MVP.
1. **In-App-Anker:** Beim Öffnen zeigt der Tagesring sofort, welcher Anker offen ist – die App nutzt jeden ohnehin stattfindenden Griff zum Handy.

### 7.5 Gibt es bessere Alternativen zum Gesamtansatz?

|Option                                                                                   |Bewertung                                                                                                                                                                                                                                                                                          |
|-----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|**A · PWA auf GitHub Pages (Empfehlung)**                                                |Daten 100 % lokal, kostenlos, dein voller Code-Besitz, sofortige Updates per Commit. Restrisiko Speicher-Eviction → durch Persist-Modus + Backups auf akzeptabel reduziert.                                                                                                                        |
|B · Gleiche PWA, zusätzlich lokal gespeichert („Sicherungskopie” der HTML in Dateien-App)|Schützt vor GitHub-Ausfall/Repo-Verlust; öffnetbar wäre sie über Safari nur eingeschränkt – eher als Code-Backup verstehen.                                                                                                                                                                        |
|C · Capacitor-Wrapper → echte iOS-App (sideload/TestFlight)                              |Bringt lokale Notifications, HealthKit (Schlaf/Puls automatisch!), garantierte Persistenz. Kosten: Xcode/Apple-Konto, Build-Pipeline, jährliche Re-Signierung ohne Dev-Account. Sinnvoller späterer Schritt, wenn v2 stabil läuft – das Datenmodell von v2 sollte ihn von Anfang an nicht verbauen.|
|D · iCloud-Sync über CloudKit JS o. ä.                                                   |Verlässt das „100 % auf dem Gerät”-Versprechen; nur als bewusste Opt-in-Zukunftsidee notieren.                                                                                                                                                                                                     |

**Empfehlung: A jetzt, C als dokumentierte Ausbaustufe.**

-----

## 8. Technische Architektur v2

### 8.1 Single-File-Prinzip behalten – aber nicht im Quellcode

Dein Constraint (eine selbsttragende index.html, keine externen Abhängigkeiten) bleibt **für das Artefakt** bestehen. Neu ist die Trennung von Quelle und Auslieferung:

```
/src
  app.js            – Bootstrap, Router (Hash), Render-Orchestrierung
  store.js          – Zustand + Aktionen (einzige Schreibstelle)
  db.js             – IndexedDB-Wrapper, Migrationen, Backup/Restore
  model.js          – Schema, Validierung, Ableitungen (Schlafdauer, ISO-KW …)
  stats.js          – reine Funktionen: Trends, Korrelationen (zentrale n-Schwellen), Insights
  report.js         – Arztbericht, Druck, CSV
  ui/…              – je View ein Modul (today, checkin, history, trends, patterns, report, settings)
  charts.js         – SVG-Erzeugung über DOM-API (kein innerHTML)
  styles.css, sw.js, manifest.webmanifest
/build.js           – ~40 Zeilen Node-Skript: inlinet alles → dist/index.html (+ sw.js, manifest)
/test                – Unit-Tests für model/stats (Node, ohne Framework-Zwang)
```

Ergebnis: weiterhin **eine** deploybare HTML-Datei auf GitHub Pages (plus Service Worker + Manifest daneben, die müssen als eigene Dateien liegen), aber wartbare, testbare Module im Repo. Falls du den Build-Schritt ablehnst, ist Plan B: native ES-Module direkt ausliefern (GitHub Pages kann das) – dann ~10 Dateien statt einer, ohne Build. Entscheidung O-1 unten.

### 8.2 Rendering-Modell

- Zentraler Store mit benannten Aktionen (`addCheckin`, `setDayContext` …); UI löst nur Aktionen aus.
- **Gezieltes Re-Rendern pro Sektion** (jede View abonniert die Datenpfade, die sie braucht) statt globalem `render()` – damit verschwinden die unkontrollierten Input-Workarounds und Fokusverlust-Bugs von v1.
- Event-Delegation am View-Root; keine globalen `window._…`-Handler.
- SVG-Graphen über `createElementNS` (keine innerHTML-Injektion); die buildXxxGraph-Logik aus v1 lässt sich dabei fast 1:1 portieren.

### 8.3 Datenmodell v2 (Kernentscheidungen)

```jsonc
// meta
{ "schemaVersion": 2, "createdAt": "...", "lastBackupAt": "..." }

// checkins: Zeitstempel ist der Schlüssel (mehrere pro Stunde möglich)
{ "ts": "2026-06-12T09:42:00+02:00",
  "metrics": { "focus": 4, "mood": 3, "energy": 4, "unrest": 2 },
  "note": "" }

// events: ereignisbasiert, minutengenau
{ "ts": "2026-06-12T08:05:00+02:00", "kind": "caffeine", "label": "Kaffee" }
{ "ts": "2026-06-12T16:30:00+02:00", "kind": "rebound" }
{ "ts": "...", "kind": "pain", "locations": ["knie_l"] }
{ "ts": "...", "kind": "ibuprofen", "mg": 400 }

// days: alles, was pro Kalendertag genau einmal existiert
{ "date": "2026-06-12",
  "sleep": { "from": "23:30", "to": "07:10", "quality": "bad",
             "tags": ["gedankenkarussell"] },
  "med":   { "name": "Elvanse", "doseMg": 30, "takenAt": "08:00",
             "manufacturer": "Takeda", "pause": false, "baseline": false },
  "vitals": { "weightKg": 76.4, "bp": { "sys": 122, "dia": 78, "pulse": 64, "at": "08:10" } },
  "context": { "tags": { "arbeit": true, "stress": false }, "sick": false,
               "sport": true, "depressiveThoughts": false },
  "note": "" }
```

Regeln: Kalendertage immer in **lokaler Zeit** berechnen (eigene `localDateStr()`); ISO-8601-Wochen; `schemaVersion` + Migrationskette in db.js; jedes Feld optional (Teilerfassung ist der Normalfall, nicht der Fehlerfall).

### 8.4 Qualitätssicherung

- Unit-Tests für: Schlafdauer über Mitternacht, lokale Datumsgrenzen (00:30 Uhr!), ISO-KW, Korrelations-n-Schwellen, Migration v1→v2, Backup-Roundtrip (Export→Import→identisch).
- Ein Satz Beispieldaten (30 Tage) als Fixture – auch praktisch, um die Auswertung beim Entwickeln zu sehen.
- Manuelle Testcheckliste iOS Safari: Installieren, offline öffnen, Persist-Status, Eingabe-Fokus, Druckreport.

-----

## 9. Anforderungskatalog (MUSS / SOLL / KANN)

### Funktional – Erfassung

|Nr  |Prio|Anforderung                                                                                                                                    |
|----|----|-----------------------------------------------------------------------------------------------------------------------------------------------|
|F-01|MUSS|Moment-Check-in mit Fokus/Stimmung/Energie, speicherbar in ≤ 3 Taps + Speichern, minutengenauer Zeitstempel, rückdatierbar                     |
|F-02|MUSS|Zusatzmetriken (Unruhe, Impulsivität, Hunger) als zuschaltbares Modul im selben Check-in                                                       |
|F-03|MUSS|Quick-Events Koffein, Essen, Ibuprofen (Dosiswahl), Rebound/Crash, Schmerz (Silhouette), freies Ereignis – je ≤ 2 Taps vom Heute-Screen        |
|F-04|MUSS|Morgenkarte: Schlafzeiten + Qualität + Subtags; Medikation mit „Wie gestern”-1-Tap, Pause, Detailformular (Name/Hersteller/Dosis/Zeit/Baseline)|
|F-05|MUSS|Abendkarte: Kontext-/Stress-Tags (Vortag vorausgewählt), Tagesflags (Sport, depressive Gedanken, krank), Tagesnotiz – überspringbar, nachholbar|
|F-06|MUSS|Alle Einträge nachträglich editier- und löschbar mit Undo                                                                                      |
|F-07|SOLL|Gewichts- und Blutdruckmodul (inkl. „RR-Serie für X Tage”)                                                                                     |
|F-08|SOLL|Entwurfs-Persistenz: unterbrochener Check-in geht nicht verloren                                                                               |
|F-09|KANN|Eigene Metriken/Tracker durch Nutzer definierbar (Registry-Modell)                                                                             |

### Funktional – Auswertung & Bericht

|Nr  |Prio|Anforderung                                                                                                                                         |
|----|----|----------------------------------------------------------------------------------------------------------------------------------------------------|
|F-10|MUSS|Tagesgraph (Schlafschattierung, Einnahmemarker, Event-Icons) mit minutengenauen Punkten                                                             |
|F-11|MUSS|Trends: Metriken, Schlafdauer, Gewicht, RR/Puls, Ibuprofen pro ISO-KW, Symptom-/Schmerzfrequenzen, Schmerz-Heatmap; Zeiträume 7/30/365              |
|F-12|MUSS|Insights mit zentral definierten Mindest-n und sichtbarer n-Angabe je Aussage; Baseline-Vergleich; Koffein-Fenster-Analyse; Schlaftag→Folgetag-Fokus|
|F-13|MUSS|Medikamentenvergleich (inkl. Hersteller-Dimension) Tag/Woche/Monat                                                                                  |
|F-14|MUSS|Arztbericht Text + Druck/PDF (A4) funktional mindestens wie v1; Wirkdauer-Filter 2–12 h; Ibuprofen weiterhin nicht im Druckreport                   |
|F-15|MUSS|Export JSON (vollständig, re-importierbar) und CSV (Semikolon, de-DE)                                                                               |

### Daten & Datenschutz

|Nr  |Prio|Anforderung                                                                                                               |
|----|----|--------------------------------------------------------------------------------------------------------------------------|
|D-01|MUSS|Primärspeicher IndexedDB; `navigator.storage.persist()` bei Start; Speicherstatus einsehbar                               |
|D-02|MUSS|Keine externen Requests; CSP im Dokument erzwingt dies; keine Fonts/CDN/Analytics                                         |
|D-03|MUSS|Import aller v1-Daten (focuslog_v1 inkl. Altschlüssel-Kette) mit Mapping Stundenraster→Zeitstempel, Schlaftags Checkin→Tag|
|D-04|MUSS|Backup-Erinnerung (>7 Tage), Export über iOS-Share-Sheet, Backup-Status sichtbar                                          |
|D-05|MUSS|`schemaVersion` + geordnete Migrationskette; Import alter Backups dauerhaft möglich                                       |
|D-06|SOLL|Optional passphrase-verschlüsseltes Backup (WebCrypto)                                                                    |
|D-07|KANN|„Alle Daten löschen” mit doppelter Bestätigung (Gerätweitergabe)                                                          |

### Technik & Qualität

|Nr  |Prio|Anforderung                                                                                                                    |
|----|----|-------------------------------------------------------------------------------------------------------------------------------|
|T-01|MUSS|Echte PWA: Manifest + Service Worker, offline voll funktionsfähig, Install-Hinweis im Onboarding                               |
|T-02|MUSS|Modulare Quellstruktur; Auslieferung als eine index.html via Build-Skript (oder ES-Module, s. O-1); keine Laufzeit-Dependencies|
|T-03|MUSS|Lokale Zeit für alle Kalenderlogik; ISO-KW; Mitternachts-Testfälle                                                             |
|T-04|MUSS|Gezieltes Rendern ohne Eingabe-Fokusverlust; keine globalen Handler; SVG ohne innerHTML                                        |
|T-05|MUSS|Unit-Tests für model/stats/db-Migration; 30-Tage-Beispieldatensatz                                                             |
|T-06|SOLL|Erinnerungs-Setup-Anleitung (Kurzbefehle) + Deep-Links (#checkin, #morgen, #abend)                                             |
|T-07|KANN|Web-Push-Ausbaustufe (separater minimaler Push-Dienst, generische Inhalte)                                                     |
|T-08|KANN|Capacitor-Pfad offenhalten (keine Web-only-APIs an architektur-kritischen Stellen ohne Abstraktion)                            |

### UX-Leitplanken

|Nr  |Prio|Anforderung                                                                            |
|----|----|---------------------------------------------------------------------------------------|
|U-01|MUSS|Eine Primäraktion pro Screen; Check-in-Button immer ortsfest                           |
|U-02|MUSS|Wortlabels je Metrikstufe, einheitliche Farblogik; nie zweideutige Skalenrichtung im UI|
|U-03|MUSS|Auswertung max. 4 Bereiche ohne horizontales Tab-Scrollen                              |
|U-04|MUSS|Defaults „wie zuletzt” überall; keine Pflichtfelder außer dem Wert selbst              |
|U-05|MUSS|Keine Schuld-/Verlust-Sprache; leere Slots sind neutral                                |
|U-06|SOLL|Tagesring/„erledigt”-Feedback + dezente Haptik nach Speichern                          |
|U-07|SOLL|Onboarding: 3 Screens (Idee · Installation · Erinnerungen einrichten)                  |

-----

## 10. Migration v1 → v2

1. **Automatisch beim ersten Start:** v2 liest `focuslog_v1` (und die Alt-Kette `adhs_v4` …) aus localStorage, mappt und schreibt nach IndexedDB; Original bleibt unangetastet, bis ein Backup-Export bestätigt wurde.
1. **Mapping:** Stunden-Check-ins → `ts = Datum + HH:00`; `events` aus Check-ins → eigenständige Event-Records mit demselben ts; `sleepTags/schlafschlecht` aus Check-ins → konsolidiert in `days.sleep` (bei Konflikten: „schlecht” gewinnt, Tags vereinigt); Tagesfelder 1:1; `healthFlags.sport/depression` → `days.context`; `gelenke + painLocations` → Pain-Events; `ibuprofenMg` → Ibu-Events.
1. **Fallback:** Manueller Import des v1-JSON-Backups über denselben Mapper (gleicher Codepfad = einmal testen reicht).
1. v1 bleibt unter altem Pfad erreichbar, bis v2 zwei Wochen fehlerfrei lief.

-----

## 11. Vorgeschlagene Ausbaustufen

1. **M1 – Fundament:** db.js, model.js, Migration, Tests, PWA-Hülle (offline + installierbar). *Ab hier sind die Daten sicherer als je in v1.*
1. **M2 – Erfassung:** Heute-Screen, Check-in, Quick-Events, Morgen-/Abendkarte, Modul-Profile.
1. **M3 – Auswertung:** Tag/Trends/Muster portieren (Graph-Logik aus v1 übernehmen), Insights mit n-Schwellen.
1. **M4 – Bericht & Backup:** Arztbericht, Druck, CSV, Share-Sheet-Backup, Erinnerungs-Onboarding.
1. **M5 – Feinschliff:** Haptik, Tagesring-Animation, optionale Module (RR-Serie, verschlüsseltes Backup).

## 12. Offene Entscheidungen (vor dem Coden klären)

- **O-1 Build-Schritt:** Eine index.html via 40-Zeilen-Build (empfohlen) **oder** ES-Module ohne Build, dafür ~10 Dateien im Deploy?
- **O-2 Check-in-Form:** Kompaktkarte (alle 3 Metriken auf einem Screen, Empfehlung) **oder** geführter Flow (eine Frage pro Screen, Swipe)?
- **O-3 Welche Metriken sind für *dich* unverzichtbar im Schnellpfad?** Vorschlag Fokus/Stimmung/Energie – falls dir z. B. Unruhe wichtiger als Energie ist, gehört sie ins Kerntrio (die Modulliste macht das konfigurierbar, aber der Default sollte zu dir passen).
- **O-4 Anker-Zeiten:** 4 Vorschlagszeiten fest verdrahtet oder konfigurierbar ab v2.0?
- **O-5 Erinnerungen:** Reicht Kurzbefehle-Anleitung (MVP) oder ist Web-Push dir den kleinen Serverdienst wert?

-----

*Konzept erstellt auf Basis der vollständigen Code-Analyse von FocusLog v1 sowie aktueller EMA-Compliance-Forschung, Leitlinien für kognitiv barrierefreies UI-Design und der WebKit-Speicherrichtlinien für iOS-Web-Apps.*

# Butler Fish — Bugs & Feedback (Live-Tracker)

**Was offen ist: Bugs, abgestimmte Bauten, Diskussionspunkte, Betrieb.** Erledigtes wandert ins `CHANGELOG.md`. Größere Features & Vision → `ROADMAP.md`.

Stand: 17. Juli 2026. Legende: 🐛 offener Bug · 🔧 offener Bau (abgestimmt) · 💬 Diskussion/Entscheidung · ⚙️ Betrieb.

> **Prozess-Regel (JC):** Bei **Diskussions-/Entscheidungspunkten** legt Claude Optionen vor und **wartet auf JC's Entscheidung** — keine Alleingänge, außer JC sagt ausdrücklich „bau es".

---

## ⚙️ BETRIEB — offen

*(Keine offen.)*

---

## 🐛 OFFENE BUGS

### Verhalten — Rollen
- **🔬 Ownerschaft übergeben am Gerät durchspielen (offen).** Der Rollen-Umbau (owner/admin/member) ist
  gebaut, live und in der DB verifiziert — Details im `CHANGELOG.md` (Commit `6abb9d7`). Geprüft wurde die
  **Logik**, nicht der **Fluss**: Die Übergabe hat noch niemand am Gerät durchgeklickt.

### 🔬 Am Gerät noch unverifiziert (live seit `878d17d`, 17. Juli — Details im `CHANGELOG.md`)
- **Sternchen-Legende kontextsensitiv** — braucht zum Prüfen einen Verein mit einem Pflichtfeld.
- **Erfolgs-Screen nach Einladung nennt Verein/Gruppe beim Namen** — braucht eine echte Einladung.
*(Stapel-Fix und lange Fischnamen sind an der ausgelieferten Fassung nachgemessen — die sind belegt.)*

### Filter
- **🐛? „Meine Fänge"-Filter: Gruppenfilter fehlt (JC, 17. Juli — „filter my catches is missing a group
  filter, ist it?").**
  **Nachgesehen, und der Befund ist nicht eindeutig:** Es *gibt* ihn. `#flt-ctx-field` mit dem Label
  **„Fänge aus"** (`showCatchesFrom`), und er wird genau dann eingeblendet, wenn
  `state.filterTarget==='stats'` — also auf „Meine Fänge". Der Filter-Knopf dort ruft `openFilter('stats')`,
  die Bedingung müsste greifen.
  **Damit sind es zwei mögliche Fehler, und ich weiß nicht welcher:**
  (a) Er erscheint tatsächlich nicht → echter Bug, dann am Gerät nachsehen, ob `filterTarget` stimmt.
  (b) Er erscheint, aber JC hat ihn unter „Fänge aus" nicht als Gruppenfilter erkannt → Benennung.
  *Für (b) spricht: Der i18n-Schlüssel `ctxFilter` heißt „Verein / Gruppe" — das Wort existiert also,
  wird an dieser Stelle aber nicht benutzt.*
  **Vor dem Fix: JC fragen, ob das Feld „Fänge aus" da ist.**

### Verhalten
- **🔧 Detail-Edit: Tap auf einen Verein soll nach dem Pflichtwert fragen (JC entschieden 17. Juli — NOCH ZU BAUEN).**
  **Der Befund, der JCs Entwurf („Warnung beim Speichern") nicht aufgehen ließ:** Im Detail-Edit gibt es
  **kein Speichern**. `detailToggleArea()` schreibt im Moment des Antippens direkt in die DB:
  `await getSB().from('catches').update({context_id, context_ids})` — ohne jede Prüfung. Der Verein ist
  drin, bevor ein Dialog aufgehen könnte. *(Im Formular greift es dagegen seit dem 17. Juli korrekt —
  dort sammelt `toggleArea` nur in `state.formTargets`, geprüft wird in `saveCatch`.)*
  **JCs Entscheidung:** Der Tap fragt gleich nach dem Wert — „ASV braucht die Entnahme" → Auswahl öffnet
  sich → danach ist der Verein drin. Kein Umweg übers Formular; der Moment bleibt dort, wo er ausgelöst wurde.

---

## 🔧 OFFENE BAUTEN (abgestimmt — kein Diskussionsbedarf)

- **🔧 Fotos umsortieren: ZIEHEN im Raster — entschieden (JC, 17. Juli).**
  Im Fotoeditor gibt es heute **nur „Titelbild setzen"** (`setFormCover`) — das schiebt ein Bild auf
  Platz 1; Bild 3 vor Bild 2 geht gar nicht. Die Daten können es längst: `catch_photos.sort` ist eine
  eigene Spalte, und `saveSoloPhotos` schreibt sie aus der Reihenfolge von `state.editPhotos`.
  **Es fehlt nur die Geste — und die gibt es bereits:** Vereine/Gruppen werden per **Long-Press-Ziehen**
  sortiert (Reihenfolge in `bf_order`, Handler ~Z. 6493). JC: *„verhalten sollte ähnlich dem umsortieren
  der gruppen und vereinslisten sein"* — **genau, dieselbe Mechanik wiederverwenden**, nicht neu erfinden.

- **🔧 Versionsnummer im Ladescreen (JC, 15. Juli)** — der `BUILD`-Marker liegt schon als Code-Cleanup
  in der Roadmap; hier bekommt er einen sichtbaren Ort. Beantwortet nebenbei die Frage „läuft das Update
  überhaupt?", die uns schon Zeit gekostet hat.

- **🔧 Rekord-Kacheln umsortierbar** — „Deine Rekorde"-Shelf (Stats, `rec-strip`) per **horizontalem**
  Drag&Drop neu ordnen (Reihenfolge lokal). *(JC, 14. Juli.)*

- **🔧 Einstellungen „+ add" → „−"** wenn geöffnet/aktiv (welche Stelle genau prüfen — Akkordeon/Add-Buttons). Klein.

- **🔧 Vereins-/Gruppenlogo in die WhatsApp-Einladung (JC, 17. Juli)** — die Einladungs-Nachricht könnte
  das Logo des Vereins/der Gruppe tragen. *(Prüfen: WhatsApp zieht Vorschaubilder aus OG-Tags einer URL —
  ein Bild lässt sich nicht in den Text legen. Braucht also eine Einladungs-Seite mit `og:image`,
  nicht nur einen Text. Hängt damit an der Deep-Link-Arbeit, Roadmap #17.)*

- **🔧 Altes „Bearbeiten"-Formular abschaffen** (JC: „can go") — Inline-Edit deckt alles ab;
  Einstiegspunkte/`openEdit`/`editCurrentCatch` entfernen, Formular-Sektion prüfen.

- **🔧 Angler-Filter aus Mitgliederliste** *(JC: „to change it")* — Filter mit allen Mitgliedern
  vorbefüllen („Ich" zuerst), nicht nur mit Anglern, die schon gefangen haben.

- **🔧 Einstellungen Verein/Gruppe-Selektor kontextsensitiv** — wie der Homescreen-„+" soll der
  Umschalter **nur eines** zeigen (entweder Verein **oder** Gruppe, je nach Kontext), nicht beides. *(JC, 13. Juli.)*

- **🔧 Ladezeiten/gefühlte Langsamkeit** *(JC: „follow the rec")* — Maskottchen-Loader zentriert beim
  Initial-Load/Datenabruf **und** echter Perf-Pass: Login-Queries parallelisieren, Fonts `display=swap`
  (später self-host), Foto-Thumbnails (später). Loader kaschiert, Perf-Pass behebt.

- **🔧 Gewässername aus dem Pin *vorschlagen* (JC, 16. Juli — Ansatz steht, Details offen).**
  **Entschieden (JC): vorschlagen, nicht setzen.** Beim Setzen eines Pins auf ein **leeres** Namensfeld
  schlägt die App einen Namen vor, den JC antippen oder ignorieren kann. **Nie überschreiben, nie
  automatisch** — sonst kostet ein verschobener Pin den handgetippten Namen („Hausstrecke").
  Recherchiert (16. Juli):
  - **Nominatim-Richtlinie:** max. **1 Anfrage/Sekunde**, **eigener User-Agent** Pflicht (Standard-Header
    der HTTP-Bibliotheken reichen ausdrücklich nicht), **systematische Abfragen verboten**. Einzelne
    Pin-Setzungen sind unkritisch; **einmal über alle Altfänge laufen wäre genau der verbotene Fall.**
  - **Der harte Teil ist inhaltlich:** Reverse Geocoding liefert eine **Adresse**, keinen Gewässernamen.
    Ein Pin auf dem Kleinen Brombachsee ergibt „Absberg, Weißenburg-Gunzenhausen, Bayern". Für den See
    bräuchte es eine gezielte Suche nach `natural=water` in der Umgebung — und ein Pin am Ufer trifft
    womöglich eine Wiese. Dazu: OSM-Namen sind oft nicht das, was Angler sagen.
  - Offen: Welcher Dienst? Vorschlag nur bei leerem Feld oder auch als „passt das noch?" nach dem
    Verschieben? Was bei mehreren Wasserflächen in Reichweite?

- **Batch B (Layout/Struktur):** 15 My-Catches-Button-Layout · 16 Teilen-Vorschau kombinieren.

---

## 💬 ZU DISKUTIEREN / ENTSCHEIDEN (Proposals kommen vor dem Bau)

### Löschen
- **💬 Löschen aus EINER Gruppe löscht den Fang ÜBERALL — ohne Warnung (JC, 17. Juli).**
  JC: *„wenn ich einen fang aus einer gruppe loesche, sollte es eine warnung geben, falls der fang noch
  in anderen gruppen geloggt ist."*
  **Nachgesehen, und es ist schärfer als die Meldung:** Es fehlt nicht nur die Warnung — es gibt gar
  keinen Weg, ihn *aus einer* Gruppe zu nehmen. Ein Fang ist **eine Zeile** mit `context_id` +
  `context_ids`; `deleteCatch()` macht `from('catches').delete().eq('id', id)`. Die Zeile ist weg, und
  damit der Fang aus **allen** Vereinen und Gruppen gleichzeitig. Der Dialog sagt dazu nur „Diesen Fang
  wirklich löschen?".
  **Der Fall ist nicht theoretisch:** In der DB liegen **2 von 17** Fängen in mehreren Kontexten, einer
  in **dreien**.
  **Die eigentliche Frage ist nicht der Warntext, sondern was „Löschen" hier heißen soll** — und davon
  hängt der Bau ab:
  - **(a) Nur warnen:** „Dieser Fang ist auch in *Butter bei die Mutti* und *ASV Steife Rute* eingetragen.
    Löschen entfernt ihn überall." Ehrlich, klein, ändert nichts an den Daten. Löst aber JCs eigentliches
    Problem nicht, wenn er ihn nur aus *einer* Gruppe nehmen wollte.
  - **(b) Zwei Aktionen trennen:** „Aus dieser Gruppe entfernen" (streicht die Gruppe aus `context_ids`,
    Fang bleibt) vs. „Fang löschen" (Zeile weg). Das ist vermutlich, was JC meint — und es ist die
    einzige Variante, die den Fang eines anderen Vereins nicht mit wegreißt.
  - **Zu klären, bevor irgendwas gebaut wird:** Wer darf was? Der Vereins-**Admin** möchte einen Fang
    vielleicht aus *seinem* Verein nehmen — er darf ihn aber nicht dem Angler aus dem Logbuch löschen.
    Heute kann er beides nicht unterscheiden, weil es nur einen Knopf gibt. Dazu: Was passiert mit der
    letzten Gruppe — wird der Fang dann Solo, oder ist das das Löschen? Und: Rekorde/Podium hängen an
    `context_ids`, müssen also nachgerechnet werden.


### Personal Best
- **💬 PB-Anzeige NEU diskutieren (JC, 14. Juli)** — JC will „how **and** where" ändern (nach
  Krone-neben-Art). Neue Mockups/Optionen nötig. *(Blockiert nichts mehr: Die manuelle PB-Kontrolle stand
  hier als „pausiert, bis die Anzeige entschieden ist" — sie ist längst gebaut und live.)*
- **💬 PB-Anzeige für DB/Verein-Fänge:** evtl. **„PB"-Text statt Krone** — oder **PB + Krone**?
- **💬 „PB erkannt. Bestätigen." — was ist besser bei neuen Usern, um Noise zu verhindern? (JC, 14. Juli.)**
  Der wunde Punkt: Bei einem neuen Nutzer ist **jeder Fang ein Rekord** — der erste Hecht ist der größte
  Hecht. Die Feier feuert dann bei jedem Log und entwertet sich in einer Woche. Optionen zu bauen
  (nicht entschieden): Feier erst ab dem n-ten Fang einer Art · erste Woche stiller · nur feiern, wenn
  ein *bestehender* Rekord geschlagen wird (nicht beim Aufstellen des ersten).

### Detail-Seite & Fang-Eingabe
- **💬 Lateinische Namen zu den Fängen (JC, 16. Juli — vertagt auf „morgen").**
  In Klammern hinter dem Artnamen im Hero. **Der Haken:** 8 der 9 Arten bilden sauber ab, **„Forelle"
  aber ist drei Arten** — Bachforelle (*Salmo trutta*), Seeforelle (*S. t. lacustris*) und Regenbogenforelle
  (*Oncorhynchus mykiss*, **andere Gattung**). Ein Name für „Forelle" wäre schlicht falsch.
  Dazu: **selbst eingetragene Arten bekommen gar keinen.** Hängt am kuratierten Artenkatalog (Roadmap).
- **💬 Karte aus dem Ortsnamen ziehen (JC, 16. Juli — vertagt).** Hat ein Foto keine Geodaten, JC trägt
  aber Gewässer/Ort ein — soll die Karte automatisch nachziehen? **Claudes Vorschlag: Karte zentrieren,
  Pin nie setzen** (ein gesetzter Pin behauptet Genauigkeit, die ein Ortsname nicht hat).
- **💬 Soll die Einstellungen-Option auf der Catch-Seite überhaupt bleiben? (JC, 16. Juli.)**
- **💬 Leere Catch-Bilder: warum nicht der Puffer? (JC, 17. Juli)** — Platzhalter für Fänge ohne Foto.
  *(Anmerkung: Der Hero ohne Foto wurde am 15. Juli auf Hintergrundfarbe + niedrige Höhe umgebaut —
  JC meint vermutlich die Listen-Thumbnails. Vor dem Mockup kurz klären, welche Stelle.)*
- **💬 (1) „Hero-Kategorie"** — unklar, Rückfrage offen: was/wo genau (Detail-Hero? Stats? Kategorie-Label
  auf dem Foto?).

### Filter & Auswahl-Elemente
- **💬 Ab wie vielen Einträgen braucht unser Auswahl-Element eine Suchleiste? (JC, 14. Juli)** — bei 2
  ergibt sie keinen Sinn. Und **„neu hinzufügen" ergibt nicht überall Sinn** (z. B. Kept/Released-Filter)
  → das Element muss kontextsensitiv werden. *(Ein Bau, drei Symptome: Suchleiste, „+ neu", das „—".)*
- **💬 Artenfilter als Mehrfachauswahl? Andere Filter auch? (JC, 14. Juli.)**
- **💬 Zoomen auf der Karte als Filter für die angezeigten Fänge? (JC, 14. Juli.)**
- **🔧 „Meine Fänge" bekommt ZWEI TABS: Übersicht / Fangliste — entschieden (JC, 17. Juli).**
  JCs eigener Vorschlag vom 14. Juli, und er löst **beide** Meldungen auf einmal: die Karte ist zu
  prominent (nimmt die halbe Seite und zeigt bei 2 Fängen zwei Punkte auf halb Europa), **und** die
  Knöpfe (Download/Sortieren) sitzen weird direkt über dem ersten Fang. Karte + Kacheln wandern in
  „Übersicht", die Liste bekommt ihren eigenen Raum mit ihren eigenen Knöpfen.
  *(Damit erledigt sich Batch-B-Punkt 15 „My-Catches-Button-Layout" mit.)*
- **💬 Gruppen-Seiten neu denken (JC, 14. Juli)** — sollen ebenfalls die Rekord-Liste bekommen
  (horizontal scrollend) und umsortierbare Rekord-Kacheln.

### Einstellungen & Struktur
- **💬 Icons in den Einstellungen?** *(Rest einer größeren Frage, die sich mit dem Feld-Umbau selbst
  erledigt hat — dieser Teil ist nie entschieden worden.)*
- **💬 Mehrspaltiges Layout je Gruppe?** In den Log-Feldern (Vereins-Liste) evtl. **2 Spalten** pro Gruppe,
  um Platz zu sparen. *(JC mag die Kategorien inhaltlich — nur Platz/Layout.)*
- **💬 Tagline neu brainstormen:** „Catch · Log · Organize" → Optionen sammeln.
- **💬 (Rundown #4) „App nutzt nicht die volle Höhe":** mein Safe-Area-Fix war vermutlich **nicht** das
  Gemeinte — klären, was genau gemeint war.

---

## ⏳ VERTAGT (JC hat sie bewusst geparkt)

### Erfolgs-Karte: glüht cyan (JC, 16. Juli: „diskutieren wir nochmal")
**Wo:** `#success-ov` / `.success-card` — erscheint nach dem Speichern eines Fangs. Als `.big` beim
**Rekord-Moment** mit Konfetti und einer pulsierenden Animation namens **`lureflare`** — *Köderlicht*.

**Der Befund:** Genau dieses Köderlicht ist `rgba(95,205,225)` — **cyan**, hartcodiert, kein Token.
Rest aus der Zeit vor dem Rebrand. Überall sonst (Lockup, Splash, Topbar) ist die Laterne inzwischen warm
`#EB9E18`. Im auffälligsten Moment der App glüht der Anglerfisch also kalt. `.su-check`, `.su-badge` und
`.success-card.big .su-title` hängen zusätzlich an `--spark` (Aqua).

**Die eigentliche Frage:** Ist Aqua die **Farbe des Feierns** — dann ist es eine Entscheidung und gehört
als Token benannt. Oder ein Versehen — dann zieht die ganze Karte auf warm nach, sonst bleibt sie halb
aqua und die Farbe sieht nach Zufall aus.
**Optionen:** (a) alles warm · (b) nur das Maskottchen warm, Rest aqua · (c) bewusst kalt, aber als
Token. Mockup: `mockup-glow.html`, Abschnitt ①.

### Login: gleiche Szene wie der Splash? (JC, 16. Juli)
**Der Login zeigt dasselbe Lockup und damit dieselbe 3,3-s-Choreografie.** Kein Blocker: die Eingabefelder
sind sofort benutzbar, es animiert nur das Logo darüber. **Aber:** Wer auf dem Login steht, will rein —
nicht zusehen.
**Optionen:** (a) so lassen · (b) kurze Einblendung (~0,9 s) · (c) ganz ohne Animation.
Technisch trivial: eigene Klasse statt `.anim` am `#login-logo`.

### Splash-Dauer bei Wiederkehrern (JC, 16. Juli: „leg es als Punkt in den Backlog")
**Der Ladescreen steht bei jedem Start 4,6 s.** Bewusst so entschieden, und für einen Design-Award ist es
ein Auftritt. **Das Problem kommt später:** Wer zum dritten Mal am Wasser einen Fang eintippt, wartet
4,6 Sekunden — genau dann, wenn die App schnell sein müsste (Fisch liegt im Kescher).
**Üblicher Weg:** volle Szene beim **ersten Start am Tag**, danach Kurzfassung (~1,2 s). Merker via
`localStorage` + Datum. **Offen:** ob überhaupt, und welcher Auslöser.

---

## 🧯 ALTLASTEN (gefunden, nicht angefasst — kein Auftrag, aber sie beißen später)

- **`saveSoloPhotos` löscht bei jeder Fotobearbeitung alle Zeilen und legt sie neu an** (`delete` →
  `insert`). Funktioniert heute nur, weil die Zuschnitte per `path` hinübergerettet werden. Zwei Risiken:
  **(a)** das nächste Feld an der Fotozeile fällt wieder hinten runter; **(b)** **schlägt das `insert`
  fehl, sind alle Fotozeilen des Fangs weg** — die Storage-Objekte überleben, die Zeilen nicht.
  Seit 17. Juli meldet der Fall wenigstens `console.error` statt zu schweigen (vorher ging er nur ins
  Test-Log). **Der echte Fix ist ein `upsert` statt delete+insert** — eigener Bau, nicht nebenbei.
- **Mitgliederliste in den Vereinseinstellungen zeigt keine Profilbilder**, obwohl sie geladen werden.
- **Toter Code:** `manifest.webmanifest` (nicht verlinkt) · `.mascot-band`-CSS.
  *(Die verwaisten i18n-Keys `trendNote`, `sparkCap`, `trend3`, `trend7`, `wSourceFields` sind am
  17. Juli entfernt — sie existierten wirklich und hatten null Verwendungen.)*

> **⚠️ Prüf-Falle, an der ich am 17. Juli hereingefallen bin — bitte nicht wiederholen:**
> `I18N` wird **nicht** von einem einzigen Objektliteral gebildet. Das Literal enthält nur **160**
> Schlüssel; die restlichen ~390 tragen **37 `Object.assign(I18N.de, {…})`-Blöcke** darunter nach.
> Wer nur das Literal auswertet (Klammern zählen ab `const I18N = {`), bekommt für jeden nachgetragenen
> Schlüssel `undefined` — und damit **ein „ist längst weg", das schlicht falsch ist**. Genau so habe ich
> JC gemeldet, die fünf Keys oben seien schon aufgeräumt, und den korrekten Tracker-Eintrag gelöscht.
> **Richtig:** den ganzen Bereich von `const I18N = {` bis `function t(k)` auswerten.

---

## ⚙️ BETRIEB — erledigt (Referenz, nicht löschen: das ist Live-Konfiguration)
- ✅ **Allowlist (14, Stand 17. Juli — in der DB geprüft):** johannesclaudi@gmail.com (Admin) ·
  oktay.duzgun@gmail.com · schuerholz.rene@googlemail.com · Matthew.scott0991@gmail.com · m_baron1@gmx.de ·
  michael-baron86@gmx.de · mberger2209@gmx.de · cmjcody@gmail.com · felixformhals@web.de ·
  jurjen.terpstra@gmail.com · leandro.gianfrancesco@gmx.de · svenflosbach@yahoo.de · ralphkempen@gmail.com ·
  **englart@hotmail.com** *(17. Juli auf JCs Ansage nachgetragen; von JC am 14. Juli zweimal gebeten und
  damals übersehen — die Person konnte sich drei Tage lang nicht einloggen)*.
- 📎 **Wie die Allowlist wirklich funktioniert** (gelernt am 17. Juli, damit es niemand nochmal suchen muss):
  Die App fragt `beta_allowlist` **nie** ab — die Prüfung sitzt komplett in der DB. Der Trigger
  `enforce_beta_allowlist()` wirft beim Anlegen `not_on_beta_allowlist`; gelesen wird über
  `is_email_allowed()`. Beide vergleichen über **`canon_email()`**, das kleinschreibt, trimmt und bei
  **Gmail/Googlemail zusätzlich Punkte und `+`-Suffixe entfernt** und auf `@gmail.com` normalisiert.
  **Folge:** Schreibweise ist egal, `j.c+beta@googlemail.com` = `jc@gmail.com`. Neue Einträge trotzdem
  kleingeschrieben ablegen — so macht es auch `approve_beta_request()`.
  **Nach einem Eintrag verifiziert man mit `select public.is_email_allowed('…')`, nicht mit einem
  `select *` auf die Tabelle** — nur die Funktion beantwortet die Frage, die der Login stellt.
- ✅ **info@thefishingbutler.app** — MX + SPF via Porkbun, getestet. *(`noreply@…` = nur Resend-Absender.)*
- ✅ **Rechtstexte (Beta)** — Impressum + Datenschutz de/en/nl, Amsterdam, DSGVO + RL 2000/31/EG.
  Für *öffentlichen* Launch offen: Anwalt, DPAs, Drittland — siehe `LEGAL-DRAFT.md`.
- ✅ **E-Mail-Backend** — `RESEND_API_KEY` gesetzt; Edge Functions `feedback-notify` + `beta-request-notify` ACTIVE.

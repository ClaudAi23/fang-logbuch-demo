# ButlerFish — Arbeitsregeln

## Mockups: immer als HTML-Datei

**Inline-Mockups (Widgets im Chat) rendern bei JC nicht — er sieht nichts.**
Mockups für dieses Projekt gehören **immer** in eine `.html`-Datei im Projektordner
(`mockup-*.html`) und werden dort angesehen. Keine Widgets, keine Bilder im Chat.

Öffnen: Finder ist freigegeben → `cmd+shift+g` → vollen Dateipfad tippen → `Return` → `cmd+down`.
Die Datei öffnet im Standardbrowser. (Die Chrome-Extension kann `file://` nicht laden, und
`curl` aus der Sandbox erreicht die Domain nicht — beides ist keine Option.)

Assets in Mockups: `mascot.png` / `puffer.png` von `https://thefishingbutler.app/…` laden,
Referenzbilder relativ (liegen im selben Ordner).

## Entscheidungen trifft JC

Nie eigenmächtig entscheiden. „Sieht gut aus" ist **keine** Freigabe zum Einbauen.
Vorschläge machen, Varianten zeigen, JC wählt.

## Deployen nur auf Ansage

Erst hochladen, wenn JC es sagt. Deploy-Weg: GitHub-Web-Upload (`/upload/main`),
Commit-Message per nativem Setter + `input`/`change`-Event, dann „Commit changes" klicken.
Die GitHub-API ist schreibgeschützt (403).

**„Commit changes": erst `hover`, dann per KOORDINATE klicken — nie per Element-Referenz.**
Der Klick über `ref_…` meldet Erfolg, löst aber nichts aus; die Seite bleibt still stehen. Auch der
reine Koordinaten-Klick ging am 17. Juli mehrfach ins Leere (bis zu zweimal hintereinander).
**Was zuverlässig funktionierte: `hover` auf den Knopf, dann `left_click` auf dieselbe Koordinate.**
Der Knopf sitzt unten links unter „Commit directly to the `main` branch" — vorher einen Screenshot
machen, seine Position wandert mit der Fensterhöhe.

**Nach JEDEM Klick die Commit-Liste prüfen** (`list_commits`, perPage 1) und mit dem erwarteten Titel
vergleichen. Das ist der einzige verlässliche Schritt — die Weiterleitung auf die Repo-Seite ist **kein**
Beweis (sie erfolgt auch ohne Commit), und ein verlorener Commit sieht aus wie ein erfolgreicher.
Steht der alte SHA noch: nochmal klicken, nicht weiterarbeiten.
Danach die **ausgelieferte Datei** befragen (`fetch('/index.html?v='+Date.now())`), nicht das
Vercel-Dashboard und nicht die Vercel-API — beide haben schon veraltete Stände gemeldet.

## „new:" heißt auf die Liste, nicht sofort bauen (JC, 17. Juli)

Fängt JC einen Punkt mit **`new:`** an, kommt er **auf die Liste** (`BUGS-AND-FEEDBACK.md` bzw.
`ROADMAP.md`) — nicht in den laufenden Bau. Sagt er **`new now:`**, wird es direkt gemacht.

Auf die Liste heißt nicht „hinschreiben und weiter": Der Eintrag soll etwas taugen. Kurz nachsehen,
ob der Befund stimmt, und das Ergebnis dazuschreiben — auch wenn es „existiert schon, heißt aber
anders" lautet. Ein Eintrag, der die halbe Arbeit schon erledigt hat, ist beim nächsten Mal Gold wert;
einer, der nur JCs Satz wiederholt, kostet dann dieselbe Suche nochmal.

## Tracker immer aktuell halten (JC, 17. Juli)

`BUGS-AND-FEEDBACK.md` und `ROADMAP.md` werden **laufend** gepflegt, nicht auf Nachfrage.

- **Neues von JC** (Mail, Chat, Screenshot) landet sofort im richtigen Dokument:
  Bug/abgestimmter Bau/Diskussion → `BUGS-AND-FEEDBACK.md` · Feature/Vision → `ROADMAP.md` ·
  Ausgeliefertes → `CHANGELOG.md`.
- **Erledigtes wird gelöscht, nicht abgehakt.** Häkchen-Friedhöfe verstecken das Offene.
  Ausnahme: Live-Konfiguration (z. B. die Allowlist) bleibt als Referenz stehen.
- **Neues an Bestehendes hängen, statt danebenzulegen.** Drei Einträge über dieselbe Sache sind
  schlechter als einer — dann sieht niemand, dass es ein Bau ist.
- **Status nie raten.** „Erledigt" nur nach Nachschauen; sonst ausdrücklich *unverifiziert*
  markieren. Es ist schon vorgekommen, dass ich JC etwas als kaputt gemeldet habe, das lief.

## Verifizieren: hinschauen, nicht nur messen

Zahlen haben hier schon mehrfach „grün" gemeldet, während die Seite sichtbar kaputt war.
Typische Fallen, die real aufgetreten sind:
- `getBoundingClientRect` ≠ computed width (Transform!)
- `getAnimations()` ohne `{subtree:true}` → leeres Array → `.every()` ist immer `true`
- Canvas `measureText` ohne `ctx.letterSpacing` → falsche Tinte-Kanten
- Messbereiche mit fester Grenze fangen Nachbarobjekte mit ein (Serviette, „f" von *fish*)
- Hintergrund-Verläufe werden an der Box-Kante hart abgeschnitten; `blur()` weicht das nur auf

## Guardrails (aus dem Briefing, unverändert)

App-Name, `butler.fish`-Domains und Supabase/Auth **nicht** anfassen.
Unklares melden statt raten.

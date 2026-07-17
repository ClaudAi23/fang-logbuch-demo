# ButlerFish — Roadmap (leading document)

**This is the leading doc for features, vision and decisions.** Three planning files, clear roles:
- **`ROADMAP.md`** (this file, EN) — features, big-vision phases, prioritisation, open decisions.
- **`BUGS-AND-FEEDBACK.md`** (DE) — the live tracker: open bugs, agreed builds, discussion, ops.
- **`CHANGELOG.md`** (DE) — the build log: what shipped, commit by commit.

*Snapshot: 17 Jul 2026. Legend: ✅ done · 🟡 built, not deployed · 🔜 next (client-side) · ⏳ needs backend · 🐛 bug · 💬 needs your decision.*

---

## The architectural line
Everything currently stores **locally in each browser / in Supabase per user**. Fine for: theming, sharing, weather, maps, solo/groups, personal stats, offline write-queue. **Needs the full backend** (accounts + shared data + server logic): cross-club leaderboards, trip voting, expenses, payments, cash book, club calendars. This line is what splits "🔜 next" from "⏳ vision" below.

---

## What's shipped
The Butler Fish rebrand + UX pass and all prior live features are logged in **`CHANGELOG.md`**. This doc tracks only what's still open.

---

## 🐛 Open bugs / ops
Live-tracked in **`BUGS-AND-FEEDBACK.md`**. As of 17 Jul (evening) the tracker is **empty of open bugs** —
what remains there is agreed builds, open decisions and known debt.
*Cleared 17 Jul, details in `CHANGELOG.md`: the role model (owner/admin/member, `6abb9d7`) · swipe-back
white screen (`0f697be`) · swipe-back navigation — one level per gesture (`21a66e4`), scroll position
(`df4aa86`), the dark flash (`4cabee2`), forward-swipe went backwards (`fcef41e`) · the nav-stack grew on
back, plus star legend / long species names / invite success screen.*
*Two headline items from this list were stale and are gone: the **test-log button** (searched the code, no
trace; JC: "ich glaub der ist schon gelöscht") and **"Beitritts-Anfragen" mislabelled** — the key already
reads `reqInboxTitle:"Gründungs-Anfragen"`.*
Launch-readiness done: info@ email · DE/EU legal texts · DB + storage reset. For a *public* launch still
needed: lawyer review, DPAs, third-country-transfer wording (see `LEGAL-DRAFT.md`).

---

## 🔜 Short-term (pre-launch polish, client-side)

**A. Verify on your device** (can't be tested on desktop): new home-screen icon (reinstall PWA) · edge-swipe back · celebration (normal + milestone) · extended FAB label · rem type at larger system font · cover reframe · offline logging.

**B. Accessibility** (from audit): bump sub-44 px touch targets (colour swatches ~34 px, some icon buttons ~38 px) · verify aria-labels on icon-only buttons · focus-trap in sheets/lightbox.

**C. Copy & content:** fix EN photo hint ("tap the star" → "tap a photo to set the cover") · empty states → CTAs ("Log your first catch →") · first-run coach-mark on the FAB · 💬 localise share-template tokens (`{art}`/`{länge}`… are German code tokens; needs EN token names + alias mapping so saved templates don't break).

**D. Performance / first impression:** self-host fonts (Barlow + Marcellus) — removes render-blocking third-party request + GDPR-cleaner (else `preconnect`) · iOS "Add to Home Screen" hint (desktop + Android + iOS explainer). 💬 **Perceived slowness** *(JC, 13 Jul)* — login→data and catch-photo loads feel slow. Likely causes: render-blocking Google Fonts, single 1.14 MB `index.html` parse, sequential Supabase queries on login (contexts → activity → counts), full-res catch photos with no thumbnails. Fixes to weigh: parallelize the login queries, self-host/`display=swap` fonts, generate/serve photo thumbnails, lazy-load. 💬 **Mascot loading screen** *(JC idea)* — centered-mascot loader during initial post-login fetch + catch-photo loads; masks latency + on-brand. Decide alongside the real perf pass (loader hides slowness but doesn't fix it).

**E. Small design calls:** 💬 context header (FAB now names destination — decide if a stronger banner is also wanted) · 💬 cream accent for stat values (theme-aware; cream fails on light bg) · angling-method field: confirm it's a clear dropdown like species (currently text + datalist).

**F. Personal Best info** *(JC, 13 Jul)* — surface personal bests/records to the angler (e.g. biggest length & weight overall and per species, first-of-species, this-year best). Client-side, data already captured (delivers on the onboarding "See your best" promise). A light version of the fuller **Insights** feature — ship the simple record cards pre-launch, the deep trends view later.

**G. Measurement-unit settings** *(JC, 13 Jul)* — let the user choose units: temperature **°C / °F**, weight **kg / lb**, speed **km/h / mph** (and length cm/in likely too). Device-bound setting (like theme/language), a new group in Settings → Display. Store raw values in metric; convert only for display + input placeholders. Touches: catch detail spec-sheet, weather (air + water temp), trolling speed, stats KPIs, form inputs, share template. Client-side.

---

## 🌙 14 July overnight batch — full spec in `OVERNIGHT-WORKLIST.md`
The big overnight build (rebrand · unified catch template · flow fixes · polish/fixes) and every locked decision are tracked in `OVERNIGHT-WORKLIST.md`; completed items land in `CHANGELOG.md`. New roadmap-level items from that session:

### ✅ Done overnight (14 Jul, live-verified)
- **Rebrand foundation shipped** (H below) — palette, `butlerfish` wordmark, ochre splash+motion, brand favicons/PWA icons/maskable, `site.webmanifest`, SW v7. All markers + assets verified HTTP 200; login screen visually confirmed. Details in `CHANGELOG.md`.
- **Bug fixed & shipped:** editing a *context* catch couldn't reselect the angler (member list loaded too late → only "me" available). Now loads members before selecting. Verified live.
- **Verified already-correct:** stats count by **angler** (`owner_id`), not logger; catches logged-for-others don't count for the logger; confirm/reject buttons present.

### 📄 Prepared overnight (docs for your review — no app risk)
- `GUIDE-domains.md` — step-by-step butler.fish canonical + redirects + Supabase/Resend/Vercel (you execute, I stand by).
- `PLAN-push-notifications.md` — plan + ready client/SW/edge code (needs your VAPID keys; not deployed).
- `PROPOSAL-personal-best.md` + `mockup-personal-best.html`.
- `PROPOSAL-following.md` + `mockup-following.html`.
- `PROPOSAL-welcome-catch.md` (Butler's Welcome Catch).
- Audits: `AUDIT-design.md`, `AUDIT-ux.md`, `AUDIT-copy.md`, `AUDIT-code.md`, `AUDIT-security.md`.

### ⏳ Still to build (locked, not shipped — deliberately not deployed blind)
- **#15 Unified catch template + keyboard-bar** — biggest UX change; build behind a flag + test together (mockups exist). 
- **#16 Selector sweep + Angler avatars** (typeahead-in-modal for species/method; avatars in angler picker/sheet).
- **#17 Invite auto-join deep-link** (`…/?join=CODE` → auto-join after login + success modal) · **angler reassignment** on edit · Claim-a-fish v2 (backend).
- **#18 Polish batch** — unify destructive (outline-red button everywhere, non-red hint) · home add-button B' (hide when both collapsed) · filter glow-up + seam fix + angler avatars · weather footer tidy · map external-link icon/position + restyled Leaflet zoom · Mondphase (drop emoji, theme-aware) · email→Konto accordion · onboarding icon alignment · **trolling-only-if-method-trolling** *(needs your trigger terms — see Decisions)*.
- **Code cleanups** (safe, verhaltensneutral, awaiting go): `isMyCatch()` extraction (dup logic ×3), `BUILD` marker, `setVal()` helper, drop now-unneeded `.brandname !important`.
- **Security to-dos** (`AUDIT-security.md`): RLS review + Supabase Security-Advisor · `catch-photos` bucket private+signed · CSP header in `vercel.json` · focused `innerHTML`/`esc()` pass on DB text fields · SRI+version-pin for CDN scripts.
- **Copy:** DE/EN/NL key-diff (I can automate) · update legal-text domain on the move.

**H. ButlerFish visual rebrand** — ✅ **shipped & live 14 Jul.** New deep-teal+cream+ochre palette (light+dark), icons/manifest/favicon, Barlow lowercase `butlerfish` wordmark, app name "ButlerFish", ochre splash + launch motion (part-level esca/bowtie choreography still needs a layered/SVG mascot asset — polish item). Brief + assets: `~/Downloads/butlerfish-brand/`.
**I. ⭐ Claim-a-fish invite (v2 growth loop)** — log a catch for a non-member → send an invite from that *unclaimed* fish → they sign up and it's waiting to claim (or they claim independently → raises an approval request to the logger). Needs backend (unclaimed state, invite token bound to catch, claim+approval, milestone). Full spec in worklist.
**J. ⭐ Butler's Welcome Catch (onboarding gag)** — a pre-seeded fun catch (JC supplies real friend photos/data) that auto-vanishes on first real log, excluded from stats, deletable. Proposal: `PROPOSAL-welcome-catch.md`.
**K. Personal Best tracking** — surface records; proposal + mockups: `PROPOSAL-personal-best.md`.
**L. Following members (social)** — follow + feed; proposal + designs: `PROPOSAL-following.md` (extends `SOCIAL-PROPOSAL.md`).
**M. Push notifications** — plan + partial client impl 14 Jul; needs VAPID keys + iOS 16.4+ standalone.
**N. Domains → butler.fish canonical** — butlerfish.com/.app + current URL all redirect; Supabase Site-URL/allow-list + email/Resend. Step-by-step: `GUIDE-domains.md`. (Supersedes Parked #2 below.)
**O. Recognise/classify catch** — AI species ID from a photo.
**P. Two-way beta feedback** — notify feedback-givers in-app when something's fixed / ask them follow-ups.

---

## Parked — Butler Fish rollout (revisit together)
1. ✅ **Logo / app icon — decided and shipped (Visual Update #2, 14 Jul):** the **puffer** (gold on deep-teal
   #0D5164) is the app icon + favicon; the **anglerfish is archived** as hero/brand mascot only. The old
   anglerfish-vs-monogram-vs-origami question is closed. *(Roadmap decision #1 below is therefore also closed.)*
2. **Domains & redirects** (URLs bought) — point **butler.fish** (primary) + **butlerfish.com** + **.app** at the app; 301 the old URL. **Critical:** update **Supabase Auth Site-URL + redirect allow-list** or magic-link/OTP login breaks. Optional: rename repo/Vercel to `butlerfish`.
3. **Social add-on** — spec locked in `SOCIAL-PROPOSAL.md` (MVP: profiles · follow · feed · Petri-Heil · notifications · privacy/fuzzed-location · discovery · block/report). Open: 💬 @handle scheme (rec: auto-generate). v2: comments · push · filters. v3: leaderboards/competition · challenges · recaps.

---

## Feature backlog — "later, no beta-blocker" (client-side unless marked ⏳)

**Branding / identity**
- **Reuse "Catch. Log. Organize."** *(JC, 13 Jul)* — the old triad tagline was replaced in the header by "Your catch, well kept." Find a useful, fitting home for "Catch. Log. Organize." elsewhere (e.g. onboarding welcome, empty-state, or an about/how-it-works blurb).
- **Butler "voice" / persona** — charming butler tone in copy + notifications (e.g. error → "The Butler was unable to balance the ledger, sir."; trip ready → "Your itinerary for Oudega is ready, sir."). Tasteful/optional, never at the expense of clarity.
- **"Butler by JC" signature** — sign automated emails (reminders, alerts) as "Butler by JC".
- ⏳ **Email sender logo in Gmail (BIMI)** — needs DMARC at enforcement (`p=quarantine`/`reject`; currently `p=none`), a square Tiny-PS SVG logo, a BIMI DNS TXT record, and (for Gmail to show it) a paid Verified Mark Certificate (~$1k/yr, requires a registered trademark). Post-beta nice-to-have.
- ⏳ **Push notifications** *(status, JC asked 13 Jul)* — **not built yet.** A shared backend capability, not a single feature. Needs: Web Push (service-worker `pushManager` subscribe + VAPID keys + a send path from Supabase Edge Functions), plus iOS caveat — **Web Push only works on iOS 16.4+ for apps added to the Home Screen** (standalone PWA), not in Safari tabs. Would power: notify the real angler when someone logs a catch for them (see below), club/group activity, reminders, and the whole Social layer. Currently referenced only inside the **Social add-on spec (v2)**; promote to its own build when we do the first notify-driven feature.

**Catches & data**
- **⭐ Auto-forward every log to a WhatsApp group (or similar)** *(JC, 16 Jul — new feature)* — per group,
  an admin sets a **notification target** in the group's settings: a WhatsApp group, an email group,
  iMessage, "to be explored". Every catch logged there gets posted to it. **This is how the club already
  works today** — someone catches a fish, it goes in the WhatsApp group — so this meets them where they
  are instead of asking them to move. **Before anything is built, the hard part:** *WhatsApp has no API
  for posting into a normal group.* The Business API sends to individuals who opted in, not to a group
  chat, and it costs per conversation. A realistic first version is probably **email group** (trivial,
  Resend is already wired) plus a **share-sheet nudge** ("post to WhatsApp?") — honest and buildable —
  rather than promising a WhatsApp integration that doesn't exist. Worth researching properly before
  scoping.
- **Manually override weather data** *(JC, 14 Jul)* — edit fetched weather by hand, and re-fetch/override
  again afterwards. *(Pairs with "Gewässer-Beobachtungen" below: weather = fetched, observations = seen.
  This item is the escape hatch for when the fetch is simply wrong.)*
- **Own weather forecast page** *(JC, 15 Jul)* — a forecast view, not just the conditions frozen onto a
  catch. **Note the licence collision:** Open-Meteo's free tier is **non-commercial only**, and a forecast
  page is a much heavier, more visible use than one call per catch. Check the terms before designing it.
- **Import script for the "Butter bei die Mutti" catch log** *(JC, 16 Jul)* — check whether the existing
  club log can be imported and pre-filled for members. Would give the group a populated app on day one
  instead of an empty one. Open: what format does the existing log even have?
- **Mark new catches as new** *(JC, 17 Jul)* — new-catch notifications should also **mark the new catches
  themselves** in the lists (unread/new state), so the notification and the app agree. Needs a per-user
  "last seen" marker. Ties into notifications.
- **⭐ Sharing settings — one home for "who sees what"** *(JC, 17 Jul; scope widened 17 Jul by JC's own call)*
  - **Turn off sharing your catches with others** *(JC, 17 Jul)* — a personal switch to stop your catches
    being visible to other members. **Needs care:** it layers on top of club/group membership and RLS —
    decide whether it hides you from the club feed, the stats, the leaderboards, or all three, and what a
    club admin is entitled to see regardless.
  - **"I'm not sharing the location this time"** *(JC, 15 Jul → parked here by JC on 17 Jul: "das machen
    wir später in den Social teilen angaben")* — today `optional` only means "may be empty"; there is no way
    to say *deliberately withheld*. A catch with no location and a catch whose location you are holding back
    are **two different statements**, and only one of them is an omission. JC's call: this is not a
    field-config question, it belongs with the sharing settings — decide it there, once, for every field.
- **Gear: offer to add it to the tackle box** *(JC, 15 Jul)* — when you add your first rod/reel/lure (and
  any new item after), ask whether it should go into your **tackle box**; plus an **item page with all
  the gear's details**. Extends "Personal gear inventory + export" below — same feature, this is the
  entry point into it.
- **Angler filter from member roster** *(JC, 13 Jul)* — today the club/group angler filter lists only anglers who have already logged a catch (so an empty context shows just "All"). Option: pre-populate it from the context's member list (with "Me" first) so you can filter by a member before they've logged anything. Small UX call.
- **Trolling-speed field** — optional (km/h or kn), shown when method = Trolling/Schleppangeln; wire into field-config, detail, CSV/PDF, share.
- **Catch depth** — optional manual field.
- **Water depth auto** — researched: likely **no free source for inland waters** (marine bathymetry ≠ lakes). Keep manual. Revisit only if a Garmin/Navionics-type API is worth it.
- **⭐ Personal gear inventory ("my tackle box") + "my waters"** — boat, engine, rods, reels, lures/baits;
  export. Later: link a catch to the gear used (auto-fill).
  - **JC, 17 Jul — this is why the suggestion store becomes a real column** (see `BUGS-AND-FEEDBACK.md`):
    Claude proposed deriving suggestions from the catches (no storage, automatically cross-device).
    **JC rejected it with the better argument:** a *derived* list **cannot be edited** — you could not
    rename a rod, delete one you sold, or attach details. And JC's own 15 Jul mail already asks for
    *"Item side all details of gear"* — so these are **objects with properties, not strings**.
    The suggestion store is therefore the **seed of the tackle box**, not a typing shortcut.
  - **The views come after go-live (JC's call)** — but the data has to be shaped right now, or we migrate
    out of a stopgap later.
- **Shared input suggestions (club/group)** — personal remembered suggestions for method/bait/rod/boat ship now (local `bf_suggest`). Later: optionally share a club/group's vocabulary so members get consistent suggestions. *(From tracker #7, JC 13 Jul: "personal for now, then roadmap for the others".)*
- **⚠️ Custom entries — Scope + Übersetzung (JC, 14 Jul; Richtung entschieden, Bau später):**
  - **Methode/Köder/Rute/Boot:** heute nur **gerätelokal** (`bf_suggest`). **Soll geräteübergreifend** werden (persönlich, aber synchron über alle Geräte) → serverseitig am Profil speichern statt localStorage. *Pro Verein/Gruppe ist nicht nötig — persönlich reicht.*
  - **Fischarten:** werden nicht auto-gemerkt (nur feste `SPECIES`-Liste + Freitext). **Statt Auto-Merken: Admin-Kuratierung** — selbst hinzugefügte/angefragte Arten laufen über ein **App-Admin-Panel** (JC), wo JC „Claude, füg diese Art + Übersetzungen hinzu" sagt → saubere, lokalisierte Katalog-Pflege.
  - **Übersetzung:** selbst hinzugefügte Arten **nicht auto-übersetzen** (als eingegeben anzeigen); nur der kuratierte Katalog (`SPECIES_TR`) ist mehrsprachig.
- **⏳ Fisch-Katalog + Sicht-Einstellungen (JC, 14 Jul — wichtig, um über die Beta hinaus zu wachsen; NICHT jetzt):** großer, kuratierter, **lokalisierter** Artenkatalog (aktuell nur 9). Dazu **Sicht-/Filter-Einstellungen pro Person / Gruppe / Verein**, z. B. **Salz-/Süßwasser** umschalten, nur bestimmte Arten anzeigen. Voraussetzung fürs Serving jenseits des Beta-Testers.
  - **Erweitert am 15. Juli (JC) — wie die Liste sich selbst sortiert:** Im **Onboarding** wird gefragt,
    ob der Fokus **Salz- oder Süßwasser** ist. Aus dem **Standort** zieht die App dann die lokalen Arten,
    anfangs **nach Popularität sortiert (Recherche)**, später **nach echter Nutzung**. Rangfolge:
    **eigene Fänge > App-Nutzung im Land > Recherche.** Einstellungen bleiben natürlich änderbar; ideal
    passt sich die Liste **dynamisch am Standort** an. — *Das ist der eigentliche Grund für den Katalog:
    nicht „mehr Arten", sondern **eine kurze, richtige Liste**. Zu klären: Standort bei jedem Log abfragen
    oder Heimatgewässer im Profil? Und was passiert im Urlaub — schlägt Norwegen sofort Dorsch vor, oder
    ist das genau der Moment, in dem man die Liste **nicht** umgestellt haben will?*
  - **Hängt daran:** die **lateinischen Namen** (Tracker) — 8 von 9 Arten bilden sauber ab, „Forelle" sind
    drei Arten aus zwei Gattungen, selbst eingetragene Arten haben gar keinen. Ein kuratierter Katalog
    löst beides in einem Zug.
- ⏳ **Custom user-defined extra fields** — users add their own field names (today only fixed fields toggle globally).
- **Insights** *(JC, 12 Jul)* — a dedicated insights/analysis view over your catches beyond the basic KPI tiles: trends over time (per month/season/year), best conditions (weather, moon phase, pressure, time of day), species breakdown, top waters, personal records. Personal-first; club-level insights later. (Builds on the data we already capture: weather, moon, location, method, size.)

**Roles & permissions** ⏳
- **Treasurer (Kassenwart)** role — was removed; returns with the club-finances feature (Phase 5).
- **More club roles** — water warden (Gewässerwart), 1st / 2nd chair, **forum moderator** *(JC, 17 Jul —
  note: presupposes a forum, which doesn't exist yet; parks naturally next to the Social add-on)*.
- **⚠️ The role model needs a pass before it grows, not after.** Two live bugs (admins can throw each other
  out; the group owner can't be demoted — both in `BUGS-AND-FEEDBACK.md`) say the same thing: **there is no
  rule about who may act on whom.** Every new role makes that hole bigger. Fix the rule first, then add roles.

**Onboarding**
- **"Save as app" demo — desktop + mobile** — extend the Add-to-Home-Screen helper to cover desktop (Chrome/Edge "Install app") + Android + iOS, with a short clip per platform.

**Login / auth** (code present but hidden)
- **Demo / guest mode** ("try without an account") — code intact; needs a "leave demo / sign in" path before re-enabling.
- **Password login & registration** — hidden; beta is OTP-only. All password code intact.
- **Magic-link login** — parked in favour of OTP; code kept.
- ⏳ **SMS OTP** — optional, paid.
- ⏳ **Session policy** — configurable login duration / inactivity timeout (Supabase → Auth → Sessions). Default today: effectively indefinite.
- ⏳ **Social login** — Google first (easy, big reach); Apple only with a native iOS app ($99/yr); not Facebook.

**Identity / accounts** ⏳ (needs backend)
- **Email change flow** *(JC, 13 Jul)* — account email is shown read-only in personal settings for now. Need a proper change-email flow (verify new address via OTP, update auth + profile, keep sessions valid). Until then email stays fixed.
- **★ Deletion / lifecycle** — delete own account + all data (GDPR erasure; needs an auth-admin path + cascade to profiles/catches/photos) · delete a group/Verein (cascade + strong confirm) · leave a club/group (clear action). Consider export-before-delete + a short undo grace.
- **One real account, cross-device** — ties identity + shared club data across devices.
- Per-group editable display name (cosmetic; identity = account ID).

**Launch / open beta** ⏳
- **"Beta" pill** on login/chooser (reuse the hidden `.demo-tag` pattern; remove at public launch).
- **Invite mechanism** — decide: open link vs allowlist vs request-access (allowlist is live now).
- **Custom domain** — non-destructive; add domain + DNS in Vercel + update auth redirect URLs (see Parked #2).
- **Final app name + logo** — name locked (Butler Fish); logo per Parked #1.
- **Legal EN/NL** — currently German only; add EN + NL versions.
- `context-logos` **bucket** — tighten the listing policy (security polish).

---

## ⏳ Big vision / platform (needs the full backend) — recovered verbatim

**Phase 3 — Competitive & social**
- **Leaderboard** — rank catches with the same filters; compare yourself + other clubs you belong to. Cross-club + memberships need a backend. *(Now expanded in `SOCIAL-PROPOSAL.md`.)*
- **Comments on catches** — group/club members comment under a catch; optionally toggleable per context by the admin.
- **Competition mode** — friends organise a contest; catches scored into a leaderboard. Own data model (contest, period, participants, scoring).

**Session: Zeit am Wasser loggen + live teilen + interaktive Karte (JC, 18. Juli)**
- JCs Ansage: *„you can share if you are fishing somewhere live. and show an interactive map"* —
  und nachgeschoben, entscheidend: ***„du loggst deine Zeit am Wasser."***
- **Der Nachsatz ist der Kern, nicht das Beiwerk.** Er dreht das Feature um:
  - **Er räumt die Plattformgrenze ab.** Für Zeitloggen braucht es **kein Tracking** — nur zwei
    Zeitstempel, Start und Ende. Kein `watchPosition`, kein Hintergrund-GPS, kein Batterieproblem.
    Alles, was unten unter „nicht baubar" steht, gilt fürs *Verfolgen* — nicht fürs *Loggen*.
  - **Er beantwortet, warum jemand die Session startet.** „Freunde sehen mich" ist ein Vielleicht.
    „Meine Stunden am Wasser" ist ein Grund, den man jeden Tag hat. Das Teilen wird damit zur
    **Zugabe** einer Sache, die sich für sich allein lohnt — und nicht umgekehrt.
  - **Er macht aus dem Moment einen Datensatz.** Eine Session ist ein Objekt mit Anfang, Ende, Ort —
    und **Fängen**. Damit sind Kennzahlen möglich, die eine Fangliste allein nie hergibt:
    Stunden am Wasser · Fänge pro Stunde · Schneidertage · beste Tageszeit · bestes Gewässer nach
    Ertrag statt nach Größe. Das ist Anglerwährung.
- **Datenmodell — die eigentliche Frage:** Gehört ein Fang künftig **zu einer Session**? Das wäre eine
  neue Beziehung (`sessions` ↔ `catches`), und sie berührt vieles: Loggt jemand einen Fang ohne offene
  Session, muss das weiter gehen (der Normalfall bleibt der Normalfall). Rückwirkend Sessions bilden
  („diese drei Fänge waren ein Nachmittag")? **Vor dem Bau zu entscheiden**, sonst wird es später teuer.
- **Was schon da ist — die halbe Miete liegt:**
  - **Supabase Realtime läuft bereits** (`_rtChannel = getSB().channel('ctx-'+ctxId)…subscribe()`).
    Für „live" braucht es also keinen neuen Unterbau, nur einen zweiten Kanal.
  - Leaflet, Marker, Vollbild-Karte, `fitBounds` — alles vorhanden.
  - `navigator.geolocation.getCurrentPosition` an drei Stellen (Formular, Detail-Pin, „Mein Standort").
- **Die eine Plattformgrenze — sie betrifft NUR das Verfolgen, nicht das Loggen:**
  `watchPosition` gibt es in der App nirgends, und auf iOS nützte es auch nichts: Eine PWA bekommt
  **keinen Hintergrund-Standort**. Bildschirm aus → GPS aus. „Live tracking" im Sinne von *meine Freunde
  sehen mich wandern* ist **nicht baubar**, solange ButlerFish eine PWA ist.
  **Das ist kein Verlust, sondern die Bestätigung des richtigen Zuschnitts:** Eine Session, die der
  Angler **startet und beendet**, braucht das nicht. Standort einmal beim Start (bei jedem Öffnen
  aktualisierbar), nicht dauernd nachgeführt. Deckt *„ich bin hier, kommt vorbei"* vollständig ab —
  und ist nebenbei das, was ehrlich funktioniert.
- **Die Fragen, die vor einem Mockup zu klären sind (alle sind Entscheidungen, keine Technik):**
  - **Wer sieht es?** Ein Verein, alle meine Gruppen, ausgewählte Leute? Standort ist die sensibelste
    Angabe der App — „öffentlich" wäre hier eine andere Klasse von Entscheidung als bei einem Fangfoto.
  - **Wie genau?** Exakter Pin oder auf das Gewässer gerundet? Angler verraten ihre Stelle ungern —
    die App soll das nicht zur Voreinstellung machen.
  - **Wie hört es auf?** Zeitlimit, manuelles Beenden, automatisch um Mitternacht? Eine Session, die
    jemand zu beenden vergisst, sendet sonst tagelang seinen Wohnort — **und verdirbt zugleich seine
    Statistik** (aus drei Stunden werden vierzehn). Seit dem Zeitloggen hat das Vergessen also zwei
    Folgen, und die zweite trifft den Angler selbst. Ein Vorschlag beim nächsten Öffnen — „Du bist seit
    9 Stunden am Wasser. Beenden?" — löst vermutlich beide.
  - **Was sieht man auf der Karte?** Nur „X angelt hier", oder live mitwachsend, was X fängt?
    Letzteres wäre der reizvollere Gedanke — und verbindet sich mit dem Punkt-→-Foto-Bau von heute.
- **Datenschutz:** Live-Standort ist personenbezogen und heikler als alles, was die App bisher speichert.
  Das braucht einen eigenen Absatz in der Datenschutzerklärung und eine ausdrückliche Einwilligung —
  nicht ein Häkchen, das schon gesetzt ist.

**Gewässer-Beobachtungen — eigene Kategorie (JC, 16. Juli)**
- Neue Felder: **Wassertiefe · Grundbeschaffenheit · Pflanzenbewuchs** (JC: „etc." — Liste offen).
  Zusammen mit der schon vorhandenen **Wassertemperatur** wären das vier Felder, die alle dasselbe
  gemeinsam haben: **JC beobachtet sie, keine API liefert sie.** Genau deshalb sitzt die Wassertemperatur
  heute falsch — sie steht im Wetterblock unter „Quelle: Open-Meteo", der sie nie geliefert hat.
- **Die Kategorie ist damit begründet, nicht bloss Ordnung:** „Wetter" = geholt, „Gewässer" = gesehen.
- **Vor dem Bau zu klären:** Kollidiert sie mit dem bestehenden **„Ort"**-Abschnitt (Karte + Gewässername)?
  Tiefe, Grund und Bewuchs beschreiben die **Stelle**, nicht das Gewässer als Ganzes — sie könnten also
  ebenso gut zu Karte und Pin gehören. Zwei Abschnitte über dasselbe Wasser wären schlechter als einer.
  Dazu: Datentypen (Tiefe = Zahl in m; Grund/Bewuchs = Auswahl oder Freitext?), und ob Vereine sie
  wie die anderen Felder auf Pflicht stellen dürfen.

**Phase 4 — Club organization**
- **Vereinstermine** — shared club calendar (meetings, work days, competitions); members see upcoming dates, optional RSVP/reminders.
- **Vereinsprotokolle** — meeting-minutes archive (create, store, browse); read for members, edit for the board.

**Phase 5 — Trips, money & bookkeeping**
- **Plan a trip** — date voting, location voting, bring-list & shopping-list; invite club/group members.
- **Expenses** — trip-level and club-level, split between participants.
- **Payments** — admin/treasurer marks "collected"; payee flags "paid / please check"; reminders for the reviewer.
- **Kassenbuchführung (club cash book)** — income/expense ledger, exportable. (Pairs with the treasurer role.)
- **Spenden empfangen (JC, 16 Jul)** — take donations. Nothing decided; noting the questions so they are not discovered late:
  *who* receives (JC personally / a company / the club — the tax and liability answers differ), *for what*
  (support the app vs. a club's cash box — a very different product), and *how* (payment provider, or a plain
  link out). Anything in-app on iOS collides with Apple's payment rules if it looks like buying something —
  a pure donation is usually fine, "support and get X" is usually not. **Not researched, not started.**

---

## 💬 Decisions I need from you

**Quick UI/UX calls**
1. ~~**Icon direction**~~ → ✅ **closed:** puffer, shipped with Visual Update #2 (see Parked #1).
2. **@handle** approach for social profiles (rec: auto-generate, editable).
3. **Context header** — FAB-only, or add a stronger "logging into X" banner?
4. **Cream accent** in the palette — yes/no?
5. **Share-template token localisation** — do it (EN tokens + alias map) or leave German tokens?

**✅ Resolved 14 Jul (discussion with JC) — now spec'd, ready to build**
7. **Trolling field** → shows only when the **dedicated trolling method** is selected from the method pick-list (no fuzzy text-match). Couples to #16 (method modal gets a canonical „Schleppangeln/Trolling" option).
8. **Wordmark on dark** → „butler" = **cream** (#F4EFE2), „fish" = brass. *(staged in code, not yet deployed.)*
9. **Records system** → **Personal Best global**, metric = **personal setting Länge/Gewicht (default Länge)**; marker = **crown in ochre badge, tag „PB"**. **Group/club records = per-species podium** (top 3 = gold/silver/bronze medals w/ rank number); metric **per context, admin-set (default Länge)**; **only gold** carries the word tag (DE Vereins-/Gruppenrekord · EN Club/Group record · NL Verenigings-/Groepsrecord), silver/bronze = medal only; detail line names rank+species+context+metric. Record-moment = **subtle confetti + badge** (≤900 ms, reduced-motion). Badge system = mono white symbol in coloured badge, no emojis. Spec in `PROPOSAL-personal-best.md` + `mockup-badges.html`.
   - **Locked additions (14 Jul):** PB metric = the **existing Settings → Länge/Gewicht ("measure")** control (not a separate setting); **released counts** as a record (confirmed). **Group/Verein records get an admin per-context on/off toggle** (default **on**) — some clubs are casual; folds into Stage 3. **Personal PB stays always-on** (no per-user hide toggle — subtle, self-motivating).
   - **✅ SHIPPED & live (full system, 14 Jul):** Stage 2 — DB flags `pb_length`/`pb_weight` + backfill; denormalized so group members see the crown; `recomputePb()` on log/edit/delete/confirm; crown in lists + detail banner; record-moment confetti; metric = the existing Settings measure control. Stage 3a — „Deine Rekorde" shelf in Stats. Stage 3b — group/Verein podium (gold/silver/bronze per species, gold-word tag, detail line; migration `add_context_record_settings`). Stage 3c — admin UI (podium metric + „Rekorde anzeigen" toggle) in club/group settings. All live-verified. See CHANGELOG.
   - 🔬 **TODO test (JC) — record-moment / confetti edge cases:** verify what's displayed for (a) **first-ever log** (`msFirst`/welcome), (b) **new PB longest/heaviest** (`msRecord` + confetti + crown appears), (c) **new species**, (d) combos (e.g. first log that is also a record — currently `first` wins and is shown exclusively), (e) reduced-motion (no confetti), (f) that the crown/banner/shelf all update immediately after the moment. Check copy + which milestone takes priority when several apply.
   - 🎛️ **Manual PB control** → jetzt auf der Bug/Feature-Liste (`BUGS-AND-FEEDBACK.md`, „vor dem großen Test heute"): „Nicht mein Rekord" entfernen + selbst als PB markieren.
10. **Following** → **Model B (profiles + approval)** chosen — bigger backend track (profiles, visibility, follow-requests); needs its own scoping doc. `PROPOSAL-following.md` updated.
11. **Welcome-Catch** → playful + very clear; name **Phantom-Fisch/Phantom Fish/Fantoomvis**, 235 cm/99,99 kg, Ort „In deinen Träumen", note #1, visible **DEMO** tag, stats-excluded, client-seed. Spec in `PROPOSAL-welcome-catch.md`.
12. **Decimal separator** → **language-bound** (DE/NL comma, EN dot).

**Strategic (later)**
6. Monetization (premium? club plans?) · native wrapper vs PWA · when to open registration.

---

*Provenance note: the Phase 3–5 vision and the branding/catches/roles notes above were recovered verbatim from the session logs after an accidental overwrite of the original `ROADMAP.md`. This consolidated version is now the source of truth for planning.*

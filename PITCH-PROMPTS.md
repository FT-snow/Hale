# Hale — Pitch Deck Slide Prompts
> Design language: **Premium Utilitarian Minimalism & Editorial** (document-style, warm monochrome, spot pastels)
> Made by **Anchal Singh (Leader) · Nandini Khandelwal · Prakhar Upadhyay**
> 10 slides · No project links (add before presenting if/when ready)
> Content grounded in repo docs: README, plan.md, docs/ui-inventory-before.md, docs/ui-contrast.md, docs/ui-parity.md, docs/ui-manual-qa.md

---

## Deck-wide design language

**Canvas & surfaces**
- Background: warm bone `#F7F6F3` (title/closing may use `#FBFBFA`). Never saturated color fields or colored hero sections.
- Cards: pure white `#FFFFFF`, `border: 1px solid #EAEAEA`, radius 8–12px max, padding 24–40px. Bento grids, asymmetrical.
- No gradients, no neon, no glassmorphism, no drop shadows (or a single diffuse `0 2px 8px rgba(0,0,0,0.04)` on hover only).

**Type hierarchy (extreme contrast)**
- Hero serif: **Newsreader** (or Instrument Serif) — tracking `-0.02em` to `-0.04em`, line-height `1.1`, for headlines and big numerals.
- Body sans: **Geist Sans** or **Helvetica Neue** — line-height `1.6`. Body text charcoal `#2F3437`, never pure black. Secondary text `#787774`.
- Mono meta: **Geist Mono** (JetBrains Mono acceptable) — uppercase eyebrows, letter-spacing `0.05em`, tags, citations, numbers.

**Spot pastels — one semantic color per slide, nothing else**
| Meaning | Background | Text/ink |
|---|---|---|
| Care / consent ON / positive | Pale Green `#EDF3EC` | `#346538` |
| Data / pipeline / technical | Pale Blue `#E1F3FE` | `#1F6C9F` |
| Caution / privacy / sandbox | Pale Yellow `#FBF3DB` | `#956400` |
| Problem / recording / urgent | Pale Red `#FDEBEC` | `#9F2F2D` |

**Components**
- Tags/badges: pill-shaped only at small size — 11px mono uppercase, tracking `0.05em`, pastel background, matching ink color. Never pill shapes on large containers or buttons.
- Buttons (in product mockups): solid `#111111`, white text, radius 4–6px, no shadow.
- Product mockups: wrap in faux-OS window chrome — white top bar, three small light-gray dots.
- Icons: Phosphor (Bold or Fill weight), one stroke width across the whole deck. No emojis anywhere.
- Illustrations: monochromatic continuous-line ink sketches on the bone canvas, each with exactly one offset geometric shape filled in a muted pastel.
- Note for screenshots: on-stage product screenshots use Hale's green UI — on the bone canvas they read as the intentional single saturated element. Keep it that way.

**Motion (if the deck tool animates)**
- Fade in + rise `12px` over `600ms`, easing `cubic-bezier(0.16, 1, 0.3, 1)`; grid items stagger `80ms`. Nothing else moves.

---

## SLIDE 1 — Title / Hook

**Talking points:** "Hale — Healthcare remembered." A care memory aid for patients and families. Explicitly not a diagnosis tool — the disclaimer lives in every product shell.

**Slide prompt:**
> Create a title slide on a warm bone background (#F7F6F3). Centered hero headline in Newsreader, tracking -0.03em: "Hale." — charcoal #111111, with the period set in pale green ink #346538. Beneath, one line in body sans: "Healthcare remembered." in muted gray #787774. A single small tag above the headline: 11px Geist Mono uppercase, letter-spacing 0.05em, pale green background #EDF3EC, text #346538 — "A CARE MEMORY AID — NOT A DIAGNOSIS TOOL". Lower third: a continuous-line ink sketch of a soundwave becoming a tree, charcoal on bone, with exactly one offset circle filled pale green #EDF3EC. Bottom corner, one mono line: "ANCHAL SINGH (LEADER) · NANDINI KHANDELWAL · PRAKHAR UPADHYAY". No logo, no photo, no gradient.

**Diagram prompt (decorative):**
> Continuous-line ink sketch: a soundwave that turns into a tree with deep roots, single black line on warm bone paper, one offset circle behind the canopy filled pale green. No text.

---

## SLIDE 2 — The Problem

**Talking points:** Care is a conversation — and conversations are forgotten. 53M+ unpaid US family caregivers; most medical detail evaporates within 48 hours; the story is scattered across six apps; nobody remembers *who said what, when*.

**Slide prompt:**
> Create a problem slide on bone #F7F6F3. Headline in Newsreader, tracking -0.02em: "Care is a conversation. Conversations are forgotten." — render "forgotten." with an inline mark: pale red background #FDEBEC, ink #9F2F2D, like a Notion highlight. Below, an asymmetrical three-card bento row, white cards, 1px #EAEAEA border, 12px radius, generous padding: "53M+" / "unpaid family caregivers in the US", "48h" / "before most visit detail is gone", "6" / "apps where the care story lives today". Numerals in Newsreader at display size, labels in body sans charcoal, card eyebrows in 11px mono uppercase. No icons in the cards — the numbers carry it.

**Diagram prompt:**
> Continuous-line ink sketch of a person in profile with five dissolving speech bubbles around them — doctor visit, medication instructions, family call, appointment, bill — each bubble breaking into small dots. Charcoal line on bone; one bubble offset with a pale red #FDEBEC fill. No text inside bubbles.

---

## SLIDE 3 — The Solution

**Talking points:** Hale records — with everyone present consenting first — transcribes, extracts source-linked memories, then answers "what did the doctor say?" **with citations to the exact transcript turn**. Voice assistant for the patient; permission-scoped shared memory for family.

**Slide prompt:**
> Create a solution slide on bone. Headline in Newsreader: "A shared memory for care." Below, an asymmetrical bento grid of three white cards, 1px #EAEAEA borders, 12px radius: (1) "Record & remember" — consent-first capture, per-segment retry; (2) "Ask, with sources" — every answer cites the transcript turn it came from; (3) "Coordinate care and costs" — records, reminders, shared expenses. Card 2 is the emphasis: its background is the pale green pastel #EDF3EC with ink #346538; the other two stay white. Each card: mono uppercase eyebrow, serif sub-headline, two lines of body sans. A small citation chip inside card 2: mono text "TURN 14:32 — DR. RAO" on white with 1px border. No gradients.

**Diagram prompt:**
> Three-part continuous-line sketch: a microphone becoming a notebook becoming a speech bubble with a quotation mark inside, drawn as one single charcoal line on bone paper, with one offset square filled pale green behind the speech bubble. Arrowless; the sequence reads left to right.

---

## SLIDE 4 — How It Works (Architecture) — KEY SLIDE

**Talking points (all real, from the codebase):** Foreground browser recording → segmented audio upload with 7-day raw retention and per-segment retry/discard → TigerData/Postgres queue → **Bun worker claims jobs with DB leases and `SKIP LOCKED`** — multiple replicas safe, heartbeats tracked → OpenRouter Whisper transcription → pyannote.ai diarization and voiceprint matching (opt-in per patient space; uncertain speakers stay **Unnamed**) → Backboard memory extraction plus nightly summaries → **source-grounded Ask** citing exact transcript turns. Every action lands in `audit_events`.

**Slide prompt:**
> Create an architecture slide on bone #F7F6F3 titled in Newsreader: "From conversation to recall." The full-width diagram is the centerpiece. Style it as an engineering document, not an infographic: every box is white with a 1px #EAEAEA border and 8px radius; every label is Geist Mono uppercase; arrows are 1px charcoal lines with small open arrowheads. Pipeline left to right: [PATIENT PWA · RECORDERCARD] to [SEGMENTED UPLOAD · 7-DAY RETENTION] to a cylinder [TIGERDATA POSTGRES · PROCESSING JOBS] to [BUN WORKER · DB LEASES + SKIP LOCKED] fanning to three parallel boxes: [OPENROUTER WHISPER · TRANSCRIPTION], [PYANNOTE.AI · DIARIZATION + VOICEPRINT MATCH], [BACKBOARD · MEMORY EXTRACTION + NIGHTLY SUMMARIES] — all converging on the terminal box [SOURCE-GROUNDED ASK · CITED TURNS], which is the only filled element: pale blue background #E1F3FE with ink #1F6C9F. A thin full-width rule beneath the pipeline labeled in mono: "AUDIT_EVENTS — EVERY ACTION LOGGED". Caption bottom-left in mono #787774: "CONSENT-GATED AT EVERY STAGE".

**Diagram prompt:**
> Flat engineering pipeline, monochrome: white boxes with 1px light-gray borders, mono uppercase labels, thin charcoal arrows; one cylinder shape for the database; exactly one box (the final "source-grounded ask") filled pale blue with dark blue ink. Cream-white paper background. Blueprint discipline, no 3D, no shadows.

---

## SLIDE 5 — The Patient Companion, Voice-First — KEY SLIDE

**Talking points:** No dashboard to learn — the patient just talks. The care voice assistant listens and speaks back (ElevenLabs, server-side). Medication self-reports are **confirmation-gated**: Hale reads back what it heard and waits for an explicit yes — a spoken word is never proof on its own. Reminders reach the patient as PWA push notifications, with an Apple Reminders handoff through Shortcuts. Logistics stay handoffs, not silent actions: rides open in Uber, appointments open a Cal.com booking page — Hale routes and never claims it booked. The assistant requires its own explicit online consent before it goes live.

**Slide prompt:**
> Create a product slide on bone #F7F6F3 titled in Newsreader, tracking -0.02em: "Care you can just talk to." Left half: one white 1px #EAEAEA-bordered conversation card with faux-OS window chrome — white top bar, three small light-gray dots — showing the voice exchange as a transcript. The patient's line right-aligned in charcoal body sans: "I took my morning pills at 8." Hale's line left-aligned: "Logging 8:00 AM medication — one tablet. Confirm?" The patient's reply: "Yes." followed by one small pale green tag #EDF3EC / #346538: "CONFIRMED". A solid #111111 mic button sits pinned at the card's bottom edge — the only filled element. Right half: four feature rows, each a Phosphor Bold icon (microphone, check-circle, bell, arrow-up-right) inside a 1px-bordered white square, serif row headline, one body-sans line: "Speaks and listens" — natural voice in and out, processed server-side; "Confirms before it remembers" — self-reports need an explicit yes, spoken words are never proof alone; "Reminders that reach them" — PWA push, plus an Apple Reminders handoff; "Handoffs, not lock-ins" — Uber opens the route, Cal.com opens booking, Hale links out and says so. Footer in mono #787774: "SEPARATE ONLINE CONSENT BEFORE THE ASSISTANT GOES LIVE". One pale green offset circle behind the conversation card. No gradients, no shadows, no emoji.

**Diagram prompt:**
> Continuous-line ink sketch on bone paper: one large speech bubble exchanging with a small notebook through a thin double-headed charcoal line, a drawn check mark floating between them, and one offset rectangle filled pale green behind the notebook. No text.

---

## SLIDE 6 — Family Experience

**Talking points:** Seven family routes — Overview with daily recap and provenance; Calendar with a keyboard-operable date grid, recording markers, nightly summaries; Conversations with search plus person/date filters and speaker-labeled transcripts; Ask Hale with citations; Care and Tasks; People with role-limited invites gated by `sharing:manage` and one-time links; Settings. Family sees only what the patient space authorizes — navigation never grants access.

**Slide prompt:**
> Create a product slide on bone titled in Newsreader: "Family sees what matters. Nothing they shouldn't." Left half: an Ask Hale answer mockup in a white card with faux-OS chrome — question in body sans, answer in charcoal, and beneath it a citation row: three small mono chips on white with 1px #EAEAEA borders — "TUE 14:32", "DR. RAO", "TURN 47" — the first chip tinted pale blue #E1F3FE with ink #1F6C9F. Right half: a seven-item checklist in body sans, each line separated by a 1px #EAEAEA rule, no boxes — document style. Footer row of three small mono tags: "ROLE-LIMITED INVITES", "SHARING:MANAGE GATE", "NAVIGATION IS NOT ACCESS". No illustrations of people.

**Diagram prompt:**
> A notebook drawn in single-line ink style with two devices — a phone and a laptop — flanking it, connected by thin charcoal lines; one page inside the notebook is offset with a pale yellow #FBF3DB fill and a small drawn lock. Bone background, no text.

---

## SLIDE 7 — Care Costs (Nessie / Capital One track)

**Talking points:** Sandbox bills, merchants, and spending views. Cost sharing is explicit — patients see only items a family member shared; balances stay private. P2P test transfers are reviewed, confirmed, and recorded idempotently — retries cannot double-pay. Hale never infers that a purchase means a medication was taken. Visibly labeled sandbox throughout.

**Slide prompt:**
> Create a financial slide on bone titled in Newsreader: "Splitting the cost of care, out in the open." Center: a receipt rendered as a white card, 1px #EAEAEA border, 8px radius, dashed bottom edge, set entirely in Geist Mono — line items in charcoal, two rows carrying a small pale green tag "SHARED" (#EDF3EC / #346538) and one row with a drawn lock and a muted gray "PRIVATE" label. Above the receipt, one small pale yellow badge #FBF3DB / #956400 in mono: "NESSIE SANDBOX — NO REAL FUNDS". Footer line in mono #787774: "NO MODEL-GENERATED AMOUNT EVER REACHES A TRANSFER — TOTALS ARE DETERMINISTIC". No coins, no dollar-sign icons.

**Diagram prompt:**
> A six-step horizontal chain rendered as an engineering sequence: white 1px-bordered boxes with mono labels — BILL, LABEL AS SHARED, FAMILY SEES SHARED ONLY, REVIEW AND CONFIRM, SANDBOX P2P TRANSFER, AUDIT RECORD — thin charcoal arrows between; the CONFIRM box filled pale green; a small drawn lock hanging off a private-branch line. Paper background, flat.

---

## SLIDE 8 — Trust: Consent and Privacy — KEY SLIDE

**Talking points (real gates in the code):** Recording starts only after an everyone-present consent checkbox. Ask sends excerpts to OpenRouter only behind an explicit per-question opt-in. The care voice assistant needs separate online consent. Voiceprint matching is opted in per patient space with an "only my voice" confirmation, withdrawable any time. Medication PDF extraction runs only after consent, and drafts are never saved until reviewed. Raw audio expires after the retention window; deleting source evidence deletes the AI's memory context. Every action is audited. All provider keys stay server-only.

**Slide prompt:**
> Create a trust slide on bone titled in Newsreader: "Consent first. Evidence always." Main element: four consent gates stacked vertically as document rows, each separated by a 1px #EAEAEA rule — left side a small toggle switch drawn flat (filled pale green #EDF3EC with a #346538 knob when ON), right side the unlocked capability in serif with a one-line body-sans explanation: "Cloud audio processing", "Ask via OpenRouter", "Voiceprint matching", "Medication PDF extraction". Toggles are drawn UI, not decorative art. Footer in mono #787774: "RAW AUDIO EXPIRES · DELETE EVIDENCE, DELETE MEMORY · EVERY ACTION AUDITED". No padlock clip-art; the only drawn lock is a line sketch.

**Diagram prompt:**
> A vertical consent ladder in monochrome line art: four rows, each a small flat toggle (green pastel when on) wired by a thin line to a labeled box. Beneath it, a horizontal charcoal timeline where a strip of small squares representing audio chunks fades to outline-only at the 7-day mark. Bone background, mono labels.

---

## SLIDE 9 — Accessibility, Measured — KEY SLIDE

**Talking points (measured, from docs/ui-contrast.md, ui-manual-qa.md, ui-parity.md):** Contrast is documented per token pair — 9.29:1 light body text, 15.87:1 dark, 21:1 high contrast. Four themes (Light, Dark, High-Contrast Light/Dark) times four text sizes (16/18/20/22px). A Playwright offline matrix runs all family and auth routes at 360/768/1440px across all sixteen combinations — zero console errors, zero horizontal overflow. Keyboard-only and screen-reader passes. Reduced-motion support. And the parity discipline: the redesign changed presentation only — zero routes, permissions, or handlers changed; no invented data states — "Upcoming" only, never fake "Done" or "Missed".

**Slide prompt:**
> Create an engineering slide on bone titled in Newsreader: "Accessibility, measured in numbers — not vibes." Left: one enormous numeral in Newsreader, charcoal — "21:1" — with an offset rectangle filled pale green behind it, and a mono label beneath: "HIGH-CONTRAST TEXT RATIO — WCAG MAXIMUM". Right: two compact white 1px-bordered cards. Card one: a 4x4 grid of small squares, every cell bordered 1px #EAEAEA, all filled pale green — labeled in mono "4 THEMES x 4 TEXT SIZES — PLAYWRIGHT-VERIFIED, 16/16". Card two: a minimal horizontal bar chart in mono labels — "4.5 WCAG MINIMUM" bar in muted gray #787774, "9.29 HALE LIGHT" and "21.0 HALE HIGH CONTRAST" bars in pale green with dark green ink. Bottom: three small mono tags: "KEYBOARD-FIRST", "SCREEN-READER TESTED", "NO INVENTED DATA STATES". No icons, no illustrations of people.

**Diagram prompt:**
> Two-panel monochrome figure on paper: left, a 4x4 grid of small rounded squares, all cells filled a single pale green with thin light-gray borders; right, three horizontal bars with mono axis labels — 4.5, 9.29, 21.0 — the first gray, the other two pale green. Flat, documentary, no chart junk.

---

## SLIDE 10 — Stack, Roadmap, and Why We Win

**Talking points:** Monorepo on Bun workspaces with shared `packages/contracts` for type-safe recall, voice, time, and demo contracts. Next.js + React + TypeScript, Clerk auth, TigerData PostgreSQL (canonical `db/schema.sql`, 21 tables), Bun worker with DB leases and `SKIP LOCKED`, Python sidecar (PyMuPDF extraction, optional local Whisper large-v3 GPU). Quality gates: lint, typecheck, build, production check. It works today — demo mode means the on-stage demo never breaks. Roadmap: background capture, wearables, Expo Android.

**Slide prompt:**
> Create a closing slide on bone #FBFBFA in three horizontal zones. Top zone: five stacked white bands, 1px #EAEAEA borders, 8px radius, mono uppercase labels — [NEXT.JS / REACT / TYPESCRIPT · PWA], [CLERK AUTH], [SERVER-ONLY API], [BUN WORKER · DB LEASES · SKIP LOCKED], [TIGERDATA POSTGRESQL · 21 TABLES] — plus a small offset sidecar card to the right: [PYTHON SIDECAR · PYMUPDF · GPU WHISPER]; only the worker band is tinted pale blue #E1F3FE with ink #1F6C9F. Middle zone: three roadmap items as a single charcoal line with three dots, mono labels — BACKGROUND CAPTURE, WEARABLES, EXPO ANDROID. Bottom zone: giant Newsreader line, charcoal, tracking -0.03em: "Healthcare remembered." with the period in pale green ink; beneath it one mono line: "THANK YOU — ANCHAL · NANDINI · PRAKHAR". No QR code, no links, no confetti.

**Diagram prompt:**
> Layered stack rendered as a document figure: five white 1px-bordered bands with mono labels in a vertical stack, one small sidecar box attached to the right of the worker band, the worker band tinted pale blue. Beside it, a single-line roadmap with three dots. Paper background, flat, no shadows.

---

## Pitch flow cheat-sheet (60–90 seconds per slide)

1. Hook → 2. Pain → 3. What Hale is → 4. **Architecture — spend time here; the pipeline is real** → 5–6. Voice companion, then family tour → 7. Nessie track → 8. **Consent and privacy** → 9. **The accessibility numbers — the mic-drop** → 10. Stack, close → live demo in `NEXT_PUBLIC_APP_MODE=demo` so nothing breaks on stage.

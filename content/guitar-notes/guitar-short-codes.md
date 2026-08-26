+++
authors = ["Dhruvik Donga"]
title = "Reference Guide: Writing Guitar Articles with the Custom Hugo Shortcode"
date = "2026-08-24"
description = "A developer and author guide explaining the design patterns, syntax, and visualization styles available in our string-centered Hugo guitar shortcode."
tags = ["hugo", "guitar", "music-theory", "guide", "web-dev"]
categories = ["Guides"]
+++

This reference guide establishes standard patterns for creating guitar visualizations in future articles. The underlying shortcode renders note badges centered directly on simulated guitar string lines, providing realistic fretboard diagrams with zero client-side JavaScript.

---

## 🛠️ Shortcode Parameter Spec

Every diagram is declared via the `{{</* guitar ... */>}}` shortcode using these parameters:

<div class="table-responsive">

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `title` | `string` | Optional | Descriptive label rendered above the fretboard. |
| `frets` | `csv` | **Yes** | Comma-separated fret indices. Diagrams automatically start at fret `0` (nut) up to the max fret specified. |
| `notes` | `csv` | **Yes** | Comma-separated tuples: `string:fret:label[:tag]`. |
| `mute` / `muted` | `csv` | Optional | Comma-separated unplayed string numbers (`1` to `6`). Draws a dashed line (`- - -`) and displays a red `✕` at fret 0. |
| `box` / `boxes` | `csv` | Optional | Highlights playable regions: `startString:endString:startFret:endFret[:color]`. |

</div>

---

## 🎨 Note Tagging & Visual Roles

The 4th parameter of any note tuple specifies its visual role:

```text
string_number : fret_number : note_text [: role_tag]
```

* **Standard Note (No Tag):** Defaults to slate grey badge (`#334155`). Used for general scale/chord tones.
* **Root Note (`:root`):** Highlighted with crimson red badge (`#e11d48`). Used for tonal center landmarks.
* **Start Note (`:start`):** Highlighted with amber gold badge (`#f59e0b`). Used for the initial exercise note, unisons, or scale run entry points.
* **Open String Notes (Fret 0):** Setting fret `0` (e.g. `5:0:A:start`) highlights the open string zone to the left of the thick nut wire.

---

## 📦 Playable Region Boxes (`box=...`)

The `box` parameter allows you to visually enclose playable chord or scale zones across designated strings and frets:

```text
startString : endString : startFret : endFret [: hex_or_rgb_color]
```

* **Automatic Palette:** If no color is passed, boxes cycle through a distinct palette (`cyan`, `emerald`, `purple`, `orange`, `pink`).
* **Custom Color:** You can provide a custom hex color code at the end (e.g. `1:3:7:9:#a855f7`).
* **Multiple Regions:** Separate multiple boxes with a comma (e.g. `box="1:3:3:5, 1:3:7:9"`).

---

## 🎸 Common Usage Styles & Recipes

### 1. Playable Region with Muted Bass Strings (`box` + `mute`)
Highlight the active 4-string, 4-fret playing zone for open chords while muting unused lower strings:

```javascript
{{/*< guitar
    title="Open D Major (Playable Box: Strings 1-4, Frets 0-3)"
    frets="0,1,2,3"
    mute="5,6"
    box="1:4:0:3"
    notes="4:0:D:root, 3:2:A, 2:3:D:root, 1:2:F#"
>*/}}
```

{{< guitar
    title="Open D Major (Playable Box: Strings 1-4, Frets 0-3)"
    frets="0,1,2,3"
    mute="5,6"
    box="1:4:0:3"
    notes="4:0:D:root, 3:2:A, 2:3:D:root, 1:2:F#"
>}}

---

### 2. Side-by-Side Triad Inversions with Unique Colored Boxes
Highlight separate 3-string triad zones across the neck with distinct colored borders:

```javascript
{{/*< guitar
    title="E Minor Triad Inversions (Strings 1-3)"
    frets="0,3,4,5,7,8,9"
    mute="4,5,6"
    box="1:3:3:5:#10b981, 1:3:7:9:#a855f7"
    notes="3:4:B, 2:5:E:root, 1:3:G, 3:9:E:root, 2:8:G, 1:7:B"
>*/}}
```

{{< guitar
    title="E Minor Triad Inversions (Strings 1-3)"
    frets="0,3,4,5,7,8,9"
    mute="4,5,6"
    box="1:3:3:5:#10b981, 1:3:7:9:#a855f7"
    notes="3:4:B, 2:5:E:root, 1:3:G, 3:9:E:root, 2:8:G, 1:7:B"
>}}

---

### 3. Open Position Chord Boxes (From Nut / Fret 0)
Ideal for standard open chords where open strings and fretted notes combine:

```javascript
{{/*< guitar
    title="C Major (Open Shape — Strings 1 to 5)"
    frets="0,1,2,3"
    mute="6"
    box="1:5:0:3:#0ea5e9"
    notes="5:3:C:root, 4:2:E, 3:0:G, 2:1:C:root, 1:0:E"
>*/}}
```

{{< guitar
    title="C Major (Open Shape — Strings 1 to 5)"
    frets="0,1,2,3"
    mute="6"
    box="1:5:0:3:#0ea5e9"
    notes="5:3:C:root, 4:2:E, 3:0:G, 2:1:C:root, 1:0:E"
>}}

---

### 4. Unison & Tuning Equivalences
Use `:start` alongside `:root` across open and fretted strings to highlight unison pitches:

```javascript
{{/*< guitar
    title="Unison Equivalence: 6th String Fret 5 (A) vs 5th String Open (A)"
    frets="0,1,2,3,4,5"
    notes="6:5:A:start, 5:0:A:root"
>*/}}
```

{{< guitar
    title="Unison Equivalence: 6th String Fret 5 (A) vs 5th String Open (A)"
    frets="0,1,2,3,4,5"
    notes="6:5:A:start, 5:0:A:root"
>}}

---

### 5. Full 2-Octave Box Scales
Enclose the whole scale pattern inside a multi-string box:

```javascript
{{/*< guitar
    title="A Minor Pentatonic (Box 1 — Frets 5 to 8)"
    frets="0,5,6,7,8"
    box="1:6:5:8:#f97316"
    notes="6:5:A:root, 6:8:C, 5:5:D, 5:7:E, 4:5:G, 4:7:A:root, 3:5:C, 3:7:D, 2:5:E, 2:8:G, 1:5:A:root, 1:8:C"
>*/}}
```

{{< guitar
    title="A Minor Pentatonic (Box 1 — Frets 5 to 8)"
    frets="0,5,6,7,8"
    box="1:6:5:8:#f97316"
    notes="6:5:A:root, 6:8:C, 5:5:D, 5:7:E, 4:5:G, 4:7:A:root, 3:5:C, 3:7:D, 2:5:E, 2:8:G, 1:5:A:root, 1:8:C"
>}}

---

### 6. How to Use Interactive Scale Animation in Your Markdown

Add `animate="true"` to any shortcode. The shortcode automatically:
* Attaches **Start** and **Stop** badges to the first and last notes in your sequence.
* Adds an interactive toolbar to adjust practice speed (`0.5x`, `1x`, `1.5x`, `2x`) or **Pause/Play** (`⏸` / `▶`).
* Pulses and highlights each note sequentially in the exact order declared inside `notes="..."`.

#### Syntax Example:

```html
{{/*< guitar 
    title="C Major (Box 1 / E-Shape) — Step-by-Step Run" 
    frets="0,7,8,9,10" 
    animate="true" 
    box="1:6:7:10:#0ea5e9" 
    notes="6:8:C:root, 6:10:D, 5:7:E, 5:8:F, 5:10:G, 4:7:A:start, 4:9:B, 4:10:C:root, 3:7:D, 3:9:E, 3:10:F, 2:8:G, 2:10:A:start, 1:7:B, 1:8:C:root" 
>*/}}
```

{{< guitar
title="C Major (Box 1 / E-Shape) — Step-by-Step Run"
frets="0,7,8,9,10"
animate="true"
box="1:6:7:10:#0ea5e9"
notes="6:8:C:root, 6:10:D, 5:7:E, 5:8:F, 5:10:G, 4:7:A:start, 4:9:B, 4:10:C:root, 3:7:D, 3:9:E, 3:10:F, 2:8:G, 2:10:A:start, 1:7:B, 1:8:C:root"

}}

### Pentatonic Practical Application: A Minor vs. C Major

Because A Minor Pentatonic and C Major Pentatonic share the exact same 5 notes (A – C – D – E – G), the playing path determines the tonal resolution:

A Minor Pentatonic (Ascending): Starts at low root A (String 6, Fret 5) and climbs to high root A (String 1, Fret 5), resolving on the natural minor center.

C Major Pentatonic (Descending): Cascades backward from high root C (String 1, Fret 8) down to low root C (String 6, Fret 8), resolving on the bright major center.

{{< guitar
title="A Minor Pentatonic (Box 1) — Ascending (Low A to High A)"
frets="0,5,6,7,8"
animate="true"
box="1:6:5:8:#0ea5e9"
notes="6:5:A:start, 6:8:C:root, 5:5:D, 5:7:E, 4:5:G, 4:7:A:start, 3:5:C:root, 3:7:D, 2:5:E, 2:8:G, 1:5:A:start"

}}

{{< guitar
title="C Major Pentatonic (Box 5) — Descending (High C to Low C)"
frets="0,5,6,7,8"
animate="true"
box="1:6:5:8:#10b981"
notes="1:8:C:root, 1:5:A:start, 2:8:G, 2:5:E, 3:7:D, 3:5:C:root, 4:7:A:start, 4:5:G, 5:7:E, 5:5:D, 6:8:C:root"

}}

## ✍️ Best Practices for Blog Authors

* **Order of Strings:** Always specify string numbers `1` (high e) through `6` (low E) correctly.
* **Muted Strings:** Declare `mute="5,6"` (or respective strings) whenever a chord or triad leaves strings unplayed.
* **Playable Boxes:** Use `box="s1:s2:f1:f2"` to guide the reader's eyes to the exact active grid zone.
* **Accents First:** Reserve `:start` for the primary entry/unison note and `:root` for octave anchors so diagrams remain scannable.
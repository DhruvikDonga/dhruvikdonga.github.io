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
| `mute` / `muted` | `csv` | Optional | Comma-separated unplayed string numbers (`1` to `6`). Draws a dashed horizontal line (`- - -`) and displays a red `✕` at fret 0. |

</div>

---

## 🎨 Note Tagging & Color Conventions

The 4th parameter of any note tuple specifies its visual role:

```text
string_number : fret_number : note_text [: role_tag]
```

* **Standard Note (No Tag):** Defaults to slate grey badge (`#334155`). Used for general scale/chord tones.
* **Root Note (`:root`):** Highlighted with crimson red badge (`#e11d48`). Used for tonal center landmarks.
* **Start Note (`:start`):** Highlighted with amber gold badge (`#f59e0b`). Used for the initial exercise note, unisons, or scale run entry points.
* **Open String Notes (Fret 0):** Setting fret `0` (e.g. `5:0:A:start`) highlights the open string zone to the left of the thick nut wire.

### String Index & Fret Inlay Mapping

<div class="table-responsive">

| Index | String | Visual Line Style | Unplayed (`mute="n"`) Style |
| :--- | :--- | :--- | :--- |
| `1` | High **e** | `1.5px` (Solid standard) | Dashed line (`- - -`) with `✕` |
| `2` | **B** | `1.5px` (Solid standard) | Dashed line (`- - -`) with `✕` |
| `3` | **G** | `1.5px` (Solid standard) | Dashed line (`- - -`) with `✕` |
| `4` | **D** | `1.5px` (Solid standard) | Dashed line (`- - -`) with `✕` |
| `5` | **A** | `2.0px` (Thick bass wire) | Dashed line (`- - -`) with `✕` |
| `6` | Low **E** | `2.5px` (Heaviest gauge) | Dashed line (`- - -`) with `✕` |

</div>

> **Fretboard Markers:** The shortcode automatically generates single inlay dots under frets **3, 5, 7, 9, 15, 17, 19, 21** and double inlay dots at octave **12** and **24**.

---

## 🎸 Common Usage Styles & Recipes

### 1. Open Chords with Muted Strings (`mute="5,6"`)
Use the `mute` parameter to display unplayed dashed strings (`- - -`) and `✕` indicators on strings that must not ring out:

```javascript
{{/*< guitar
    title="D Major (Standard Open Shape with Muted Bass Strings)"
    frets="0,1,2,3"
    mute="5,6"
    notes="4:0:D:root, 3:2:A, 2:3:D:root, 1:2:F#"
>*/}}
```

{{< guitar
    title="D Major (Standard Open Shape with Muted Bass Strings)"
    frets="0,1,2,3"
    mute="5,6"
    notes="4:0:D:root, 3:2:A, 2:3:D:root, 1:2:F#"
>}}

---

### 2. Open Position Chord Boxes (From Nut / Fret 0)
Ideal for standard open chords where open strings and fretted notes combine:

```javascript
{{/*< guitar
    title="C Major (Open Shape — Strings 1 to 5)"
    frets="0,1,2,3"
    mute="6"
    notes="5:3:C:root, 4:2:E, 3:0:G, 2:1:C:root, 1:0:E"
>*/}}
```

{{< guitar
    title="C Major (Open Shape — Strings 1 to 5)"
    frets="0,1,2,3"
    mute="6"
    notes="5:3:C:root, 4:2:E, 3:0:G, 2:1:C:root, 1:0:E"
>}}

---

### 3. Multi-Root Chords
Use multiple `:root` tags when a chord contains octave roots across multiple strings:

```javascript
{{/*< guitar
    title="G Major (Full 6-String Shape)"
    frets="0,1,2,3"
    notes="6:3:G:root, 5:2:B, 4:0:D, 3:0:G:root, 2:0:B, 1:3:G:root"
>*/}}
```

{{< guitar
    title="G Major (Full 6-String Shape)"
    frets="0,1,2,3"
    notes="6:3:G:root, 5:2:B, 4:0:D, 3:0:G:root, 2:0:B, 1:3:G:root"
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

### 5. Upper-Register Triad Inversions with Muted Strings
Mute the lower 3 bass strings when isolating 3-note triads on strings 1, 2, and 3:

```javascript
{{/*< guitar
    title="E Minor Triad (Root Position on Strings 1-3)"
    frets="0,7,8,9"
    mute="4,5,6"
    notes="3:9:E:root, 2:8:G, 1:7:B"
>*/}}
```

{{< guitar
    title="E Minor Triad (Root Position on Strings 1-3)"
    frets="0,7,8,9"
    mute="4,5,6"
    notes="3:9:E:root, 2:8:G, 1:7:B"
>}}

---

### 6. Full 2-Octave Box Scales
Map scales across all 6 strings in a stationary hand position:

```javascript
{{/*< guitar
    title="A Minor Pentatonic (Box 1 — Frets 5 to 8)"
    frets="0,5,6,7,8"
    notes="6:5:A:root, 6:8:C, 5:5:D, 5:7:E, 4:5:G, 4:7:A:root, 3:5:C, 3:7:D, 2:5:E, 2:8:G, 1:5:A:root, 1:8:C"
>*/}}
```

{{< guitar
    title="A Minor Pentatonic (Box 1 — Frets 5 to 8)"
    frets="0,5,6,7,8"
    notes="6:5:A:root, 6:8:C, 5:5:D, 5:7:E, 4:5:G, 4:7:A:root, 3:5:C, 3:7:D, 2:5:E, 2:8:G, 1:5:A:root, 1:8:C"
>}}

---

### 7. Interval & Scale Degree Labeling
Replace note names with interval degrees (`R`, `3`, `5`, `b7`) for theoretical analysis:

```javascript
{{/*< guitar
    title="Major Triad Formula (Interval Degrees)"
    frets="0,1,2,3,4,5"
    notes="5:3:R:root, 4:2:3, 3:0:5"
>*/}}
```

{{< guitar
    title="Major Triad Formula (Interval Degrees)"
    frets="0,1,2,3,4,5"
    notes="5:3:R:root, 4:2:3, 3:0:5"
>}}

---

## ✍️ Best Practices for Blog Authors

* **Order of Strings:** Always specify string numbers `1` (high e) through `6` (low E) correctly.
* **Muted Strings:** Always declare `mute="5,6"` (or respective strings) whenever a chord or triad does not use all 6 strings so readers immediately see the dashed unplayed region.
* **Accents First:** Reserve `:start` for the primary entry/unison note and `:root` for octave anchors so diagrams remain scannable.
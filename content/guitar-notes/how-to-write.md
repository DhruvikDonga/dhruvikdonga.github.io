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

Every diagram is declared via the `{{</* guitar ... */>}}` shortcode using three parameters:

<div class="table-responsive">

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `title` | `string` | Optional | Descriptive label rendered above the fretboard. |
| `frets` | `csv` | **Yes** | Active fret numbers forming the horizontal grid columns. |
| `notes` | `csv` | **Yes** | Comma-separated tuples: `string:fret:label[:tag]`. |

</div>

---

## 🎨 Note Tagging & Color Conventions

The 4th parameter of any note tuple specifies its visual role:

```text
string_number : fret_number : note_text [: role_tag]
```

* **Standard Note (No Tag):** Defaults to slate grey badge (`#334155`). Used for general scale/chord tones.
* **Root Note (`:root`):** Highlighted with crimson red badge (`#e11d48`). Used for tonal center landmarks.
* **Start Note (`:start`):** Highlighted with amber gold badge (`#f59e0b`). Used for the initial exercise note or scale run entry point.

### String Index Mapping

<div class="table-responsive">

| Index | String | Visual Gauge |
| :--- | :--- | :--- |
| `1` | High **e** | `1.5px` (Standard) |
| `2` | **B** | `1.5px` (Standard) |
| `3` | **G** | `1.5px` (Standard) |
| `4` | **D** | `1.5px` (Standard) |
| `5` | **A** | `2.2px` (Thick bass wire) |
| `6` | Low **E** | `2.8px` (Heaviest gauge) |

</div>

---

## 🎸 Common Usage Styles & Recipes

### 1. Open Position Chord Boxes (3-Fret Window)
Ideal for standard open chords where notes sit in frets 1–3:

```javascript
{{ /*< guitar
    title="C Major (Open Shape)"
    frets="1,2,3"
    notes="5:3:C:root, 4:2:E, 2:1:C"
>*/ }}
```
{{< guitar
    title="C Major (Open Shape)"
    frets="1,2,3"
    notes="5:3:C:root, 4:2:E, 2:1:C"
>}}

---

### 2. Multi-Root Chords
Use multiple `:root` tags when a chord contains octave roots:

```javascript
{{/* < guitar
    title="G Major (Full 6-String Shape)"
    frets="1,2,3"
    notes="6:3:G:root, 5:2:B, 1:3:G:root"
> */}}
```
{{< guitar
    title="G Major (Full 6-String Shape)"
    frets="1,2,3"
    notes="6:3:G:root, 5:2:B, 1:3:G:root"
>}}
---

### 3. Box Scales with Explicit Lead-In (`:start`)
Use `:start` on the lowest tonic pitch to guide finger sequencing in exercise articles:

```javascript
{{/*< guitar
    title="F Major Scale — 1st Position Box"
    frets="1,2,3"
    notes="6:1:F:start, 6:3:G, 5:1:Bb, 5:3:C, 4:2:E, 4:3:F:root, 3:2:A, 2:1:C, 2:3:D, 1:1:F:root, 1:3:G"
>*/}}
```
{{< guitar
    title="F Major Scale — 1st Position Box"
    frets="1,2,3"
    notes="6:1:F:start, 6:3:G, 5:1:Bb, 5:3:C, 4:2:E, 4:3:F:root, 3:2:A, 2:1:C, 2:3:D, 1:1:F:root, 1:3:G"
>}}
---

### 4. Wide 3-Note-Per-String (3NPS) Scales (5+ Frets)
The grid dynamically stretches across 5 or more columns when more fret indices are provided in `frets=""`:

```javascript
{{/*< guitar
    title="F Major Scale — 3NPS (Pattern 1)"
    frets="1,2,3,4,5"
    notes="6:1:F:start, 6:3:G, 6:5:A, 5:1:Bb, 5:3:C, 5:5:D, 4:2:E, 4:3:F:root, 4:5:G, 3:2:A, 3:3:Bb, 3:5:C, 2:3:D, 2:5:E, 1:1:F:root, 1:3:G, 1:5:A"
>*/}}
```

{{< guitar
    title="F Major Scale — 3NPS (Pattern 1)"
    frets="1,2,3,4,5"
    notes="6:1:F:start, 6:3:G, 6:5:A, 5:1:Bb, 5:3:C, 5:5:D, 4:2:E, 4:3:F:root, 4:5:G, 3:2:A, 3:3:Bb, 3:5:C, 2:3:D, 2:5:E, 1:1:F:root, 1:3:G, 1:5:A"
>}}

---

### 5. High-Register Movable Pentatonic Boxes
Shift the window anywhere across the neck by defining the start and end frets:

```javascript
{{/*< guitar
    title="A Minor Pentatonic — Box 1 (5th Position)"
    frets="5,6,7,8"
    notes="6:5:A:root, 6:8:C, 5:5:D, 5:7:E, 4:5:G, 4:7:A:root, 3:5:C, 3:7:D, 2:5:E, 2:8:G, 1:5:A:root, 1:8:C"
>*/}}
```

{{< guitar
    title="A Minor Pentatonic — Box 1 (5th Position)"
    frets="5,6,7,8"
    notes="6:5:A:root, 6:8:C, 5:5:D, 5:7:E, 4:5:G, 4:7:A:root, 3:5:C, 3:7:D, 2:5:E, 2:8:G, 1:5:A:root, 1:8:C"
>}}

---

### 6. Interval & Scale Degree Labeling
Replace letter names with interval numbers (`R`, `3`, `5`, `b7`) for theoretical analysis:

```javascript
{{/*< guitar
    title="Major Triad Formula (Interval Degrees)"
    frets="3,4,5"
    notes="5:3:R:root, 4:2:3, 3:5:5"
>*/}}
```
{{< guitar
    title="Major Triad Formula (Interval Degrees)"
    frets="3,4,5"
    notes="5:3:R:root, 4:2:3, 3:5:5"
>}}
---

## ✍️ Best Practices for Blog Authors

* **Order of Strings:** Always specify string numbers `1` (high e) through `6` (low E) correctly.
* **Keep Ranges Realistic:** Keep `frets` to contiguous sequences (e.g. `1,2,3` or `5,6,7,8`) to prevent awkward horizontal gaps.
* **Accents First:** Reserve `:start` for the primary entry note and `:root` for octave anchors so diagrams remain scannable.
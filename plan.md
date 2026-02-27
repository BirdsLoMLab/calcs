# Plan: Stat Calculator Implementation

## What the XLSX Describes

The `Final Attributes.xlsx` spreadsheet is a **Stat Upgrade Comparison Calculator** for Legend of Mushroom. It lets players:

1. Enter their **current stats** (HP, ATK, DEF) across three layers:
   - **Stat Total** — raw stat value (e.g. 20,495,900 for HP)
   - **Base Stat Total** — percentage modifier (e.g. 2,125,100% for HP)
   - **Global Stat Total** — percentage modifier (e.g. 626.81% for HP)

2. Enter a **planned upgrade** (additional Stat, Base Stat, or Global for any column).

3. See the **new final stats after upgrade** and the **difference** from current.

### Core Formula

```
Final Value = Stat × (1 + BaseStat / 100) × (1 + Global / 100)
```

Results are displayed as:
- **HP** → in Billions
- **ATK** → in Millions
- **DEF** → in Millions

---

## Implementation Plan

### Step 1: Add a New Tab — "Stat Calculator"

Add a third tab button to the existing tab navigation bar alongside "HP Damage Calculator" and "GuardianLife Calculator".

### Step 2: Create the HTML Structure for the Calculator

The new `#stat-calc-tab` tab content will contain:

#### Section A — Current Stats (Input)

A 4×3 grid (rows × columns):

|                    | HP         | ATK        | DEF        |
|--------------------|------------|------------|------------|
| **Stat Total**     | `<input>`  | `<input>`  | `<input>`  |
| **Base Stat Total (%)** | `<input>`  | `<input>`  | `<input>`  |
| **Global Stat Total (%)** | `<input>` | `<input>` | `<input>` |

Below the inputs, display the **calculated current final values** (highlighted):
- HP in Billions, ATK in Millions, DEF in Millions

#### Section B — Upgrade (Input)

A 3×3 grid for the upgrade deltas:

|                    | HP         | ATK        | DEF        |
|--------------------|------------|------------|------------|
| **+ Stat**         | `<input>`  | `<input>`  | `<input>`  |
| **+ Base Stat (%)** | `<input>` | `<input>` | `<input>` |
| **+ Global (%)**   | `<input>`  | `<input>`  | `<input>`  |

#### Section C — Results (Output)

Display the following computed results:

1. **New Final Stats** (after upgrade) — HP (B), ATK (M), DEF (M)
2. **Difference** — how much each stat increased
3. **% Increase** — relative improvement for each stat

Results should auto-calculate on input change (no submit button needed) for an interactive feel matching the existing calculators.

### Step 3: Add CSS Styles

- Follow the existing design language (white card, gradient header, dark blue background)
- Use the same `.container`, `.header`, `.section` class patterns from the HP calc tab
- Use a table/grid layout for the stat inputs
- Highlight final output values with a yellow/gold accent (matching the yellow cells in the xlsx)
- Style the difference row with green for positive increases

### Step 4: Implement the JavaScript Logic

```
function calculateStats() {
    // Read current stats
    // Read upgrade values (default 0)
    // Calculate current final: stat * (1 + base/100) * (1 + global/100)
    // Calculate new final: (stat + upgStat) * (1 + (base + upgBase)/100) * (1 + (global + upgGlobal)/100)
    // Calculate difference and % increase
    // Display results with appropriate unit formatting
}
```

- Attach `oninput` listeners to all inputs for real-time calculation
- Format HP as Billions (÷ 1,000,000,000), ATK and DEF as Millions (÷ 1,000,000)
- Round appropriately (2 decimal places for Billions, more for Millions)

### Step 5: Wire Up Tab Navigation

Update `switchTab()` to handle the new third tab with ID `stat-calc-tab`.

---

## File Changes

All changes are in **`index.html`** (single-file app):

1. **Tab nav** (~line 820): Add a third `<button>` for "Stat Calculator"
2. **CSS** (~line 7-500): Add styles for `#stat-calc-tab` and its components
3. **HTML** (after guardian tab): Add new `<div id="stat-calc-tab" class="tab-content">` section
4. **JS** (script section): Add `calculateStats()` function and input listeners

---

## Design Decisions

- **Real-time calculation** (no button click needed) — matches the Guardian calculator pattern
- **Same visual style** as existing tabs — consistent user experience
- **Input defaults to 0** for upgrade fields — user only fills in what they're changing
- **Number formatting** with locale-aware commas for readability
- **Mobile responsive** — inputs stack on small screens

# Product Vision

> **Project Name:** khronos-web  
> **Project Manager:** @ciroalo  
> **Last Revision Date:** May 8, 2026  
> **Version:**  0.1.0  
> **Status:** Accepted

## Product Overview

**Product name:** Khronos  
**Type:** Web application (browser-based, no backend)  
**Tagline:** *Precision timing for athletes* 

Khronos is a keyboard-driven stopwatch web app designed for athletes, fitness users, and students who need precise timing with rest period management. Unlike generic stopwatches, Khronos treats rest intervals as first-class citizens — giving them visual presence, configurable triggers, and clear countdown feedback. Every major action is mappable to a keyboard shortcut, enabling eyes-free operation during training.

## The Problem

Athletes timing themselves — runners tracking intervals, lifters timing rest periods, coaches logging lap splits — use generic stopwatch apps that were designed for general audiences. These tools:

- Require clicking small buttons while active or sweaty  
- Offer no rest period management beyond a separate timer  
- Give no visual urgency cue when rest time is nearly over  
- Lose lap history on page reload  
- Cannot be operated without looking at the screen  

## The Solution

Khronos is built around three ideas:

1. The timer is the hero. A massive, responsive display dominates the screen. Nothing competes with it.
2. Rest periods are visual. An inward-shrinking gradient communicates urgency without text or sound. When rest expires, it blinks — then disappears cleanly.
3. The keyboard is the controller. Every action has a configurable key binding. Key labels appear on their corresponding UI buttons so the mapping is always visible.


##  Target Users

Primary: Athletes, fitness users and students

## Platform Targets

- Desktop browser (primary experience — sidebars as drawers but visible at start, milliseconds visible) 
- Mobile browser (responsive layout — sidebars as drawers, timer fills screen)
- No native app, no backend, no login (v1)

## Feature Summary

### Must have (MVP)

| Feature | Description |
|---|---|
| Stopwatch | MM:SS with centiseconds; hours appear small above when needed; milliseconds gets under main timer gracefully on small screens |
| Start / Stop / Resume | Single smart button, three states; Restart button appears when paused |
| Lap tracking | Manual lap capture, auto-numbered; circuit label per session |
| Rest timer system | Multiple configurable rest presets; key-triggered; stacks (adds) if triggered during active rest |
| Rest gradient overlay | Radial gradient shrinks inward as rest depletes; blinks 10s at zero; auto-dismisses |
| Clear rest | Button and key binding to dismiss active rest |
| Keybinding system | All major actions rebindable; shown on buttons; saved to localStorage |
| Settings sidebar | Rests tab, Keys tab, Log tab; collapsible |
| Laps sidebar | Lap list with duration, delta, split; collapsible |
| Export | CSV and JSON export of lap data |
| Session persistence | Keybindings and lap history saved in localStorage |
| Help panel | Glassmorphic overlay; explains app and default keys |

### Out of Scope (v1)

| Feature | Reason |
|---|---|
| User accounts / login | Far-future; not needed for solo use |
| Audio cues | Intentional design decision — visual only |
| Backend / cloud sync | No backend in v1 |
| Multi-athlete tracking | Future coach feature |
| Per-circuit statistics | Lap circuit labels captured now; stats computed later |
| Mobile native app | Browser-only for now |
| Battery / system status | Filler chrome; rejected |


## Design North Star

"The Kinetic Monolith." The timer is a physical, imposing presence. The UI is dark, restrained chrome around a dominant central display. Hierarchy comes from tonal surface stacking, not borders. Every pixel that isn't the timer earns its place.
Visual language: Dark void canvas (#05080a), mint green accents (#afffd1 → #00ffab), indigo rest state (#9aa3d8), Space Grotesk for the timer, JetBrains Mono for badges and lap data.

## Interaction Model

```
Run states:  idle → running → paused → running …
             Reset → idle (persists session to Log if it had data)

Rest states: null → active (countdown + gradient) → zero (blink 10s) → null
             Stacking: new rest trigger while active → adds to remaining time
             Clear: key or button → immediate dismiss
```

### Default Key Bindings

| Key | Action |
|---|---|
| Space | Start / Stop / Resume |
| L | Lap |
| R | Reset |
| C | Clear current rest |
| 1–5 | Trigger rest preset 1–5 |
 
All rebindable. Disabled when focus is in an input.


## Responsive Behavior

| Viewport | Timer display | Sidebars | Milliseconds |
|---|---|---|---|
| Desktop (≥900px) | MM:SS + centiseconds inline | Persistent columns | Inline after seconds |
| Tablet (600–899px) | MM:SS + centiseconds below colon | Slide-in drawers | Below the colon separator |
| Mobile (<600px) | MM:SS fills screen edge-to-edge (slight bleed) | Slide-in drawers | Hidden |
| Any — once hours needed | HH appears small above MM:SS | — | Same rules as above |


## Data Persistence (localStorage)

| Key | Contents |
|---|---|
| `khronos.keybindings` | User keybinding map |
| `khronos.restPresets` | Array of `{ key, durationMs, label }` |
| `khronos.sessionLog` | Array of past sessions `{ date, totalTime, laps[], circuit }` |
| `khronos.sidebarState` | Left/right open-closed state |
 
No sensitive data. No user account needed.

## Success Criteria

- [ ] Stopwatch runs accurately with no drift over a 1-hour session
- [ ] Rest gradient is visually clear and dismisses correctly at zero
- [ ] All key bindings are rebindable and persist across page reloads
- [ ] Lap data survives page reload
- [ ] Export produces valid CSV and JSON
- [ ] App is fully usable without a keyboard (touch/click only)
- [ ] App is responsive on mobile Chrome and desktop Chrome/Firefox/Safari



# ChainReaction - progress

## Current step
Step 0 complete. Next session does Step 1 only (design system, both themes, contrast, theme toggle).

## Done
- **Step 0** (20 Sep 2026): file-timestamp evidence exported to `../chainreaction-file-dates.csv` (both local copies captured; Desktop copy retains the 19 Sep creation times and is now the repo). Git initialised on `main` with `.gitignore`, first commit of all prior work, public GitHub repo created, pushed, Pages enabled. CODE MAP written below.

## Next
- **Step 1** - design tokens, Ink/Paper themes, type, header theme toggle, restyle home + quick check + full assessment, fix contrast.

## Known bugs
- Dark text on dark backgrounds in places (brief calls this out; fix in Step 1).
- `index.html` lines 7-9 load Inter + JetBrains Mono from Google Fonts CDN. This breaks the "no network calls" privacy claim and must be replaced with self-hosted .woff2 or system stacks in Step 1.
- Only one theme exists (dark/clay). No theme toggle, no `prefers-color-scheme` default.
- Palette is clay orange + sage, not the briefed raspberry accent / teal-sage counter-colour.

## Deferred
- Step 7 (rule-based "Ask the simulator" chips) - only if Steps 1-6 are tested before 3:30 PM. Note: a rule-based assistant named **Cipher** already exists (`setupCipher`, `cipherRespond`, FAQ/GLOSSARY/KEYWORD_MAP in data.js); Step 7 may reduce to relabelling and adding question chips rather than new code.
- PNG export of the Group Chat Card (Step 4) - text copy + print layout first.

## CODE MAP
Four files, repo root, no build step. Load order: `data.js` then `app.js` (both plain `<script>` at end of `index.html`).

**index.html** (556 lines) - single page, three `<section class="page">` panels swapped by JS: `#pageHome`, `#pageQuick`, `#pageFull`. Header `.topbar` has `.brand` + `.sitenav` buttons carrying `data-page`. `#pageFull` holds three `.view` panels: `#viewProfile` (situation/persona/habit pickers, `#surfaceMap`, run bar), `#panelChain` (`#resultsEmpty`, `#analyzing`, `#resultsChain` with `#pathList`/`#chainFlow`/`#nodeDetail`/`#severityCard`/`#interventionList`/`#weakestLinkPanel`, `#resultsNoChain`), `#panelThreat` (`#threatBars`). Below: `.site-footer`, two modals (`#howModal`, `#libraryModal`, both carousel-based), and the Cipher chat widget (`#cipherPanel`, `#cipherForm`, `#cipherToggle`). Nearly all content is rendered by JS into these IDs. Lines 7-9 = the Google Fonts CDN links to remove.

**data.js** (754 lines) - all content, no logic. `HABITS` (line 14, ~30 entries; each has `id, label, shortName, stage, role` (entry/weakness/signal), `primaryThreat, entryStep, entryConsequence, attackerView, fixAdvice`). `SEVERITY_WEIGHTS` (307), `GENERIC_FINAL_IMPACT` (323), `STAGE_META` (326), `THREAT_ARCHETYPES` (334), `QUICK_CHECK` (364, six questions), `SITUATIONS` (376), `DEMO_PROFILE` (391), `PERSONAS` (401), `KEYWORD_MAP` (434), `FAQ` (466), `GLOSSARY` (491), `CHAINS` (512, hand-authored named scenarios: `id, name, objective, requiredHabits, weakestLink, weakestLinkStepIndex, realWorld, steps[]`). **New scenarios for Step 3 go in `CHAINS` + `HABITS` here, no engine changes.**

**app.js** (1880 lines) - one IIFE, in-memory `selected` Set, nothing persisted. Rules engine: `findNamedChains` (345, hand-authored chains win) -> `assembleGenericChain` (352, generic assembler) -> `buildPaths` (395) -> `computeInterventions` (460) -> `runSimulation` (500) / `executeChain` (545). Scoring: `computeSeverity` (904), `renderSeverityCard` (929). Rendering: `renderSurfaceMap`/`updateSurfaceMap` (65/116), `renderHabitGrid` (243), `renderFindings` (594), `renderPathList` (617), `renderSelectedPath` (648), `renderChainFlow` (972), `renderInterventions` (784) + `simulateIntervention` (835), `setupWeakestLink` (1018), `renderThreatModel` (1056), `renderHomeScenarios` (1563), `renderHeroMap` (1600). Perspective (my/attacker) toggle: `attackerLineFor` (666), `applyPerspective` (677), `setupPerspectiveToggle` (708). Node detail: `showNodeDetail` (733). Routing: `navigate`/`setupRouting` (1538/1556) + `activateTab`/`setupTabs` (1817/1839). Quick check: `renderQuickCheck` (1648), `runQuickCheck` (1698). Demo: `loadDemo`/`setupDemo` (1494/1512). Motion: `setupReveals` (1396), `countUp` (1426). Shared widgets: `setupCarousel` (1328), `setupModals` (1793), Cipher `cipherRespond` (1139) / `setupCipher` (1280). Bootstrap calls sit at the very bottom of the file.

**styles.css** (3799 lines) - all theming lives in two `:root` blocks at the top: colours at line 1 (`--bg`, `--bg-card`, `--border`, `--text`/`--text-dim`/`--text-faint`, `--accent`/`--accent-2`/`--accent-soft`/`--accent-glow`, `--gold`, `--sage`, `--danger`/`--success`, `--paper*` for light editorial sections, `--stage-*` per kill-chain stage) and fonts at line 50 (`--font-sans`, `--font-mono`). **This is where Step 1's token work goes.** Later sections in file order: shell/views (87-290), modals (289), print (966), nav (1078), how-it-works (1162), surface map (1510), stage icons (1685), result tabs (1707), analyzing loader (1749), chain flow (1846), threat model (1977), Cipher widget (2087), site chrome (2505), home (2569), quick check (2844), full-assessment subnav (3035), editorial light section (3072), scroll reveal (3097), interaction polish (3121), findings/metrics (3248), path list + detail (3302), node detail (3465), interventions (3529), sim result (3631), situations + demo (3693), focus rings + reduced motion (3776+). Breakpoints are scattered per-section (420/480/560/640/720/980/1080px).

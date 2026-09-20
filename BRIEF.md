# ChainReaction - master brief
Hackathon: Elevate Women Global Hackathon 2026 (Kaggle), solo submission.
Concept: a privacy-preserving attack-path simulator. It shows how ordinary pieces of someone's digital life combine into realistic attack chains, and which single intervention breaks the most paths. Tone and concept follow my original repositioning brief. Anything not listed here is DEFERRED.

## WORKFLOW RULES (read these every session)
- ONE step per session. When a step works and is tested in the browser: update PROGRESS.md (Done / Next / Known bugs / Deferred), commit, push, print the continue line below, and STOP. Never start the next step in the same session.
- Continue line: "Read BRIEF.md and PROGRESS.md, then do the next unfinished step only. Follow the workflow rules."
- Keep context small. Read PROGRESS.md's CODE MAP first, then open only the files and line ranges you need. Never re-explore the whole codebase and never print whole files. Show diffs and one-line status updates only. Keep messages terse and don't restate the plan.
- Don't re-verify things you've already tested, don't re-read files you've just written, and keep thinking short on simple edits. Save deep reasoning for genuinely hard design or logic problems.
- No subagents, no background or loop tasks, no web searches. Screenshots only when checking a visual result, and at most a few per step.
- End every step by reminding me to run /usage. If session usage is above about 85%, stop, commit, push, and tell me exactly what remains.
- Tell me in one line what you're about to do at the start of a step.
- Commit and push after every working step. Never backdate, rewrite or fake commit dates. Real timestamps only.
- Never claim something works until you've run and tested it. If you can't verify visuals, say so.
- Model note for me: Steps 1, 2, 4 and 6 are design- and logic-heavy (Opus). Steps 3, 5 and the docs are mostly data and text (fine on a cheaper model if my usage is running low).

## DEADLINE
Today 5:59 PM Chicago time. NEW FEATURES STOP AT 4:00 PM. After that, only bug fixes, README and submission prep. By 4:00 PM the live GitHub Pages link must load and the demo path must work end to end. Anything untested by then is deferred.
Judges score four equal criteria: Innovation, Problem Relevance & Impact, Execution & Feasibility (a reliable focused prototype beats a broad incomplete one), Presentation & Demo (3-minute video). Time targets: Steps 0-1 by 2:00 PM, Step 2 by 2:30 PM, Steps 3-4 by 3:30 PM, Steps 5-6 by 4:00 PM.

## STEP 0 - Evidence + repo
1. Save file-timestamp evidence to ../chainreaction-file-dates.csv (PowerShell: Get-ChildItem -Recurse | Select-Object FullName, CreationTime, LastWriteTime | Export-Csv). Do it before any copy or move.
2. If not a git repo: git init, default branch main, sensible .gitignore.
3. First commit of everything as-is with the message: "Initial commit: local work in progress from 19 Sep (first time under version control)".
4. Run gh auth status. If the GitHub CLI is authenticated: create a PUBLIC repo chainreaction-ewgh-2026, set origin, push main, and try enabling GitHub Pages from main / root. If anything fails, print the exact manual commands for me and continue.
5. Tell me the repo URL and the expected Pages URL. Keep index.html at the repo root and use relative paths only.
Some earlier work (routing, quick check, palette, scroll reveals) already exists. Improve and extend it. Don't rebuild from scratch.

## DESIGN DIRECTION (applies to every step; it must NOT look AI-generated)
Target look: premium cybersecurity product + editorial design + subtle threat-model visualization. Think a beautifully designed security-research publication crossed with a modern product, with the attack graph drawn like a real threat-model diagram (thin lines, mono labels, dashed "trust boundary" outlines, numbered callouts).
- Layout: asymmetric and editorial. Big serif display headlines, generous whitespace, numbered section markers (01, 02...), thin rules, marginal annotations, oversized pull statements. The live attack graph IS the hero visual.
- Type: high-contrast serif for display, clean grotesk for UI, mono for labels. Self-host OFL-licensed .woff2 files in the repo or use system font stacks. NO CDN or Google Fonts requests, so the "no network calls" privacy claim stays true. List fonts and licenses in the README.
- Color: two themes. "Ink" (dark): deep aubergine-black, never pure black. "Paper" (light): warm off-white with ink text. ONE signature accent, a raspberry/rose, used sparingly and reserved for the attack path. A cool counter-color (deep teal or sage) reserved for defenses and the broken chain. Never rely on color alone. All text at least WCAG AA (4.5:1; 3:1 for large text and UI parts). Move hardcoded colors to CSS variables. Light/dark toggle in the header, defaulting to system preference. Check every page and component in BOTH themes.
- Copy voice: a smart friend who knows security. Warm, direct, a little wry, short sentences, second person. Never fear-mongering, never victim-blaming, no corporate buzzwords. No em dashes anywhere in text: use a comma, period, colon, or parentheses instead.
- AVOID these AI-template tells: neon cyan on black, purple/blue gradients, glassmorphism, gradient headline text, emoji or sparkle icons, generic shield/lock icons, a centered hero with two buttons over three identical rounded cards, uniform card grids everywhere, filler copy.
- Motion: purposeful and restrained, 150-300ms easing. Path drawing via stroke-dashoffset, a small signal pulse along the active attack path, staggered reveals. No bounce or parallax gimmicks. Respect prefers-reduced-motion (instant state changes).

## INTERACTION + HOVER SPEC
- Graph node hover/focus: highlight the node, its edges and its full chain; dim everything else to about 25%. Tooltip card: what the factor exposes and, in Attacker View, what an attacker could infer.
- Edge hover: a one-line "why this connects".
- Path trace: hovering the final node lights the whole route backwards. A step scrubber (prev/next or slider) walks the chain one link at a time.
- Factor cards: lift on hover, reveal a one-line "what this exposes" preview, clear selected/unselected states, visible focus rings.
- Intervention cards: hovering PREVIEWS the effect (paths it would break go dashed and faded, "-N paths" badge); clicking commits.
- Hero graph: nodes lean a few px toward the cursor (off under reduced motion).
- Links and buttons: an underline wipe or similar micro-interaction, no generic glow.
- Touch: tap acts as hover. Keyboard: focus does everything hover does. Tooltips are dismissible with Esc and never trap focus.

## STEP 1 - Design system, contrast, theme toggle
Implement the design direction: tokens, both themes, type, toggle. Restyle the existing home, quick check and full assessment to match. Fix all contrast problems (dark text on dark backgrounds is a known bug).

## STEP 2 - The 30-60 second demo path (must be flawless)
- One-click "Try the job-seeker scenario", also via ?demo=1.
- The chain renders: LinkedIn -> Employer -> Job search -> Recruiter impersonation -> Interview link -> Credential harvesting.
- MY VIEW / ATTACKER VIEW toggle changes text and visual state.
- "Why is this connected?" explains each link.
- "Simulate MFA": before/after, count of removed paths, ending on "The chain broke."
- Label demo data on screen: "Fictional example - no real data".
- All hover and trace interactions work on this graph.
When this passes, tell me so I can record a backup video.

## STEP 3 - Women's safety layer (data only, no new engine code)
- New scenarios as data: (a) recruiter impersonation, (b) shared accounts and location history with an ex-partner, (c) online harassment and doxxing from public info, (d) solo commute or travel with live location sharing, (e) meeting someone from a dating app. Each has factors, chains and interventions that fit the engine.
- The homepage names the audience: everyday people, especially women, who face targeted harassment, stalking, impersonation and scams.
- "Quick exit" button on every page (top right; Escape twice also triggers it): location.replace to https://www.wikipedia.org, with a one-line note that it's for shared or monitored devices and does not clear browser history.
- Support note: "If you are in immediate danger, contact your local emergency services." Do NOT invent hotline numbers or organizations.
- Everything is defensive. Attacker view is educational (what could be inferred) and never gives instructions for surveilling, locating or contacting a real person.

## STEP 4 - USP: "The Circle" + "Group Chat Card"
Most tools model attacker vs. accounts. Real risk for many women runs through people: an ex with a shared cloud login, a roommate's Wi-Fi, a family location-sharing plan. ChainReaction models trust relationships too.
- The Circle: concentric rings around "You" (inner = closest). The user places ROLES only (partner, ex-partner, roommate, family member, coworker or manager, friend, online acquaintance, recruiter), never names or contact details. Each role can hold shared-access factors (shared cloud account, family plan or location sharing, shared device or Wi-Fi, group-chat membership, saved passwords on a shared device). Paths running through a person are flagged "Circle paths".
- Hover a role: the access it holds. Hover a factor: which people can reach it. Insight lines are neutral and defensive, e.g. "2 paths run through your ex-partner's access."
- Interventions: "Revoke shared access", "Turn off location sharing", "Separate the family plan", each reducing Circle paths through the same preview-then-commit interaction.
- Group Chat Card: a friendly shareable summary in the warm copy voice (the chain, the top 3 fixes, one line inviting a friend to try it), with no personal data. Copy-text button and a print-friendly layout. PNG export only if time allows.
- Everything in memory. No accounts, uploads, storage or network.

## STEP 5 - "Build your own" situation (lean)
The user picks a context, toggles factors, and can add a custom factor by choosing a name, category and what it exposes from the engine's existing tag vocabulary via dropdowns/chips (no free-text parsing). Custom factors participate in chains and interventions. Keep scenarios as one-click shortcuts. Use the empty, no-match and "Assembling your attack path..." states from the original brief.

## STEP 6 - Animation and polish
Chain drawn node by node, path-removal effect on intervention, exposure map updates, Replay button. Tune easing and timing so it feels crafted.

## STEP 7 - Rule-based "Ask the simulator" guide (ONLY if Steps 1-6 are tested before 3:30 PM)
Question chips ("Why is this connected?", "What should I fix first?", "What does the attacker see?") answered from the engine's own data. NO LLM or API calls or network requests. Label it "Rule-based guide - not an AI chatbot and not professional advice." If time is short, skip it.

## NON-NEGOTIABLES
- Vanilla HTML/CSS/JS, no new runtime dependencies, no build step, relative paths only (must work on GitHub Pages as-is). Fonts self-hosted or system.
- Keep the deterministic rules engine. Everything runs in the browser: no accounts, uploads, scraping, network calls, or storage/transmission of personal data.
- Never claim the user is protected. Footer on every page: "Educational simulation - not professional security advice and not a guarantee of protection."
- Keyboard navigation and reduced-motion support on all UI. Mobile layout must work for Steps 1-5.

## DOCS (write by about 3:30 PM, in one short session)
- README.md: problem and audience, what it does, differentiators (attack CHAINS plus people-aware risk via The Circle), how to run it (Pages link or open index.html), privacy model, limitations, fonts and licenses, deferred ideas, and a "Built during the hackathon" section: development began 19 September 2026 (within the Hackathon Period); first committed to version control 20 September 2026; earlier work was local and is documented by file timestamps and Claude Code session logs available on request; used Claude Code (AI coding assistant) for coding, design and testing; I directed the decisions and am responsible for the result; third-party resources: list them or say none; pre-existing work: none.
- DEVLOG.md: dated entries of what was built and with which tools.
- MIT LICENSE.

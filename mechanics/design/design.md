# The visual: how the things people look at get made

Applies whenever the human asks to make or remake something **visual**: a site, a landing page, a page, a screen, a presentation, a slide, a cover, a diagram, a layout, a post, the styling of a report. And on requests like "give me some design options", "look at how this reads", "make it feel more solid / livelier / simpler".

**It does not create taste where there is none — it makes taste reproducible and checkable.** It carries no promise that "it will look good": the human is the judge.

**No brand identity is required up front.** Its absence is the normal case: the first pass always runs without one. `BRAND.md` is created only by the human's consent and only after acceptance (§6) — it never appears on its own.

**With the code mechanic attached, there is ONE cycle — its own.** The visual does not open a second gate and does not add subagent rounds: it inserts two things — the design read into the task statement, and a visual pass into the acceptance criteria. Ownership is split: code owns files and execution (implementation, tests, deployment, secrets, performance, responsive code, accessibility markup), the visual owns the decision and acceptance by eye (the read, the direction, composition, typography, colour, rhythm, the words on the surface, contrast, type size, tap-target size). No code mechanic — the same five steps without tests and deployment.

---

## 1. The design read — one line before any work

> "Reading this as: `<what we are making>` for `<who will see it>`, the job is `<persuade / explain / show the work / let them operate / be recognised>`, tone `<…>`."

- The list of jobs is **closed**. None of them fits — the artifact is not needed: **"zero" is a legitimate outcome.**
- The line is written **as a comment inside the source itself** (an HTML comment as the first child of `<body>`, a note on the first slide, a file header). It survives a dropped chat and a context compaction, and it can be grepped for.
- The brief is ambiguous — **one clarifying question, not a pile of them**. It can be inferred confidently — do not ask; just state the read.
- **Do not ask** about colours, fonts or "aesthetic directions". Ask about the material: who, why, what has to happen, what must not be touched.

**Surface mode** (what success means here): `persuade` — the visitor decides and acts; `let them operate` — they complete a task, and familiar affordances outrank expression; `explain` — they understand, structure and wayfinding come first; `show the work` — they are inside the work itself and the interface recedes. The mode comes **from the surface, not from the product**: a tool's landing page is still "persuade".

---

## 2. What is already true

Read `BRAND.md`, the actual files, the assets. Determine the state **from evidence, not from whether a file exists**:

| State | What we do | What happens to §3 |
|---|---|---|
| No visual authority | create the world together with the human | full |
| A world has settled (lives in the files, no `BRAND.md`) | inherit and document it, don't replace it with invention | dropped; options come from layout inside the world |
| Incomplete brand (logo and colour exist, no deck) | preserve what is confirmed, **extend the system** onto the new surface | runs only on the missing axes |
| Redesign | the old look is **replaced, not polished** | full |

- The old look is **evidence of what the subject is, not authority over what it becomes**. Forbidden: splitting the difference into polish on the discarded look.
- A section or component inside a settled surface **inherits it**; a local addition does not turn into an identity exercise.
- Before a redesign, separate the **real commitments** (logo, a pinned colour, promises, proof, legal) — they survive it — from what merely accumulated.

**The brief wins.** Pinned aesthetics, eras, materials, fonts and palettes are honoured even when they conflict with a warning about a saturated pattern. Redirecting a clear brief toward your own taste is failure. The guessability test (§3.4) does not apply to a pinned brief. But **a pinned world pins the world, not its softest rendition**: an execution that matches what any model ships for that world failed the self-check at execution.

**Someone else's system:** do not import its tokens and then override 90% of them; do not mix two systems in one tree; if the brief reads as a specific existing system, install the official package rather than recreating its CSS by hand.

---

## 3. Direction — this is where quality is produced

**3.1. Name the rut and throw it out.** Before the list, say out loud what this category always ships and its predictable opposite. Both are out of the running. The brief paints its own picture (a name, a metaphor) — spend at most **one** candidate on its literal reading.

**3.2. Derive seven candidates from the audience's world** — not from a catalogue of interface devices. Visual systems, artifacts, places, rituals, publications, notations, data graphics the audience knows by heart. Two levers: **what would this thing look like as a physical object; what did its world look like before the web.** Each with one line on why it resonates. Near-duplicates count once. **More than three of the seven in one material family means the derivation stopped at the obvious; dig until at least three families are covered.**

**3.3. Present them — and not your own top pick.** Rank internally, **build not #1**: the top-ranked option is what every run would ship. The external source of choice here is the human.

- **Three by default.** The human named a number ("give me five") — **their word beats the rule**; then five genuine worlds, not three plus two clones. Not enough of them — say so plainly.
- Format — a table with identical anatomy: **direction · axis · thesis · world · first viewport · what it costs**. Names by substance ("Quiet", "Editorial", "Field notebook"), not "option A/B/C".
- The thesis also names **the category-default arrangement the direction refuses**. The world is recognisable with all content removed. "What it costs" is an honest risk, not marketing.
- **Two options that differ only in accent colour or caption are one option.**
- **Name the remaining candidates, one line each.** The human must see what the choice was made from, and be able to ask for a re-roll.
- **The standing exit is always on the table:** the category standard, played straight and at full fidelity, without irony. It is the human's door — never recommend it and never weigh it against the rest. They take it — ask which two or three products this should sit alongside, and execute the canon fully.
- **Compare options as a set, in one frame** (same size, side by side). Otherwise you are comparing rendering luck, not composition.
- **Stop before code.** Present and halt; then build **the chosen one**, not a safe interpretation of it.

**3.4. The guessability test.** The model converges on recognisable clusters regardless of subject. Where the aesthetic is free:

> **Could someone guess your choice knowing only the product's category — or "the category plus avoiding it"? If yes, rework it.**

A warm, bookish or child-facing subject does **not** soften the calibration: every world's full material range is in play, and its softest corner is the same default wearing the subject's clothes.

**3.5. Colour strategy before colours.** One of four: **restrained** (neutrals plus one accent; the default for "let them operate" and "explain"), **committed** (one saturated colour carries 30–60% of the surface), **full palette** (3–4 named roles), **drenched** (the surface is the colour). Colour commits at page scale: fields that own whole regions, not accents scattered over a neutral ground. Light or dark is **never a default by category** — write one sentence of physical scene (who, where, under what light) and let it force the answer.

---

## 4. The quality bar

**Loaded immediately before building**, not for planning. Hierarchy: **the human's words > `BRAND.md` > this floor > the model's habit.** A value here is copied, not approximated; what is not here is judged by eye, and then you say so rather than inventing a number.

**Every rule below states how it is verified: `[measured]` — it can be counted, `[eye]` — only by a human.** No rule should be without that tag: written down and verified by nobody, it creates the illusion of control.

### Verify

- `[measured]` **Contrast:** body text and placeholders ≥ 4.5:1, large (≥18pt) ≥ 3:1. On coloured surfaces, tint secondary text from that hue, **never grey**.
- `[measured]` **Line measure** — 65–75 characters, ceiling 78. Computed as block width divided by average character width; for Cyrillic that width is **greater than for Latin**: ≈ 0.545 of the type size for a grotesque, ≈ 0.525 for a serif. The Latin 0.5 understates it and lets violations through.
- `[measured]` **Does it fit:** first-viewport content ≤ frame height; text ≤ its own block height; nothing past the edges; text does not overlap text or images; **gutter from an image ≥ 0.25″**. **An inserted image's size is set by the file, not by intent** — offsets are computed from its actual dimensions.
- `[measured]` **Type size — by the text's role.** Body and utility labels (running head, page number, block label) have **different** minimums. A single minimum produces noise that drowns the signal.
- `[eye]` **Elevation is declared once** — border OR shadow. A 1px border under a wide soft shadow is the ghost card. Shadows carry an offset and a soft blur; a zero-offset coloured halo is decoration.
- `[eye]` **Rhythm:** tight groups, generous separation, **more space above a heading than below it**; one spacing step across the surface.
- `[eye]` **Hierarchy is built from three things** — weight plus size plus leading, not size alone.
- `[eye]` **The browser surfaces you did not draw:** text selection, the caret, scrollbars, the focus ring, underline offset, tabular numerals. They arrive with defaults belonging to no design system. The cheapest signal that a thing was built rather than assembled — and the one most reliably skipped.
- `[eye]` **States:** hover, disabled, loading, error, empty; real content, working controls, keyboard focus.
- `[measured]` **Buttons:** the label reads against its own background; a primary button's label fits on one line, 1–3 words; **two calls to action with the same intent on one page is a defect** (one label per intent, everywhere).
- `[eye]` **Forms:** label above the field, error below it, a placeholder **never** replaces a label.
- `[eye]` **Copy is the product's own language.** Controls name their action, errors name the problem and the way out. Before shipping, re-read every visible string and rewrite what is broken, unclear, or sounds like a model trying to seem profound. **Boring copy beats cute invented copy.**
- `[eye]` **Numbers:** from real data, or explicitly labelled as illustrative. Invented engineering precision is a defect.
- `[eye]` **Coverage:** every brief requirement is present and findable within seconds.
- `[eye]` **The source has been re-read.** A rule that does nothing (overridden, never applied) is either a typo or dead code; it may not show up visually and will travel onward.

### Refuse

**These are the category's defaults, not bans: the brief's own words can earn any of them. Reaching for one when the axis is free means you were not deciding.**

Page scaffolds: same-size cards of icon plus heading plus text as the page structure (the card is the lazy container; **nested cards are always wrong**) · the big-number-plus-label-plus-stats template · section numbering where the sequence carries no information · `[measured]` no more eyebrows than `ceil(sections / 3)` · `[measured]` no more than two consecutive image-plus-text blocks · `[measured]` one layout family appears once per page (8 sections → at least 4 families) · "big heading left, small paragraph right" as a section header.

Surface habits: gradient text (emphasis comes from weight or size) · glass and blur as decoration rather than a specific effect · a coloured side rule thicker than 1px · hard zero-blur shadows outside a world that is genuinely neobrutalist · sparklines and soft-shadowed rounded rectangles **standing in for content** · monospace as a costume for "technical" · **emoji and glyphs instead of an icon system** (icons are drawn: a real library or authored SVG, one stroke weight, one family per project) · a system display face as an own-world page's voice · sketch-style "illustrations" and decorative grain (**a ban on SVG imitating a picture; never a ban on SVG doing geometry**) · fake screenshots built from blocks · textured backgrounds with no real canvas under them · category labels under logos in a trust wall · `[measured]` one theme per page · `[measured]` one corner-radius language · `[measured]` the accent is locked for the whole page.

### Examples as of 2026-08-11

> **Normative is the guessability test (§3.4). Below are the tells of this version's date; they go stale and are revisited at every bump of this mechanic.**

- **Faces that mean "you stopped looking":** Fraunces, Instrument Serif, Playfair Display, Cormorant, Lora, Crimson, Newsreader, Syne, Space Grotesk, Space Mono, IBM Plex, Inter-as-display, DM Sans, DM Serif, Outfit, Plus Jakarta Sans, Instrument Sans. You may still take one, but it needs a reason no other face satisfies, and **an association with the subject is never that reason** (books want a serif, tech wants a mono — those are exactly the associations this list exists to break).
- **A serif display by default** on a "creative" brief is the most-tested tell of machine work.
- **The "premium consumer" palette:** grounds `#f5f1ea` `#f7f5f1` `#fbf8f1` `#efeae0` `#ece6db` `#e8dfcb`; accents `#b08947` `#b6553a` `#9a2436` `#bc7c3a`; text `#1a1714` `#1b1814`. Rotate the alternatives: cold luxury · forest (green + bone + amber) · black and tan · cobalt and cream · terracotta and slate · monochrome plus one bright pop.
- **The three clusters generation converges on:** warm paper plus a high-contrast serif display plus a terracotta accent; near-black with one neon and glowing edges; broadsheet hairlines plus an italic display serif plus tracked mono labels.
- **Violet-blue gradients and a glow under the button** as the language of "technology".
- **Compositional tells:** a centred hero over a dark mesh; three identical feature cards; glass on everything; infinite micro-animations everywhere.
- **The first viewport:** fits entirely; heading ≤ 2 lines; subtext ≤ 20 words; ≤ 4 text blocks; top padding ≤ 6rem. The micro-note under the CTA, the trust strip, the pricing teaser, the bullet list — **all move to sections below**.
- **More than five homogeneous items is not a longer list but a different component** (two grouped columns, cards, tabs, horizontal scroll, a marquee). A spec sheet with a hairline under every row is the worst default.
- **Motion:** `ease-in` on UI — never; animation under 300ms; `scale(0)` — never, start at 0.9–0.97 plus opacity; animate only `transform` and `opacity`; popovers scale from the trigger, modals from the centre; an element seen 100+ times a day — **do not animate at all**; stagger 30–80ms; curves `cubic-bezier(0.23, 1, 0.32, 1)` for enter/exit and `cubic-bezier(0.77, 0, 0.175, 1)` for on-screen movement.

---

## 5. Acceptance — two bounded batches, not a loop

1. **Render it and look.** One pass, all frame sizes together, in a single batch.
   > **A screenshot shows you what to look at; it proves nothing.** Overflow, clipping, overlap and not-fitting are **measured, not eyeballed**: in a headless browser the window size and the layout viewport are different things, and a mobile capture regularly shows clipping where there is none. Before calling it broken — measure it, or open it on a real device. **Reporting a defect that does not exist is worse than missing one that does:** the first destroys trust in the whole acceptance pass. Nothing to measure with — say "this needs checking", not "it's broken".
2. **Name what already works** — a short list of what was done right and **what must not be broken**. Without it, acceptance turns into a demolition and the human is left with no line between "fix" and "leave alone".
3. **One batch of fixes** covering everything found.
4. **At most one confirming pass. Stop.**
5. **A fresh lens** — a subagent that **does not inherit the build conversation**: one that does inherits your framing, your optimism and your abstractions. It gets the original request, the human's answers, artifact paths, renders, the design-read line, and this floor. No subagents — the same pass inline, **disclosing the substitution in one line of the report**.
6. **The verdict is one word from a closed list:** `rebuild | fix | ship`. Quote it verbatim; softening is not allowed; a table with open items is never announced as acceptance. No more than eight fixes, most material first.
7. `rebuild` — do not patch, rebuild what was named, **telling the human** rather than asking permission to fix a failure.
8. **Stop the moment a pass resolves nothing.**

**Degradation is disclosed in the first reply, not afterwards.**

- No renderer → "checked by reading the source, not by rendering".
- No image generation → hand over the markup, SVG or a frame specification and **say so immediately**.
- A format the environment cannot display (a `.pptx` with no converter, say) → **acceptance becomes measurement**: fit, overlaps, gutters, line measure, type sizes by role, contrast. The result is phrased as **"no violations found"**, not "it looks good", and **what remained unchecked is listed**.
- **When the environment cannot render, the human becomes the renderer.** That is a normal step, not a failure: hand over the file, ask them to page through it, and take what they find as the acceptance result. What they find is **not patched in place** — it is turned into a check and run across the whole job: one human finding almost always exposes a class.

---

## 6. Recording — `BRAND.md`, and only by consent

**The file is never created on its own.** A one-off good-looking presentation does not have to become the project's brand identity: it may contain not a single device the human wants repeated, and quietly turning it into a rulebook imposes a commitment they never made.

**The offer is made once, after the human has accepted the final version** — not at the start and not mid-work:

> "If you'd like future work to follow the same style, I can save a brand book — I'd record the style you just approved and hold it for future presentations, landing pages and anything else visual. Shall I?"

- **Declining is a normal outcome.** Do not ask again on the same artifact and do not return to the subject along the way.
- **A second visual artifact is a fair occasion to offer again**, and now the argument is concrete: "this is the second one; with no style on record I choose afresh each time, and they will drift apart".
- **The human brought a brand identity themselves** (a logo, brand book, guidelines — the catcher in [ingest.md](ingest.md)) → the file is created right away: that act is the consent, and there is nothing to ask twice.
- No file — we work without one. That is the normal mode, not a deficiency.

A root-level file in the project. A third axis of memory: `wiki/` — what we know, `STATE.md` — what we are doing, **`BRAND.md` — what the things we make look like**. Deliberately not a wiki page: the wiki is maintained by Claude through ingest and requires sources, whereas `BRAND.md` is edited by the human and is itself the source.

Sections: who we are and what we promise (2 lines, taken from "About the project", not invented) · palette by roles (ground · text · muted · accent) · typography (faces, scale, leading) · rhythm (spacing step, radii, shadows) · the recognition device (1–2 moves) · **named rules** (2–5, in the form "**The One Voice Rule.** The accent never covers more than a tenth of the screen; its rarity is the point" — a named rule can be cited in conversation and in acceptance) · **what we don't do** (the project's own ban list, grown from experience, separate from §4) · what each artifact type is built with · `_Updated: YYYY-MM-DD · after <which artifact>_`.

Two iron rules: **written after building, from what was built** (a rulebook written in advance gets defended against reality instead of used); **a breach of the floor is never canonised into the system** — one run shipped five eyebrows, that does not make them the style. A single surface's strategy (who exactly this deck is for, which one action) is **not** promoted here — it lives in the design-read line inside the artifact.

---

## 7. Surfaces

**Landing page.** One goal-action. The first viewport answers "what is this and why" without scrolling and **demonstrates the mechanism rather than describing it**: show the subject at work, the specifics a competitor could not copy-paste. The memory test: someone left after one viewport — what would they describe an hour later? An honest answer of "a mood" means the concept has not committed. The scaffold test: **remove all the copy — the structure must still say what this block is.** **A section heading is an assertion, not a topic:** "What's inside", "Pricing", "FAQ" are a table of contents; "Eight things a normal chat cannot do" works. Check **every** heading, not the first. Navigation on one line, height ≤ 80px. Pace the scroll like a studio: dense passages alternate with quiet ones, and the page ends anchored by a real close.

**A set of graphics.** **Medium inventory:** every visible region gets a means of production — raster · vector · code · an existing asset · **a deliberate refusal**; an unrecorded element gets silently dropped in the build, which is how "we approved a rich sketch and got a flat block" happens. **Scope lock by count:** N frames fixed before the start and reconciled against the actual count before delivery. **Consistency lock as two lists:** must match — palette, faces, spacing step, image treatment, tone of voice, mark placement; may vary — composition, density, focus; the over-salting test — **it stopped reading as one set**. One artifact = one file, no collages and no crops out of a shared board. Acceptance views the set **side by side**, as a contact sheet. Diagrams: the conclusion goes in the title, honest axes, no 3D.

**Presentation.** **The first question: will there be a speaker, or does the deck travel as a file?** These are two different decks, and it also sets the metric.

- **Slide roles from a closed list**, assigned before production: cover · problem · solution · how it works · proof · numbers · comparison · risks · plan · next step · section · close. **A slide without a role does not get made** — that is what cuts a deck from 22 to 13.
- **The heading is an assertion, not a topic:** "Revenue rests on two clients", not "Revenue". **One thesis per slide.**
- **Loudness rhythm:** a deck is never uniformly loud. The expression budget goes to the cover, the insight slide and the close; **whatever repeats on every slide** (running head, mark, plate) **must be quieter**.
- **The layout is approved at one point — a text outline before production**, not through fifteen drafts.
- `[measured]` **Type sizes depend on the medium.** Projection: body ≥ 18–20pt (viewing distance). A file read on a screen: body 12.5–16pt, utility labels ≥ 10pt. Demanding projection sizes from a file deck squeezes the text out of the frame. Also: heading ≥ twice the body; ≤ 4 meaningful elements per slide; heading ≤ 2 lines; a table of ≤ 5 rows (more than that is a handout, not a slide).
- **Refuse:** eight bullets · a centred paragraph · 3D charts · default shadows · a stretched logo · clip art and the stock "team in a meeting room" · a gradient under everything · **a decorative line or colour bar under the title** · reading the slide aloud verbatim.
- **Fonts for a file that goes outside:** system faces or embedded ones. A font the recipient does not have is substituted silently, and the deck arrives broken. A fallback declared by the project itself is a legitimate source of choice.
- **Four acceptance lenses, each required to name a slide number:** someone paging through without a speaker · someone who sees exactly one slide (it was forwarded) · someone printing in black and white · someone watching from the back row.
- **The build format is not dictated** — build in whatever the human can open and edit themselves; record the choice in `BRAND.md`.
- Fifteen slides is **a long pass**: progress to disk, an "X of Y" marker, a break at the ninth leaves nine.

---

## 8. Boundaries — not for this, go there

- **Print production** (colour separation, trapping, bleeds, spot colours) — no.
- **Video and editing** — no.
- **Dense product interfaces** with tables, filters and bulk operations — only the floor applies here; this mechanic offers no depth on them.
- **Motion as implementation** (curves, springs, performance, responsive code) — that is the code mechanic's axis. Here, only the decision "should this animate at all, and why".

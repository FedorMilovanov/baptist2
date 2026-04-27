# TRANSFER - Russian Baptist History Map

This file is the handoff document for a new AI session. It describes the current repository, architecture, source policy, known issues, and the exact workflow we use in long Arena sessions.

## Project Goal

Interactive historical website about Russian / Russian-Ukrainian Evangelical Christian-Baptist history.

Main product goals:

- visual mind-map of Russian Baptist origins and later unions;
- historically verified timeline;
- compact tables for memory and study;
- built-in quiz with explanations and sources;
- source-first historical workflow using Russian Baptist and archival sources;
- premium dark visual style with gold accents;
- 3D relationship map, but with 2D/outline fallback planned.

## Current Stack

- React 19.2.3
- TypeScript
- Vite 7.2.4
- Tailwind CSS 4
- Framer Motion
- lucide-react
- react-force-graph-3d
- three
- three-spritetext
- @xyflow/react is installed but not currently the main map implementation
- @google/generative-ai is installed for optional AI assistant
- vite-plugin-singlefile is configured through the project setup

## Current Dependencies

From `package.json`:

```json
{
  "@google/generative-ai": "^0.24.1",
  "@xyflow/react": "^12.10.2",
  "clsx": "2.1.1",
  "framer-motion": "^12.38.0",
  "lucide-react": "^1.11.0",
  "react": "19.2.3",
  "react-dom": "19.2.3",
  "react-force-graph-3d": "^1.29.1",
  "tailwind-merge": "3.4.0",
  "three": "^0.184.0",
  "three-spritetext": "^1.10.0",
  "zustand": "^5.0.12"
}
```

If 3D does not render in a future environment, first verify `react-force-graph-3d`, `three`, and `three-spritetext` exist in dependencies and are installed.

## Repository Structure

### Root files

```text
package.json
index.html
vite.config.ts
tsconfig.json
ROADMAP.md
HANDOFF.md
SOURCE_POLICY.md
EXPORT_GUIDE.md
TRANSFER.md
```

### Entry and global style

```text
src/main.tsx
src/App.tsx
src/index.css
src/config.ts
src/utils/cn.ts
src/hooks/useIntersectionObserver.ts
```

### Data files

Historical data should live here as much as possible.

```text
src/data/timeline.ts
src/data/organizations.ts
src/data/quizData.ts
src/data/mindmapData.ts
src/data/mindmap.ts
src/data/originsData.ts
src/data/baptistJournalData.ts
```

Important: continue moving hardcoded facts out of components and into `src/data`.

### Components

```text
src/components/AIChat.tsx
src/components/BaptistJournal.tsx
src/components/Comparison.tsx
src/components/Footer.tsx
src/components/Glossary.tsx
src/components/Hero.tsx
src/components/KeyFigures.tsx
src/components/LuxCross.tsx
src/components/MindMap.tsx
src/components/MindMap3D.tsx
src/components/MindMapFlow.tsx
src/components/Navigation.tsx
src/components/Organizations.tsx
src/components/Quiz.tsx
src/components/SearchOverlay.tsx
src/components/Section.tsx
src/components/ThreeOrigins.tsx
src/components/Timeline.tsx
```

`MindMap.tsx` and `MindMapFlow.tsx` are legacy / alternative map components. The active map in `App.tsx` is currently `MindMap3D.tsx`.

## Current App Layout

`src/App.tsx` renders:

1. Navigation
2. Hero
3. ThreeOrigins
4. Quiz
5. MindMap3D
6. BaptistJournal
7. Timeline
8. Organizations
9. KeyFigures
10. Comparison
11. Glossary
12. SearchOverlay
13. AIChat
14. Footer

## Working Rules for Long Arena Sessions

Use this workflow to avoid patch failures and broken context:

1. For large changes, rewrite full files.
2. For small changes, patch is allowed.
3. If a patch fails twice, stop patching and rewrite the full file.
4. After 3-5 edits, run `build_project`.
5. If a screenshot may belong to another Arena model, do not blindly react. First compare whether the UI/files match this repository.
6. Keep historical facts in `src/data/*`.
7. Keep UI components mostly data-driven.
8. Prefer SVG/lucide icons over emoji.
9. Avoid overloading 3D with labels; use progressive disclosure.
10. Do not claim a build is clean unless `build_project` succeeded.

## Source Policy

Read `SOURCE_POLICY.md` before historical work.

Core rules:

- Savinsky/Savinskii is not a reliable standalone source. Use only after cross-checking.
- Wikipedia is not a final source. It may be used only as a discovery pointer.
- Prefer journal `Baptist`, `Bratskiy Vestnik`, baptist.org.ru, baptistru.info, pravenc.ru, mbchurch.ru, archival protocols, and primary letters.

Preferred sources:

- baptist.org.ru
- baptistru.info
- pravenc.ru
- mbchurch.ru
- journal `Baptist` 1907-1929
- `Bratskiy Vestnik` from 1945 onward
- GMIR protocols, V.G. Pavlov collection
- Coleman 2007 for foreign/academic context

## Historical Facts Already Accepted

- N.I. Voronin was baptized on 20 August 1867 in Tiflis/Kura River. Primary source: Voronin letter to A.M. Mazaev, 15 March 1889, published at baptist.org.ru.
- First Russian Baptist community: 1869.
- Journal `Baptist`: decision at Rostov-on-Don congress 25-30 May 1907; first issue June 1907.
- `Baptist` editor: D.I. Mazaev first, then V.G. Pavlov; later N.V. Odintsov in the Soviet period.
- Motto of journal `Baptist`: `One Lord, one faith, one baptism` (Eph. 4:5).
- Russian Baptist Union: Novovasilievka, 30 April-1 May 1884.
- First chairman: I.I. Wieler / Viler, from Mennonite background.
- Viler had to leave Russia in 1886.
- D.I. Mazaev was chosen for leadership at the 29 Dec 1887 - 1 Jan 1888 meeting.
- ВСЕХ: created at the first All-Russian Congress of Evangelical Christians in September 1909.
- I.V. Kargel is not a founder of the movement; he is a major preacher, theologian, interpreter, and spiritual teacher.
- Kargel: according to `Bratskiy Vestnik` no. 5, 1947, born in 1849 and died in November 1939 in Lebedin, Sumy region.
- ВСЕХБ: created 27 October 1944.
- In 1946, the name was changed from `evangelical Christians and Baptists` to `evangelical Christians-Baptists` with a dash, according to `Bratskiy Vestnik` no. 2, 1946.

## Current 3D Map State

File: `src/components/MindMap3D.tsx`

Current behavior:

- uses `react-force-graph-3d`;
- imports directly, not via `require`;
- uses ResizeObserver to avoid 0x0 container bug;
- graph renders only after container dimensions exist;
- nodes are spheres;
- labels are reduced to avoid clutter;
- click node opens a top smart panel;
- click link opens relationship panel;
- hover highlights links and connected nodes;
- search by node label exists;
- filter exists;
- reset / zoom to fit / unlock pinned nodes exist.

Known 3D issues to continue improving:

- semantic zoom is not implemented yet;
- 2D/Outline toggle is not implemented yet;
- camera orbit constraints are not fully implemented;
- WebGL may fail inside Arena iframe on some runs;
- bundle size is large due to Three.js;
- current 3D may still be too heavy for low-power environments.

If WebGL is unreliable, add a 2D fallback and visible toggle.

## Known UI Issues to Watch

- Header can overlap hero cross if hero spacing is changed. Current fix: cross has larger top margin.
- Timeline center dots were previously misaligned. Current fix: timeline axis and dots share the same translate geometry.
- Section wrapper previously added extra padding; current `Section` only animates and does not add big vertical padding.
- Glossary used to be a long list; now it is a compact expandable table.
- Some old components still use older blue/purple styling; future work should unify premium black/gold design.

## Current Build Status

Last verified build succeeded with `build_project`.

Approximate build size after 3D dependencies: around 1.9 MB, gzip around 524-535 KB. This is expected due to Three.js.

## Recommended Next Tasks

1. Add semantic zoom in `MindMap3D`.
2. Add `3D / 2D / Outline` view switcher.
3. Add a clean WebGL fallback mode.
4. Move glossary terms to `src/data/glossaryData.ts`.
5. Move comparison hardcoded text to `src/data/comparisonData.ts`.
6. Add Viler, Mazaev, Odintsov as nodes in `mindmapData.ts`.
7. Add sources field to mindmap nodes and timeline events.
8. Continue extracting facts from journal `Baptist`, especially congress protocols and legal/persecution notices.

## How to Restart in a New Session

If using GitHub:

```text
Continue the project "Russian Baptist History Map".
Repository: <your GitHub URL>
First read TRANSFER.md, SOURCE_POLICY.md, ROADMAP.md.
Then inspect src/data and src/components/MindMap3D.tsx.
Work by full-file rewrites for large changes. Run build_project after changes.
```

If using text-only transfer:

Send files in this order:

1. package.json
2. vite.config.ts
3. tsconfig.json
4. index.html
5. src/main.tsx
6. src/index.css
7. src/App.tsx
8. src/data/*
9. src/components/Navigation.tsx
10. src/components/Hero.tsx
11. src/components/MindMap3D.tsx
12. src/components/Quiz.tsx
13. src/components/Timeline.tsx
14. src/components/Glossary.tsx
15. then all other components

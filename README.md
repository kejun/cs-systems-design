# Systems Explorer

A Chinese interactive guide to computer systems design, inspired by the exploration format of Protocol Explorer. It contains 27 topics: execution and performance (8), state and correctness (8), and distributed coordination (11).

计算机系统设计核心技术的中文交互式站点，涵盖执行与性能、状态与正确性、分布式协作三个部分，包含 27 个主题、54 个独立场景和 216 个演示阶段。

## GitHub Pages

Site URL: [kejun.github.io/cs-systems-design](https://kejun.github.io/cs-systems-design/).

The `Build and deploy GitHub Pages` workflow checks and builds every pull request. Updates to `main` also publish `dist/` to GitHub Pages after those checks pass. It uses Node.js 24 and the committed dependency lockfile; the site needs no external runtime service.

For initial setup, set **Settings → Pages → Build and deployment → Source** to **GitHub Actions**, then run the workflow from the Actions tab if the initial run has already finished. Site assets use relative paths so the `/cs-systems-design/` project prefix works without changing application routes.

Alternatively, enable Pages with a personal access token: add an Actions repository secret named `PAGES_ADMIN_TOKEN`, limited to this repository with **Pages: Read and write** and **Administration: Read and write** permissions, then re-run the failed deployment job. The workflow uses this token to enable Pages with the Actions build type. After the first successful deployment, the secret can be removed; subsequent deployments use the built-in `GITHUB_TOKEN`. Never commit the token to source files.

## Build and serve

```sh
npm ci
npm run build
npm run check
```

Serve `dist/` with any static HTTP server. The site needs no backend or CDN at runtime. Open `index.html` with an optional hash such as `#wal`, `#raft` or `#fencing`.

The mechanism diagram uses the actual `react-isokit@0.5.0` library (`Plate`, `Box`, `Glow`, and coordinate/path utilities), bundled with React as a small isolated rendering layer. A small elapsed-time controller moves an SVG circle along IsoKit connector coordinates. The rest of the application remains ordinary JavaScript. Dependencies are pinned in `package-lock.json`; generated JS, CSS, and upstream license notices are shipped in `dist/`.

## Teaching and interaction

Each topic includes two independent four-stage scenarios (54 scenarios / 216 stages in total), state observations, scenario-specific conceptual pseudocode, tradeoffs, pitfalls, related topics, and primary sources. A lesson is a simplified teaching model, not a production implementation or an exhaustively verified protocol.

The existing mechanism panel also includes three deeper explanations per topic and section-level reading links to [Hansimov's Chinese CSAPP edition](https://hansimov.gitbook.io/csapp/). These notes cover concrete state, execution and failure boundaries without changing the three-part navigation or diagrams. CSAPP provides the systems foundations; database and distributed protocol guarantees retain their specialist sources. The source mapping and editorial boundaries are documented in [docs/csapp-expansion.md](docs/csapp-expansion.md).

The isometric shapes represent logical participants, not four physical hosts. They distinguish processing, storage, queues, conditions, and records without encoding quantities in the illustration. Horizontal labels remain readable. Relation semantics remain controlled by the audited course data: requests/data use directed transfers; control/checks use dashed lines; rejection ends at the recipient; undelivered messages stop midway; internal operations have no transfer; comparisons have no arrow. IsoKit supplies the drawing; the application supplies playback and protocol teaching data.

The knowledge graph includes all 27 topics and highlights undirected direct relationships for the selected topic, including cross-section links. It does not imply execution order or prerequisites.

Playback: Space to play/pause (outside interactive controls), left/right arrows to navigate, Esc to reset; `/` focuses search. Speed cycles through 0.5×, 1×, 1.5× and 2×. Theme and completion marks are saved locally when storage is available. A solid circle travels once per request/data stage, including manual step selection. Pause holds the circle at its current position; resume continues from there. The final stage gets a full playback interval before stopping. Hidden-page handling pauses playback; reduced-motion settings omit the moving circle. Sound is not enabled.

## Source layout

- `dist/data.js`: audited course content and source mappings.
- `dist/mechanisms.js`: expanded explanations for all 27 topics, CSAPP foundations and section references.
- `dist/app.js`: navigation, scenario state, traces, details, and knowledge graph.
- `dist/icons.svg`: shared SVG symbols for every interface icon, including theme/playback states and graph completion marks; colors inherit the current theme.
- `src/isokit-scene.jsx`: IsoKit scene and logical participant illustrations.
- `src/isokit-model.mjs`: readable label wrapping and safe connector routing.
- `src/point-motion.mjs` and `src/travel-dot.jsx`: elapsed-time movement, pause/resume, and the SVG transfer circle.
- `src/isokit-renderer.jsx`: synchronous React bridge to the existing application.
- `scripts/build.mjs`: deterministic local bundle and license packaging.

## Verification

`node check-content.cjs` validates topic/scenario counts, references, and previously corrected content contracts. `node check-isokit.cjs` renders all 216 stages using the real library and checks participant counts, arrow meaning, interrupted delivery, playback/pause, reduced motion, projection, and label limits. `node check-motion.cjs` checks position changes, bends, pause/resume, speed, delayed frames, cleanup, and final-stage dwell. The tests do not prove the underlying protocols correct.

The preceding content audit and source boundaries are in `dist/accuracy-audit.md`. IsoKit migration checks are in `docs/isokit-integration.md`; the subsequent moving-circle correction is recorded in `docs/motion-fix.md`.

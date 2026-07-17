# Markov Chain Studio

**An interactive workbench for finite Markov chains — exact solver, analysis and simulation in a single dependency-free HTML file.**

Open [`markov-chain-solver.html`](markov-chain-solver.html) in any modern browser and you get a complete environment for working with Markov chains: a visual graph editor, exact answers computed from linear systems, stationary distributions, state classification, and an animated random walk with a Monte Carlo cross-check. No install, no build step, no network — everything runs locally in one file.

## Quick start

1. **Download or clone** this repository (or just the HTML file).
2. **Open `markov-chain-solver.html`** in your browser.
3. Pick an example from the **Examples…** menu (Gambler's ruin is a good first one), or build your own chain:
   - **Double-click** the canvas → new state
   - **Shift + drag** from one state to another → transition edge (drag onto the same state for a self-loop)
   - Click an edge to edit its probability; drag states to move them
4. Choose a **start state** and the **target set X** in the *Solve* tab — the answer updates instantly.
5. Hit **Play** in the *Simulation* tab to watch the chain run.

## Features

### Visual editor
- Double-click to add states, drag to move, Shift+drag to connect (a click-only **Connect** mode is also available).
- Inline probability editing, mouse-wheel zoom, background-drag pan, one-key fit-to-view.
- Auto-arrange into a regular polygon with smooth animated transitions.
- Undo/redo (Ctrl+Z / Ctrl+Y), autosave to `localStorage`, JSON import/export.
- Edge thickness is proportional to probability; states can be colored by the computed value.

### Solve — exact answers
- **P(X before Y)** — the probability of reaching set X before set Y, from every state.
- **E[steps to X]** — expected hitting time; closed classes that cannot reach X are detected and infinite times are reported as ∞ rather than failing.
- Linear systems are solved by Gaussian elimination with partial pivoting, and the residual `max |Ax−b|` is checked and displayed with every result.
- Missing outgoing probability can be auto-completed as a self-loop (toggleable).

### Distributions
- **Stationary distribution π** — unique for irreducible chains; for reducible chains, one π per closed recurrent class.
- Property badges: irreducible, aperiodic (with period d), ergodic.
- **πₙ over time** — an interactive line chart of the distribution across n steps, starting from the start state or from the uniform distribution.

### Simulation
- An **animated random walk** through the graph: step-by-step or continuous, with auto-stop on reaching X/Y.
- Free-typed speed from **0.25× to 10,000×** — up to 5× the token glides along edges; above that, steps run in batches (thousands of steps per second) with live statistics.
- Path trace, step counter, and **empirical visit frequencies** compared against the stationary π.
- **Monte Carlo estimator** (1,000–100,000 runs) with a 95% confidence interval, automatically compared with the exact value.

### Matrix & classification
- Edit the transition matrix directly; row sums are validated live.
- Export the effective matrix as **CSV** or **LaTeX**.
- State classification: communicating classes (Tarjan's SCC), closed/open, recurrent/transient, absorbing states, and the period of every class.

### Built-in examples
Absorbing chain · Weather (ergodic) · Gambler's ruin · Waiting for HH · Periodic cycle (d = 4).

## Keyboard shortcuts

| Key | Action |
|---|---|
| `N` | New state |
| `E` | Toggle connect mode |
| `S` / `X` / `Y` | Selected state becomes start / joins X / joins Y |
| `Del` | Delete selection |
| `F` | Fit graph in view |
| `Ctrl+Z` / `Ctrl+Y` | Undo / Redo |
| `P` | Play / pause the simulation |
| `?` | Quick guide |

## Deploying

This repository is ready for Vercel: `index.html` is the deploy entry point, while `markov-chain-solver.html` is kept as a named standalone copy.

- **Locally:** just open the file — no server needed.
- **GitHub Pages:** rename the file to `index.html`, enable Pages in the repository settings, and the app is live.

## Search setup

The production URL used for canonical tags, `robots.txt`, `sitemap.xml`, and `llms.txt` is `https://markov-chains-solver.vercel.app/`. If Vercel assigns a different production domain or you add a custom domain, update those files before submitting the sitemap in Google Search Console.

## Correctness

Self-tests run on every page load (see the browser console) and cover: hitting probabilities and expected times on an 11-state pattern-matching chain (exact fractional values), Gambler's ruin, a closed-form stationary distribution, the period of a cycle, and infinite-expected-time detection. Run them manually with `runMarkovSelfTests()` in the console.

Light and dark themes are designed separately, the chart palette is validated for color-vision deficiency in both themes, and the UI respects `prefers-reduced-motion`.

## Tech notes

- Single HTML file, zero dependencies, ~2,000 lines of vanilla JavaScript with inline SVG rendering.
- All math is implemented from scratch: Gaussian elimination with partial pivoting, iterative Tarjan SCC, BFS-based period computation, reverse-reachability analysis.
- Works offline; state persists in `localStorage`.

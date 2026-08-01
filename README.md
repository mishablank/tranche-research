# Tranche Research

> **Eight DeFi yield-tranching chassis, side by side. Seven untested theories. One that has actually absorbed real losses on real defaults.**

[![Live](https://img.shields.io/badge/live-trancheprotocol.com-C44A36.svg)](https://trancheprotocol.com/)
[![Updated](https://img.shields.io/badge/updated-Aug%202026-blue.svg)](https://trancheprotocol.com/)
[![Build](https://img.shields.io/badge/build-none%20·%20single%20file-lightgrey.svg)](index.html)
[![License: BSL-1.0](https://img.shields.io/badge/license-BSL--1.0-green.svg)](LICENSE)

A *tranche* is French for "slice" – in structured finance, a slice of the same loan book carved out by where it sits in the loss waterfall. Junior takes the first dollar of loss and gets paid a premium for it; Senior is protected up to a defined cushion and accepts a thinner, steadier coupon. **Tranche Research** is a 13-slide research brief on the eight protocols trying to rebuild that machinery on-chain, and on the single question that separates them: *when the losses actually arrive, does the waterfall hold?*

Seven of the eight have never found out. The eighth, Goldfinch V1, has – three documented defaults and ~$17.9M of writedowns, ~$7.0M of it landing on Junior exactly as designed. That asymmetry is the spine of the deck.

Written to answer an insurance-vault design question – which chassis could actually sit under a vault that has to pay claims out of a subordinated capital cushion – and published because the side-by-side didn't exist anywhere else.

**[→ Read it at trancheprotocol.com](https://trancheprotocol.com/)**

---

## What's in it

- **Eight chassis compared on one matrix** – core abstraction, senior and junior instruments, loss waterfall, both yield sources, yield-split mechanism, lockups, underlying assets, live products, and the closest TradFi analogue for each.
- **The case against looping** – why one risk profile for a whole vault socializes loss, why phantom TVL hides insolvency, and what ranked loss changes. With the two 2025 worked examples: the wstETH:ETH unwind on Aave and Stream Finance's ~$285M cross-protocol blast radius.
- **The counterpoint, stated fairly** – tranching only delivers ranked loss when the underlying is predictable. Over an actively managed vault, the senior tranche is mostly a leveraged bet on the curator. Procyclical buffers, oracle and contract risk hitting both tranches *pari passu*, and binary DeFi tails all get their own bullet.
- **A worked example, end to end** – $100 into Royco, split ~$70/$30, run through five loss states from +8% yield down to −40%, showing where Junior is wiped and Senior starts to bleed.
- **The only real proof point** – Goldfinch V1's three defaults (Tugende Kenya, Stratos, Lend East) broken out by pool size, total writedown, Backer capital wiped, Senior portion, and what was backstopped off-protocol.
- **Five cross-cutting lenses** – where the first dollar of loss lands, who sets the yield split, how general the chassis is, liquid vs. locked, and what happens to Senior once Junior is exhausted.
- **An August 2026 postscript on Morpho Midnight** – why the fixed-rate launch isn't a ninth chassis but *is* the benchmark every senior coupon now has to clear against.

## The eight chassis

| # | Chassis | Shape | Status |
|---|---|---|---|
| 01 | **Royco Dawn** | Two-tranche wrapper over any yield source; dynamic utilization curve; observation-period loss recognition | Live |
| 02 | **Infinifi** | Stablecoin issuer with a three-tier deposit waterfall (iUSD → siUSD → liUSD) | Live |
| 03 | **Strata** | The purist generalized two-tranche chassis; guaranteed senior minimum + uncapped upside | Live |
| 04 | **Covenant** | Yield Coin / Leverage Coin per market; pro-rata senior haircut, no time priority | Live |
| 05 | **Knox Finance** | Fixed-maturity three-tier (Senior / Spectrum / Junior); the only contractual fixed senior coupon | Live |
| 06 | **Goldfinch V1** | Per-loan Senior/Junior over real-world private credit | **Tested – and wound down** |
| 07 | **Lotus Protocol** | LLTV-ordered tranches inside one lending market; RWA yield floor under Senior | Pre-launch |
| 08 | **Mezzanine** | Managed tranched stablecoin yield; mechanics partly undisclosed | Pre-launch |

Lotus and Mezzanine are covered narratively but held out of the matrix – neither is live with public on-chain data.

## How the deck is built

```
index.html          ← the entire deck: 13 <section class="slide"> blocks,
     │                inline <style>, no framework, no JS build step
     ├─ fonts        → Fraunces + Inter, Google Fonts (only external request)
     ├─ favicon.*    → .ico / .svg / .png / apple-touch-icon
     └─ og.png       → 2000×1050 social card, absolute-URL'd in the meta tags
     ↓
wrangler.jsonc      ← static-asset Worker, name "tranche-research", assets dir "."
     ↓
git push origin main
     ↓
Cloudflare auto-build  →  https://trancheprotocol.com/
                          https://tranche-research.blankm.workers.dev/
```

One file, no dependencies, no toolchain. Open `index.html` in a browser and you have the whole thing. That is deliberate: the deck should still render in ten years without a lockfile resolving.

## Repo layout

| Path | Purpose |
|---|---|
| `index.html` | The deck. All 13 slides, all styling, ~75 KB |
| `og.png` | Social preview card (2000×1050) |
| `favicon.ico` · `favicon.svg` · `favicon.png` · `apple-touch-icon.png` | Icon set |
| `wrangler.jsonc` | Cloudflare static-asset Worker config |
| `LICENSE` | Boost Software License 1.0 |

## Local preview

```bash
git clone https://github.com/mishablank/tranche-research.git
cd tranche-research
python3 -m http.server 8000     # then open http://localhost:8000
```

Or, to preview exactly as Cloudflare serves it:

```bash
npx wrangler dev
```

## Deploying

The repo is git-connected to Cloudflare, so **any push to `main` redeploys automatically** – no manual step, no CLI. Pushes typically go live within a minute.

If you fork it and want your own deployment: change `name` in [`wrangler.jsonc`](wrangler.jsonc), then either connect the repo in the Cloudflare dashboard (Workers & Pages → Create → Connect to Git, framework preset *None*, build command blank, output dir `/`) or run `npx wrangler deploy`. Keep the config `name` and the Worker name in sync – a mismatch silently creates a second Worker instead of updating the first.

Editing the deck means editing `index.html` directly. Slides are numbered in two places – the `data-slide="NN / 13"` attribute on each `.top-bar` and the footer – so adding a slide means renumbering both.

## Sources and method

Every figure is drawn from protocol documentation, public defaults write-ups, or DefiLlama, and is dated in place rather than presented as evergreen. Where a number moves – Infinifi's tier APYs, Midnight's TVL – the deck states the capture date next to it. Knox deliberately has no platform-wide APY because its parameters are set per pool at deployment, and the deck says so instead of inventing an average.

The Goldfinch default table is reconstructed from public documentation and the figures are approximate; it is the one dataset in the deck describing losses that actually happened, so it is flagged as approximate rather than quietly rounded.

## Known gaps

- **Goldfinch Prime is described as live, and it isn't any more.** Slides 04 and 09 present Prime as Goldfinch's ongoing fund-of-funds pivot. In June 2026, governance proposal GIP-87 passed unanimously to wind Prime down and put the protocol into maintenance mode, after defaults across a ~$100M loan book. The V1 tranching analysis – which is what the deck actually rests on – is unaffected, but the Prime framing needs a rewrite. This is the top open item.
- **Mezzanine remains partly undisclosed** – tranche token names, exact waterfall ordering, lockup terms and live TVL were all unpublished as of the last revision.
- **Lotus is still pre-launch**, so its loss-containment claims are documentation, not observed behaviour.

## License

[Boost Software License 1.0](LICENSE) © 2026 Mikhail Blank

The analysis is research, not investment advice. Figures are approximate and dated; verify anything load-bearing against the protocol's own documentation before acting on it.

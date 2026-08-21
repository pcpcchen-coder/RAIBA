# RAIBA

RAIBA (Reconfigurable Array of Inexpensive Battery Architecture) research and development repository.

The project is being reconstructed in 2026 around a broader product thesis:

> **RAIBA 2.0 — Software-Defined Battery Lifetime Platform**

The goal is to prevent a small number of weak cells/modules from prematurely determining the end-of-life of an entire battery pack, rack or system, using health-aware sensing, topology reconfiguration, selective power control and lifecycle optimization.

## Start Here

- [`handoff.md`](./handoff.md) — self-contained project context for a new AI/engineering session.
- [`docs/research/RAIBA_Deep_Research_2026.md`](./docs/research/RAIBA_Deep_Research_2026.md) — 2026 market, regulation, competitor and product-direction research summary.

## Evidence Layers

All future documents should keep these layers separate:

1. **Prior RAIBA Facts** — supported by historical RAIBA material.
2. **External Research Findings** — supported by current public/primary sources.
3. **Engineering Hypotheses** — proposed RAIBA 2.0/3.0 design ideas requiring validation.
4. **Unknowns** — items that require original artifacts, source code, schematics or experiments.

Do not turn hypotheses into historical facts.

## Current Architecture Thesis

> **Low-frequency Reconfiguration + Selective Bidirectional DC/DC + Deterministic Safety + Battery Digital Twin**

## North-Star Metrics

- **Lifetime Safe Delivered Energy**
- **System Availability over Lifetime**

## Planned Repository Structure

```text
RAIBA/
├── README.md
├── handoff.md
├── docs/
│   ├── architecture/
│   ├── algorithm/
│   ├── market/
│   ├── regulation/
│   ├── ip/
│   ├── prototype/
│   ├── research/
│   └── sources/
├── simulation/
├── firmware/
├── hardware/
├── data/
└── experiments/
```

## Historical Source Artifacts

Historical files recovered in the prior research session included:

- `RAIBA驗收.pdf`
- `Lab for energy storage management and systems software 20220328.pptx`

These files contain historical project material and should be preserved under `docs/sources/` once they are intentionally added to the repository. Their historical claims should be re-checked before external publication.

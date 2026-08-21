# RAIBA

RAIBA (Reconfigurable Array of Inexpensive Battery Architecture) research and development repository.

The project is being reconstructed in 2026 around a broader product thesis:

> **RAIBA 2.0 — Software-Defined Battery Lifetime Platform**

The goal is to prevent a small number of weak cells/modules from prematurely determining the end-of-life of an entire battery pack, rack or system, using health-aware sensing, topology reconfiguration, selective power control and lifecycle optimization.

## Start Here

- [`handoff.md`](./handoff.md) — self-contained project context for a new AI/engineering session.
- [`docs/research/RAIBA_Deep_Research_2026.md`](./docs/research/RAIBA_Deep_Research_2026.md) — 2026 market, regulation, competitor and product-direction research.
- [`docs/history/RAIBA_2022_BASELINE.md`](./docs/history/RAIBA_2022_BASELINE.md) — sanitized reconstruction of the historical RAIBA technical baseline.
- [`docs/architecture/RAIBA2_ARCHITECTURE.md`](./docs/architecture/RAIBA2_ARCHITECTURE.md) — RAIBA 2.0 architecture and control-stack thesis.
- [`docs/prototype/RAIBA2_MVP_SPEC.md`](./docs/prototype/RAIBA2_MVP_SPEC.md) — 12-month prototype hypothesis, KPI and kill criteria.
- [`docs/strategy/RAIBA_2_0_PRODUCT_THESIS.md`](./docs/strategy/RAIBA_2_0_PRODUCT_THESIS.md) — first-life extension / Long-Life Battery Chassis strategy.
- [`docs/session/SESSION_2026-08-21_OUTPUTS.md`](./docs/session/SESSION_2026-08-21_OUTPUTS.md) — archive/index of durable outputs from the reconstruction session.

## Evidence Layers

All future documents should keep these layers separate:

1. **Prior RAIBA Facts** — supported by historical RAIBA material.
2. **External Research Findings** — supported by current public/primary sources.
3. **Engineering Hypotheses** — proposed RAIBA 2.0/3.0 design ideas requiring validation.
4. **Unknowns** — items that require original artifacts, source code, schematics or experiments.

Do not turn hypotheses into historical facts.

## Current Architecture Thesis

> **Low-frequency Reconfiguration + Selective Bidirectional DC/DC + Deterministic Safety + Battery Digital Twin**

## Product Thesis

> **Use RAIBA from Day-1 to extend first life / in-situ asset life, rather than depending on second-life battery repurposing to create value.**

Longer-term product framing:

> **Long-Life Battery Chassis** — maintainable battery infrastructure rather than a monolithic consumable pack.

## North-Star Metrics

- **Lifetime Safe Delivered Energy**
- **System Availability over Lifetime**

## Repository Structure

```text
RAIBA/
├── README.md
├── handoff.md
├── docs/
│   ├── architecture/
│   ├── history/
│   ├── prototype/
│   ├── research/
│   ├── session/
│   ├── strategy/
│   ├── algorithm/      # planned
│   ├── market/         # planned
│   ├── regulation/     # planned
│   └── ip/             # planned
├── simulation/         # planned
├── firmware/           # planned
├── hardware/           # planned
├── data/               # planned
└── experiments/        # planned
```

## Public-Repository Security Rule

This repository is public.

Historical source artifacts reviewed during the reconstruction included files such as:

- `RAIBA驗收.pdf`
- `Lab for energy storage management and systems software 20220328.pptx`

At least one historical source contained enterprise-confidential markings/material. **The original PDF/PPT files are intentionally not committed to this public repository.**

Only sanitized technical reconstructions are stored here. Before adding any historical source code, schematic, presentation, email, report, test data or corporate artifact, explicitly verify that public disclosure is authorized.

Historical numerical claims—including the prior 3S2P / 5S5P context and approximately 75% capacity-disparity / 98% utilization acceptance direction—must be re-verified from original sources before external publication or customer use.

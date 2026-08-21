# RAIBA

> English source: [`README.md`](./README.md)

RAIBA（Reconfigurable Array of Inexpensive Battery Architecture）研究與開發 repository。

本專案於 2026 年重新整理，並以更廣義的產品主張為核心：

> **RAIBA 2.0 — Software-Defined Battery Lifetime Platform**

目標是透過 health-aware sensing、topology reconfiguration、selective power control 與 lifecycle optimization，避免少數弱化的 cell/module 過早決定整個 battery pack、rack 或 system 的 End-of-Life（EOL）。

## 從這裡開始（Start Here）

- [`handoff.zh-TW.md`](./handoff.zh-TW.md) — 提供新 AI / engineering session 使用的完整專案交接背景。
- [`docs/research/RAIBA_Deep_Research_2026.zh-TW.md`](./docs/research/RAIBA_Deep_Research_2026.zh-TW.md) — 2026 市場、法規、競品與產品方向研究。
- [`docs/history/RAIBA_2022_BASELINE.zh-TW.md`](./docs/history/RAIBA_2022_BASELINE.zh-TW.md) — 歷史 RAIBA 技術 baseline 的去機密化重建。
- [`docs/architecture/RAIBA2_ARCHITECTURE.zh-TW.md`](./docs/architecture/RAIBA2_ARCHITECTURE.zh-TW.md) — RAIBA 2.0 architecture 與 control stack 主張。
- [`docs/prototype/RAIBA2_MVP_SPEC.zh-TW.md`](./docs/prototype/RAIBA2_MVP_SPEC.zh-TW.md) — 12 個月 prototype 假說、KPI 與 kill criteria。
- [`docs/strategy/RAIBA_2_0_PRODUCT_THESIS.zh-TW.md`](./docs/strategy/RAIBA_2_0_PRODUCT_THESIS.zh-TW.md) — first-life extension / Long-Life Battery Chassis 產品策略。
- [`docs/session/SESSION_2026-08-21_OUTPUTS.zh-TW.md`](./docs/session/SESSION_2026-08-21_OUTPUTS.zh-TW.md) — 本次重建 session 的持久化產出索引。

## Evidence Layers

後續所有文件都應嚴格區分下列四種 evidence layer：

1. **Prior RAIBA Facts** — 由歷史 RAIBA 資料支持的事實。
2. **External Research Findings** — 由目前公開資料與 primary sources 支持的外部研究結果。
3. **Engineering Hypotheses** — 尚需驗證的 RAIBA 2.0/3.0 新設計與工程假說。
4. **Unknowns** — 必須取得原始 artifact、source code、schematic 或實驗結果才能確認的項目。

不可把 hypothesis 寫成歷史事實。

## 目前 Architecture Thesis

> **Low-frequency Reconfiguration + Selective Bidirectional DC/DC + Deterministic Safety + Battery Digital Twin**

## Product Thesis

> **從 Day-1 導入 RAIBA，以延長 first life / in-situ asset life，而不是把價值建立在 second-life battery repurposing 上。**

長期產品定位：

> **Long-Life Battery Chassis** — 可維護的 battery infrastructure，而不是 monolithic consumable pack。

## North-Star Metrics

- **Lifetime Safe Delivered Energy**
- **System Availability over Lifetime**

## Repository Structure

```text
RAIBA/
├── README.md
├── README.zh-TW.md
├── handoff.md
├── handoff.zh-TW.md
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

這個 repository 是 public。

在專案重建過程中曾檢視的歷史 source artifacts 包括：

- `RAIBA驗收.pdf`
- `Lab for energy storage management and systems software 20220328.pptx`

至少有一份歷史 source 含有 enterprise-confidential 標示或內容。**原始 PDF/PPT 刻意不 commit 到這個 public repository。**

此處只保留 sanitized technical reconstruction。在加入任何歷史 source code、schematic、presentation、email、report、test data 或 corporate artifact 之前，必須明確確認該資料已獲授權可公開揭露。

歷史數值 claim——包括先前 3S2P / 5S5P 背景，以及約 75% capacity disparity / 98% utilization 的驗收方向——在對外發表或提供客戶使用之前，都必須重新由原始資料確認。
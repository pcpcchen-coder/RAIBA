# RAIBA 2.0 Product Thesis

> English source: [`RAIBA_2_0_PRODUCT_THESIS.md`](./RAIBA_2_0_PRODUCT_THESIS.md)
>
> 狀態：2026 strategy hypothesis，依據 prior RAIBA material 與目前市場研究形成。

## 1. 核心 Repositioning

RAIBA 不應把價值建立在把 retired battery 轉移到不相關 second-life application 的經濟模型上。

更強的 product thesis 是：

> **從 Day-1 導入 reconfiguration，延長 first life / in-situ asset life，並延後 retirement。**

要解決的核心問題，是 fixed electrical topology 中少數 weak module 導致整個 system 過早 EOL。

## 2. Strategic Statement

> **RAIBA 2.0 是一個 health-aware、dynamically reconfigurable battery platform，透過重新分配 battery 與 power-electronic resources，最大化 safe lifetime energy、system availability 與 asset life。**

這個定位比單一 BMS feature 更廣，但也不需要一開始就企圖取代所有 PCS/inverter function。

## 3. Product Model Shift

傳統 battery product model：

```text
Battery Pack
   |
   v
Age / mismatch
   |
   v
Power or capacity limitation
   |
   v
Pack / rack replacement
```

RAIBA product model：

```text
Long-Life Battery Chassis
   |
   +-- replaceable battery modules
   +-- reconfigurable switching fabric
   +-- selective power-electronic resources
   +-- deterministic safety controller
   +-- battery digital twin
   +-- maintenance / warranty intelligence
```

概念上更接近可維護的 computing platform，而不是 monolithic consumable。

## 4. Long-Life Battery Chassis

Long-Life Battery Chassis 的重要觀念，是把不同 component 的 service life 拆開。

以下僅為 illustrative concept：

- structural/enclosure asset 可以跨越多次 battery-module service interval；
- switching/power stage 可以有獨立 maintenance schedule；
- controller/software 可以 upgrade；
- individual module 可以 isolation 或 service，而不必迫使整個 system 過早 retirement。

每一種 component 的實際 service life 必須由 product design 與 qualification 建立，不能直接假設。

## 5. Revenue Layers

可能的 product stack：

### Hardware
- RAIBA controller；
- switching fabric；
- sensing；
- selective/shared DC/DC；
- serviceable battery chassis。

### Embedded Software
- Battery OS；
- state estimation；
- Safety Supervisor；
- Health-aware Scheduler；
- topology 與 converter-resource allocator。

### Cloud / Fleet Software
- RUL / SOH analytics；
- warranty intelligence；
- maintenance prioritization；
- lifetime-energy reporting；
- fleet benchmarking。

### Performance-based Commercial Models
後期可能的模式包括：

- guaranteed lifetime energy throughput；
- availability guarantee；
- lifecycle-service contract；
- module maintenance / replacement service。

## 6. Market Priority Hypothesis

### Priority 0 — C&I / Utility BESS with New Batteries

原因：
- battery asset value 高；
- 預期 project life 長；
- module mismatch 與 aging 無法避免；
- system 可以容忍一定程度的額外 electronics；
- lifetime throughput 與 maintenance economics 容易量化。

### Priority 0 — UPS / Data Center / Telecom

這些市場的核心價值可能是 **availability**，而不是 energy density。

若 weak module 能被 isolation，而 system 仍以 reduced capability 安全持續運作，就可能產生顯著經濟價值。

### Priority 1 — Swap Batteries / Battery-as-a-Service

有利條件：
- 大量 fleet；
- modules 天然會形成不同 aging history；
- centralized charging 與 maintenance；
- SOH-aware duty 與 charging allocation 具有高價值。

### Priority 1 — Industrial Fleet

例如：
- AGV / AMR；
- forklift；
- mining equipment；
- port equipment；
- selected commercial vehicles。

這些環境中的 downtime 成本高，而且 fleet 通常能集中管理。

### Priority 2 — EV Traction

技術價值高，但障礙也高：
- functional safety；
- HV isolation；
- crash safety；
- packaging 與 weight；
- automotive qualification；
- OEM integration cycle。

不建議做第一個 commercial product。

### Long-term / Specialized — Aviation and Drones

Fault tolerance 很有吸引力，但 certification 與 mass constraint 非常嚴格。

## 7. Regulatory Trigger — 重要表述原則

2026 中國 retired-battery 政策討論**不能**被簡化為「所有 used batteries 都禁止再利用」。

較安全的 strategic interpretation 是：

- traction-battery cascade utilization 的特殊 policy treatment/framework 正在改變；
- target application 越來越需要符合其一般 product quality 與 safety requirement；
- 某些 category 可能被法律或 mandatory standard 明確限制；
- 因此 second-life business model 可能面臨更高的 product-specific certification 與 liability burden。

這會強化**在原 product/service context 中延後 retirement**的經濟論點，但這是 product-strategy inference，不是普遍性的法律結論。

## 8. Competitive Positioning

市場正在逐步驗證原始 RAIBA thesis 的多個部分。

高相關 benchmark：
- Relectrify；
- Element Energy；
- STABL Energy；
- Pulsetrain；
- Brill Power。

Adjacent companies：
- B2U Storage Solutions；
- Smartville；
- Connected Energy；
- Moment Energy；
- Voltfang；
- TWAICE；
- ACCURE；
- Circunomics。

現在的重要問題不再是 adaptive/reconfigurable battery architecture 能不能實現，而是：

> **在 cell-level converter、multilevel battery inverter 與 battery-intelligence platform 已經存在的 2026 年，RAIBA 還能在哪裡建立有辨識度的 value？**

目前最好的 hypothesis：

1. **Selective rather than universal power electronics.**
2. **Lifetime optimization rather than primarily waveform synthesis.**
3. **Day-1 asset-life extension rather than dependence on used-battery repurposing.**
4. **Deterministic safety plus predictive intelligence.**
5. **Maintainable chassis / module-level service model.**

## 9. North-Star Metrics

產品最終應依下列指標判斷：

1. **Lifetime Safe Delivered Energy**
2. **System Availability over Lifetime**
3. avoided premature module/rack/pack replacement；
4. lifecycle TCO；
5. safety 與 certification feasibility。

Single-cycle efficiency 仍然重要，但不足以成為主要 business metric。

## 10. Core Scientific Problem

可將核心 research question 正式化為：

> 在一群 heterogeneous、持續 aging 的 battery modules，以及有限的 switching/converter resources 條件下，應如何隨時間動態配置 electrical topology 與 power duty，才能最大化 safe lifetime energy 與 system availability？

這是 RAIBA 2.0 最核心的 problem statement。
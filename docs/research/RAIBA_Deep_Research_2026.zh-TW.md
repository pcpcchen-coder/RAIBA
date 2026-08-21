# RAIBA 2.0 — 2026 市場、法規與產品方向深入研究

> English source: [`RAIBA_Deep_Research_2026.md`](./RAIBA_Deep_Research_2026.md)
>
> Research snapshot：2026-08  
> Purpose：作為重新啟動 RAIBA 的 strategy 與 engineering baseline。  
> Evidence rule：嚴格區分 prior RAIBA material、external research findings 與 engineering hypotheses。

## Executive Summary

市場已明顯往原始 RAIBA thesis 靠近。約 2023–2026 年間，commercial 與 near-commercial battery architecture 越來越多採用 cell/module-level power electronics、adaptive control、dynamic insertion/bypass、distributed conversion 與 software-defined battery behavior。

目前辨識出的最重要 benchmark：

- **Relectrify** — cell-level adaptive control 與 bypass，並與 power conversion 整合。
- **Element Energy** — adaptive/distributed BMS，搭配 dedicated module-level power conversion。
- **STABL Energy** — modular multilevel battery/inverter architecture，可動態連接 battery module。
- **Pulsetrain** — software-defined EV powertrain，整合 BMS、inverter、charger 與 dynamic battery switching。
- **Brill Power** — 相鄰的 adaptive battery-management direction。

2026 年的 regulatory trigger 也改變了最適合的 business framing。中國針對 retired traction-battery「梯次利用」的政策變動，**不能**簡化為「全面禁止所有 used-battery reuse」。較可信的 interpretation 是：「梯次利用」原本作為特殊管理 category 的 policy framework 正在調整或移除；使用 retired traction batteries 生產的產品，越來越需要符合 target application 一般的 quality、safety 與 mandatory requirements，部分 application category 另有明確限制。

因此可建立更強的 RAIBA business thesis：

> **不要依賴 second-life reuse 才創造價值；從 Day-1 導入 RAIBA，延長 first life / in-situ asset life，並延後 retirement。**

建議的 2026 architecture direction：

> **RAIBA 2.0 = Low-frequency Reconfiguration + Selective Bidirectional DC/DC + Deterministic Safety + Battery Digital Twin.**

建議第一個 commercial prototype **不要從 EV traction 開始**。以 C&I ESS / UPS 為導向的 8–16-module rack-scale prototype，更適合先證明核心 value proposition：在 realistic module mismatch 下，取得更多 Lifetime Safe Delivered Energy、更高 availability 與 graceful degradation。

---

## 1. Research Trigger

本專案重新啟動的直接觸發點，是中國針對 retired traction-battery utilization 的政策與標準變動討論：

- Battery100 article：https://www.battery100.org/news/tongzhi/0I1203402026.html

核心 strategic question：

> 如果 retired battery 要跨到另一個 application 做 repurposing，變得更受限制、更昂貴、或 certification burden 更高，那麼 reconfigurable architecture 是否能透過延後原 system retirement，創造更高價值？

RAIBA 特別相關，因為其原始設計 intent 就是避免 weak module 限制整個 battery array 的 usable capability。

---

## 2. Regulatory Correction — China

### 2.1 不應宣稱的內容

避免寫：

> 「China bans all used batteries from being reused.」

目前研究不支持這種 blanket statement。

### 2.2 較準確的 Interpretation

官方/地方政策解讀顯示，2026 年的變動主要是移除或調整「梯次利用」作為獨立管理 product/category framework 的特殊 policy treatment。使用 retired traction batteries 生產的產品，應依其 target product category 符合一般 quality 與 safety requirement；如果 laws 或 mandatory standards 明確禁止某應用，則仍屬禁止。

研究中找到的 public-policy references：

- Heilongjiang policy interpretation：
  https://gxt.hlj.gov.cn/gxt/c107069/202607/c00_31963411.shtml
- Hunan policy interpretation：
  https://hunan.gov.cn/zqt/zcsd/202607/t20260731_34037283.html
- Jiangxi policy interpretation：
  https://gxt.jiangxi.gov.cn/jxsgyhxxht/gyjnhzhly/content/content_2084911550435762176.html

在對外作 legal conclusion 前，仍必須用最新 MIIT primary documents 核對 exact legal effect、受影響 standard 與 application boundary。

### 2.3 對 RAIBA 的 Strategic Implication

即使 reuse 法律上仍可行，如果必須增加下列流程，經濟性也可能下降：

- dismantling
- state-of-health screening
- sorting
- recertification
- repacking
- application-specific safety validation
- warranty reassignment
- traceability and responsibility transfer

因此 RAIBA 可以走另一條 value path：

```text
New Battery System
     -> RAIBA from Day-1
     -> Health-aware duty allocation
     -> Weak module isolation / support
     -> Graceful degradation
     -> Selective maintenance
     -> Longer first life
     -> Later retirement / recycling
```

這必須視為 **product-strategy inference**，而不是直接的法律要求。

---

## 3. Taiwan Comparison

目前研究顯示，台灣比較像是走向「對 repurposed battery 建立明確 safety / inspection 管理」，而不是 blanket prohibition。

研究中找到與下列項目相關的 BSMI materials：

- CNS 63330-1 — safety of repurposed batteries
- CNS 62933-5-3 — electrochemical energy storage systems safety / repurposing
- 特定 second-use lithium energy-storage products 的 phased mandatory inspection

研究中保存的 primary BSMI references：

- https://www.bsmi.gov.tw/wSite/record/file_actData.jsp?filename=f1749608524136.pdf&fileuuid=121610
- https://hsinchu.bsmi.gov.tw/wSite/record/file_act.jsp?ixCuAttach=36289
- https://www.bsmi.gov.tw/wSite/public/Data/f1766539203719.pdf

在 external publication 前，必須重新由最新 BSMI notice 核對 effective date、product-capacity range 與 exact inspection scope。

---

## 4. Technology Landscape Taxonomy

### Type A — Topology Reconfiguration / Bypass-first

最接近原始 RAIBA。

典型功能：

- cell/module isolation
- dynamic insertion/removal
- fault bypass
- topology adjustment
- graceful degradation

Value proposition：

> Battery 的 physical topology 不需要在完整 system lifetime 中永遠固定。

### Type B — Cell/Module-level DC/DC

典型功能：

- independent module current control
- independent power allocation
- mismatch compensation
- heterogeneous battery support

Trade-offs：

- converter cost
- switching/conduction loss
- EMI
- thermal management
- component count 增加
- reliability 與 certification burden

### Type C — Modular Multilevel / Cascaded Converter Battery

典型功能：

- module insertion/bypass
- battery 與 inverter integration
- multilevel voltage synthesis
- 直接產生 AC 或 high-voltage DC

這個 category 越來越把「BMS」與「power conversion」合併成同一套 architecture。

### Type D — Active Balancing / Distributed BMS

可透過降低 SOC mismatch 與提高 observability，提供部分相同效益，但通常仍維持 main pack topology 固定。

### Type E — Software / AI Optimization without Reconfiguration

包括：

- SOH
- RUL
- anomaly detection
- cloud fleet analytics
- Digital Twin
- warranty analytics

這些 system 能改善 decision，但通常無法透過改變 topology，physically remove weakest-member constraint。

RAIBA 的 strategic opportunity 是把 intelligence 與 actuation 結合。

---

## 5. Benchmark Deep Dives

## 5.1 Relectrify

Official references：

- https://www.relectrify.com/
- https://www.relectrify.com/us/patents
- ARENA second-life trial report：
  https://arena.gov.au/assets/2024/01/Relectrify-Second-Life-Battery-Trial-Lessons-Learnt-Report.pdf

Research-supported characteristics：

- cell-level control
- 可對 weak/faulty cells 進行 individual management
- adaptive battery behavior
- BMS 與 inverter functionality 趨於融合
- commercial BESS direction
- AC1 公開定位約 250 kW / 1 MWh

為什麼對 RAIBA 重要：

1. 它驗證 weakest-cell problem 可以形成 commercial value proposition。
2. 它證明 cell-level hardware control 可在 commercial scale 實作。
3. 它是主要 patent/FTO benchmark。
4. 它代表 design spectrum 中偏 converter-dominant 的一端。

Potential RAIBA differentiation：

- lower-frequency reconfiguration
- fewer active power stages
- selective rather than universal converter allocation
- lifetime/TCO optimization，而不是以 AC waveform synthesis 為主要目的

---

## 5.2 Element Energy

Official references：

- https://elementenergy.com/
- https://elementenergy.com/solutions/
- https://elementenergy.com/our-path/

U.S. DOE project material：

- https://www.energy.gov/sites/default/files/2022-11/Recycling%20and%20Second-Use%20Selections%20Factsheets%2011-16.pdf

Research-supported characteristics：

- adaptive/distributed BMS architecture
- module-level dedicated power conversion
- independent module power-flow management
- mismatch compensation
- lifecycle-energy optimization
- second-life 與 heterogeneous-battery relevance

為什麼對 RAIBA 重要：

這是最接近歷史 RAIBA「per-column / per-module DC/DC conversion」構想的 modern commercial analogue 之一。

RAIBA 的關鍵 design question：

> 是否能用較小的一組 selectively allocated converters，而不是每個 module 一顆 dedicated converter，仍得到相近的 lifetime benefit？

---

## 5.3 STABL Energy

Official references：

- https://stabl.com/en/technology/
- https://stabl.com/en/technology/working-principle/

Research-supported characteristics：

- modular multilevel inverter architecture
- battery module dynamic connection/disconnection
- module-level sensing/control
- heterogeneous-battery support
- power conversion 與 battery management convergence

為什麼重要：

STABL 位在歷史 RAIBA-style topology reconfiguration 與 MVAC/converter-dominant control 的交界附近。

Potential RAIBA differentiation：

- 不把 high-frequency conversion 設為每一個 module 的 default path
- 利用 low-frequency topology change 最佳化 lifetime 與 availability
- 只有能產生 measurable lifecycle value 時，才啟用昂貴 conversion resource

---

## 5.4 Pulsetrain

Official references：

- https://pulsetrain.com/
- https://pulsetrain.com/technology/

Research-supported characteristics：

- integrated BMS + inverter + charger
- software-defined EV powertrain
- MOSFET matrix 與 dynamic battery switching
- runtime cell enable/disable 與 series/parallel control

為什麼重要：

它顯示 reconfigurable battery concept 已開始進入 EV powertrain architecture，而不是只停在 laboratory BMS research。

為什麼 RAIBA 不應先從這個市場 commercialize：

- automotive qualification
- functional safety
- HV isolation
- crash integration
- weight/cost pressure
- OEM design cycle

EV 仍然具 strategic importance，但不是建議的第一個 MVP market。

---

## 6. RAIBA 2.0 Positioning

### 6.1 不要只停在 Old RAIBA

Pure Enable/Bypass architecture 仍有價值，但相較 2026 market direction 可能太窄。

### 6.2 不要直接跳到 Universal Converters

每個 module 都配置 bidirectional converter 雖然 control 能力很強，但會提高：

- BOM
- semiconductor count
- conversion loss
- EMI
- thermal load
- firmware complexity
- failure points
- certification burden

### 6.3 建議的 Hybrid Direction

> **Low-frequency topology reconfiguration + Selective Bidirectional DC/DC + Deterministic Safety + Battery Digital Twin.**

概念：

```text
Battery Modules
   |
   +--> Direct path for healthy modules
   |
   +--> Enable/Bypass switching fabric
   |
   `--> Selectively allocated DC/DC support
              |
              v
        Common DC bus / system
```

Control stack：

```text
Cloud / EMS / TCO / Warranty
            |
Battery Digital Twin / SOH / RUL / Risk
            |
Lifetime Optimization Engine
            |
Safety Supervisor
            |
Switch Scheduler + Selective DC/DC Controller
            |
Battery Modules / Sensors / Switches
```

---

## 7. Selective / Shared DC/DC — 2026 Key Hypothesis

不是：

```text
N modules = N converters
```

而是研究：

```text
N modules
   -> switch/routing fabric
   -> M shared converters

where M << N
```

Scheduler 可依以下資訊決定哪個 module 取得 converter assistance：

- SOC mismatch
- SOH mismatch
- internal resistance
- temperature
- predicted degradation
- requested system power
- converter availability
- switching/conversion loss

Potential advantages：

- converter count 較少
- cost 較低
- conversion loss 較低
- 仍可對最需要支援的一小部分 module 提供 independent assistance

Potential new risks：

- routing-matrix complexity
- converter resource contention
- additional failure modes
- 重新配置 converter 時的 transient management
- 困難的 FTO landscape

這是 key prototype 與 patent-study topic。

---

## 8. Control and Optimization

### 8.1 Safety Ownership

AI/optimizer 絕不能直接發出 unsafe hardware state。

```text
Optimizer / AI
     -> proposed action
     -> deterministic Safety Supervisor
     -> validated switching/power command
     -> hardware
```

Hard deterministic rules 應控制：

- over-voltage
- under-voltage
- over-current
- over-temperature
- short circuit
- isolation fault
- precharge
- switching sequence
- minimum connected-module count
- output-voltage window
- converter/switch SOA
- fail-safe state

### 8.2 Optimization Objective

概念 objective：

```text
maximize
  + delivered energy
  + availability
  + remaining useful life

minimize
  - degradation spread
  - thermal imbalance
  - switching wear/count
  - conversion loss
  - operational risk
```

Generic objective：

```text
J =
 + w1*E_delivered
 + w2*Availability
 + w3*RUL
 - w4*DegradationSpread
 - w5*ThermalSpread
 - w6*SwitchingCost
 - w7*ConversionLoss
 - w8*Risk
```

### 8.3 Time-scale Separation

```text
µs-ms       hardware protection
ms-100ms    fast BMS / power protection
seconds-min topology scheduling
minutes-hrs degradation / thermal optimization
hours-days  maintenance / lifetime planning
```

如此可避免把所有問題硬塞進一個「AI agent」。

---

## 9. Application Opportunity Ranking — Current Hypothesis

### P0 — C&I / Utility ESS using New Batteries

適配度高，因為：

- asset cost 高
- target service life 長
- mismatch 會隨年限累積
- 額外 electronics 容忍度較高
- maintenance 是預期中的作業
- lifetime throughput 具有直接 TCO value

### P0 — UPS / Data Center / Telecom

適配度高，因為：

- availability 是關鍵
- graceful degradation 很有價值
- weak module 不應不必要地讓整個 string/rack 離線

### P1 — Swap Battery / Battery-as-a-Service

適配度高，因為：

- modules 天然會累積不同 history
- charging 集中化
- 有 fleet-level data
- duty allocation 可以 software-defined

### P1 — Industrial Fleets

例如：

- AGV / AMR
- forklift
- port equipment
- mining / heavy equipment

特別適合 downtime 與 battery replacement 都昂貴的環境。

### P2 — EV Traction

長期價值大，但 engineering / certification barrier 高。

### Conditional — Second-life BESS

Opportunity 高度依賴 jurisdiction、certification 與 product category。不可假設 retired-battery reuse 沒有限制。

---

## 10. Recommended 12-Month Prototype

### Target

C&I / UPS rack-scale demonstrator。

### Initial Engineering Hypothesis

- 8–16 modules
- 16S1P 或 8S2P-like module-level topology
- 48–120 VDC
- 5–20 kW
- 10–50 kWh

最終 spec 必須依 available lab batteries、test equipment 與 safety limit 決定。

### 比較三種 System

1. Fixed topology + conventional BMS
2. Fixed topology + active balancing
3. RAIBA 2.0：Enable/Bypass + Health-aware Scheduler + Selective DC/DC

### Fault / Mismatch Matrix

注入或模擬：

- capacity mismatch
- resistance increase
- SOC mismatch
- temperature mismatch
- weak module
- communication loss
- sensor fault
- converter unavailable
- bypass switch failure

### Acceptance KPIs

- usable energy before cutoff
- lifetime throughput
- system availability
- degradation spread
- thermal spread
- conversion efficiency
- switching count
- fault-recovery time
- safe degraded-mode operation

North-star metric：

> **Lifetime Safe Delivered Energy**

Secondary north-star metric：

> **System Availability over Lifetime**

---

## 11. Commercial / TCO Framing

Traditional approach：

```text
Battery Rack
 -> mismatch grows
 -> weakest member limits string
 -> module/string augmentation or replacement
 -> asset downtime / maintenance
```

RAIBA approach：

```text
Long-Life Battery Chassis
 -> monitor individual health
 -> redistribute duty
 -> support or bypass weak modules
 -> continue degraded operation
 -> replace only when economically justified
```

Potential revenue layers：

- controller 與 switching hardware
- Selective DC/DC hardware
- Battery OS license
- cloud/fleet analytics
- warranty intelligence
- predictive maintenance
- availability/lifetime-throughput service agreement

TCO model 應比較：

```text
incremental RAIBA BOM + loss + maintenance complexity
versus
avoided premature replacement + additional lifetime MWh + availability value
```

---

## 12. IP / FTO Priorities

Simple bypass switch 已有大量 prior art。

可能較能形成 defensible combined IP 的方向：

- health-aware topology selection
- shared/selective converter allocation
- degradation-aware duty scheduling
- safe bypass/rejoin sequencing
- Digital Twin-driven topology change
- reconfiguration 下的 Mixed CC/CV operation
- lifetime-energy objective function
- maintainable Long-Life Battery Chassis

Priority FTO targets：

- Relectrify patents
- Element Energy patents
- STABL Energy patents
- Pulsetrain patents
- modular multilevel battery/inverter patents
- reconfigurable battery-switching patents
- distributed converter BMS patents

Architecture freeze 前應先做 claim-level timeline work。

---

## 13. 12 / 24 / 36 Month Roadmap

### 0–3 months

- architecture trade study
- patent/FTO landscape
- lifetime simulation model
- switch prototype
- representative mismatch dataset

### 3–6 months

- 8–16 module hardware
- Enable/Bypass state machine
- fault injection
- Health-aware Scheduler
- baseline comparison

### 6–12 months

- selective/shared DC/DC
- Battery Digital Twin
- accelerated aging
- lifetime-throughput experiment
- first TCO validation

### 12–24 months

- 50–200 kWh pilot direction
- C&I / UPS / telecom design partner
- cloud analytics
- functional-safety 與 certification planning

### 24–36 months

- DVT / PVT
- field pilot
- certification
- fleet deployment
- commercialization decision

---

## 14. Kill Criteria

如果下列情況成立，應停止、簡化或 pivot：

- incremental electronics cost 高於可信的 lifetime-value gain；
- converter/switching loss 抵消 additional lifetime energy benefit；
- realistic mismatch 只能帶來 marginal improvement；
- 增加的 component count 讓 availability 降幅大於 reconfiguration 的提升；
- certification 或 FTO 使 target architecture 商業上不可行。

可作內部 decision target，但**不是 public claim**：

- <5% lifetime-energy improvement：case 弱
- >10%：繼續研究
- >20%：具吸引力
- >30%：可能具 disruptive potential

---

## 15. Current Strategic Conclusion

RAIBA 的原始概念並沒有過時。Commercial battery architecture 正逐步驗證其核心 premise：當 cell/module population 越來越 heterogeneous，fixed battery topology 會犧牲 usable energy、availability 與 lifetime。

但是 2026 restart 不應只是重新製作 2022 design。

建議 positioning：

> **RAIBA 2.0 是一個 health-aware、dynamically reconfigurable battery platform，會重新分配 battery 與 power-electronic resources，以最大化 safe lifetime energy、system availability 與 asset life。**

最強 differentiation hypothesis：

> **Selective rather than universal power electronics.**

最強 commercial thesis：

> **Lifetime optimization rather than only waveform synthesis or second-life repurposing.**

最強 product vision：

> **Battery Pack 從 monolithic consumable 演進成 maintainable infrastructure — 由 Battery Operating System 管理的 Long-Life Battery Chassis。**

---

## References / Starting Points

### Regulation / Standards

- Battery100：https://www.battery100.org/news/tongzhi/0I1203402026.html
- Heilongjiang policy interpretation：https://gxt.hlj.gov.cn/gxt/c107069/202607/c00_31963411.shtml
- Hunan policy interpretation：https://hunan.gov.cn/zqt/zcsd/202607/t20260731_34037283.html
- BSMI materials：https://www.bsmi.gov.tw/

### Direct Technology Benchmarks

- Relectrify：https://www.relectrify.com/
- Relectrify patents：https://www.relectrify.com/us/patents
- Relectrify ARENA trial report：https://arena.gov.au/assets/2024/01/Relectrify-Second-Life-Battery-Trial-Lessons-Learnt-Report.pdf
- Element Energy：https://elementenergy.com/
- Element solutions：https://elementenergy.com/solutions/
- STABL Energy：https://stabl.com/en/technology/
- Pulsetrain：https://pulsetrain.com/technology/
- DOE second-use factsheets：https://www.energy.gov/sites/default/files/2022-11/Recycling%20and%20Second-Use%20Selections%20Factsheets%2011-16.pdf

### Background Research Directions

後續研究 search terms：

- dynamically reconfigurable battery
- reconfigurable battery system
- software-defined battery
- programmable battery pack
- adaptive battery topology
- module bypass battery
- fault-tolerant battery pack
- cell-level power electronics
- module-level DC/DC battery
- modular multilevel battery
- cascaded battery inverter
- adaptive/distributed BMS
- battery digital twin

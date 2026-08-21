# RAIBA 2.0 Project Handoff — 繁體中文版

> English source: [`handoff.md`](./handoff.md)
>
> Status：2026-08-21  
> Purpose：讓新的 AI / engineering session 不需要重新拼湊專案歷史，就能直接接手 RAIBA。

## 1. Mission

RAIBA 是一套 **dynamically reconfigurable battery architecture**。2026 年重新啟動後的 thesis，重點不再是把 retired battery 搬到另一個 second-life application，而是從 Day-1 延長 **first life / in-situ asset life**：避免少數 weak cell/module 決定整個 pack、rack 或 system 的 EOL。

核心問題：

```text
Traditional fixed series battery

Module A  SOH 96%
Module B  SOH 94%
Module C  SOH 92%
Module D  SOH 71%   <- weakest member
Module E  SOH 93%

System capability and cutoff are frequently constrained by Module D.
```

RAIBA objective：

> 透過 health-aware sensing、reconfiguration 與 selective power control，解除 weakest member 對 system lifetime、available energy 與 availability 的綁定。

一句話產品方向：

> **RAIBA 2.0 = Software-Defined Battery Lifetime Platform.**

更長期的產品定位：

> **Long-Life Battery Chassis** — 可維護的 battery infrastructure，而不是 monolithic consumable pack。

---

## 2. Historical RAIBA Baseline — Prior User Material

以下內容來自原始 session 中曾討論與檢視的 prior RAIBA material。必須視為 historical RAIBA facts 或 prior-material claims，而不是新的 web research。

歷史上使用過的名稱包括：

- Reconfigurable Array of Inexpensive Battery Architecture
- Dynamically Reconfigurable Battery Array Architecture and Management Algorithm
- 可動態重組與自我調節之電池陣列系統

核心 hardware concept：

- per-module **Enable / Bypass**
- 動態改變有效 battery array topology
- 相對於 continuous high-frequency DC/DC regulation，偏向 low-frequency topology reconfiguration

概念 topology：

```text
B1 -> [Enable/Bypass] --+
B2 -> [Enable/Bypass] --+
B3 -> [Enable/Bypass] --+--> Battery DC Bus / Array
B4 -> [Enable/Bypass] --+
B5 -> [Enable/Bypass] --+
                ^
                |
        RAIBA Controller
```

歷史上考慮過的 controller input：

- voltage
- current
- temperature
- SOC
- SOH
- capacity
- internal resistance / DCIR
- RC / ECM parameters
- EIS-related measurements
- fault state

可能 action：

- ENABLE
- BYPASS
- DERATE
- REST
- REJOIN

### 2.1 Discharge Direction

2022 planning 將工作拆成 discharge optimization 與 charge optimization。

Discharge-side topic 包括：

- real-time battery-state measurement
- SOC / SOH / EIS / RC estimation
- health-aware module selection
- linear / nonlinear switching strategy
- switching timing
- surge/transient behavior
- measurement frequency
- current limiting

概念上：

```text
Battery State
    -> State Estimator
    -> Health-aware Scheduler
    -> Module Selection
    -> Enable / Bypass
    -> Optimized Discharge
```

### 2.2 Charge Direction

歷史規劃也包含：

> **Mixed CC/CV Operations for Batteries Connected in Series**

核心想法是：fixed series string 迫使所有 member 流過相同 current。如果某一 module 較早到達 voltage/SOC limit，就會限制整串。RAIBA 希望讓 charging 變成 battery-state-aware，而不是把整個 string 視為單一 homogeneous element。

### 2.3 DC/DC Direction

歷史規劃還包含：

> **Per-column DC/DC Conversion and Current Limiting Source**

這表示專案當時已從純 ON/OFF topology control，開始走向 controlled power sharing。

因此有兩個重要 control dimension：

1. **Topology control** — enable、bypass、connect、disconnect。
2. **Power control** — current、voltage、power、charge/discharge rate。

### 2.4 RAIBA vs. MVAC

歷史資料曾將 RAIBA 與 MVAC-like continuous power-electronic regulation 比較。

- RAIBA：low-frequency topology reconfiguration，例如 Enable/Bypass。
- MVAC / converter-dominant architecture：continuous DC/DC 或 multilevel power conversion。

2026 年不應再問「哪一個贏」，而應問：**哪一個 hybrid boundary 能產生最佳 lifetime/TCO trade-off？**

### 2.5 Battery Intelligence

歷史資料也包含下列 battery intelligence topic：

- lifetime prediction
- RUL-related prediction
- micro internal short / micro-short detection
- charging-trace comparison
- voltage/current/temperature/DCIR-based models

這很重要，因為 RAIBA 從來不只是 switching circuit。完整 loop 是：

```text
Sensing -> State/Health Intelligence -> Decision -> Physical Actuation
```

### 2.6 需要重新確認的 Historical Demo / Validation Claims

Prior material 曾提到：

- 3S2P hardware/demo
- 5S5P simulation
- 在 module capacity disparity 約 75% 時，仍可達約 98% array-capacity utilization 的驗收方向

這些數值在 paper、patent、customer presentation 或 business case 對外使用前，**必須重新由原始文件與 exact test condition 驗證**。

---

## 3. 為什麼在 2026 年重新啟動 RAIBA

觸發點是中國 2026 年與 retired traction-battery「梯次利用」相關的政策與標準變化討論，包括 Battery100：

https://www.battery100.org/news/tongzhi/0I1203402026.html

研究中最重要的修正：

> **不要**把它簡化成「中國禁止所有 used-battery reuse」。

較可信的 interpretation 是：「梯次利用」原本的特殊 policy classification / whitelist framework 正在移除或調整，而使用 retired traction batteries 製造的產品，越來越需要符合其 target product category 的一般 quality、safety 與 mandatory requirements；部分 application category 另有明確限制。

因此必須分清：

1. formal law / regulation；
2. mandatory standard；
3. ministry notice / policy interpretation；
4. product-strategy inference。

RAIBA 的策略 thesis 應寫成：

> **如果 cross-application second-life reuse 變得更困難、更昂貴、或需要更多 certification，就應在 battery retirement 之前，把原始 asset 的價值盡可能延長。**

一句話：

> **The cheapest and safest second life may be a longer first life.**

---

## 4. Competitive Technology Landscape

後續研究採用以下 taxonomy。

### A. Topology Reconfiguration / Bypass-first

最接近原始 RAIBA。

特徵：

- switch matrix
- module 或 cell enable/bypass
- faulty-member isolation
- topology adaptation
- degradation tolerance

### B. Cell/Module-level DC/DC Power Electronics

特徵：

- independent current/power control
- mismatch compensation
- heterogeneous batteries
- SOH-aware power allocation

Trade-offs：

- semiconductor count
- conversion loss
- EMI
- thermal complexity
- failure points
- cost

### C. Modular Multilevel / Cascaded Converter Battery

特徵：

- battery module 與 power conversion 整合
- dynamic insertion/removal
- 直接合成 HV DC 或 AC
- BMS 與 inverter 功能逐漸融合

### D. Active Balancing / Distributed BMS

可透過降低 SOC mismatch 部分取代 RAIBA 的效益，但通常沒有解除 topology rigidity。

### E. Software / AI Battery Optimization without Reconfiguration

包括 cloud analytics、SOH/RUL、anomaly detection、Digital Twin 等。這些 system 可以 observe、predict、recommend，但一般無法 physically reconfigure battery。

RAIBA 的 differentiation 可以濃縮成：

> **Predict + Decide + Act**，而不是只有 Predict。

---

## 5. Important Benchmarks

2026 research 認定以下為高優先 benchmark。

### 5.1 Relectrify — Direct / Very High Relevance

核心方向：

- cell-level independent monitoring/control
- weak/faulty cell bypass capability
- battery management 與 inverter behavior 整合
- commercial BESS deployment

AC1 公開定位約為 250 kW / 1 MWh，並對大量 cell 執行 individual management。

為什麼重要：

- 直接處理 weakest-cell problem
- 是重要 architecture 與 FTO benchmark

Official references：

- https://www.relectrify.com/
- https://www.relectrify.com/us/patents
- https://arena.gov.au/assets/2024/01/Relectrify-Second-Life-Battery-Trial-Lessons-Learnt-Report.pdf

### 5.2 Element Energy — Direct / Very High Relevance

核心方向：

- distributed adaptive BMS
- battery-module level dedicated power conversion
- mismatch compensation
- independent module power-flow control
- life-cycle energy optimization

這與歷史 RAIBA 的 per-column / per-module DC/DC 方向高度相關。

References：

- https://elementenergy.com/solutions/
- https://elementenergy.com/our-path/
- U.S. DOE second-use project materials

### 5.3 STABL Energy — Direct / High Relevance

核心方向：

- modular multilevel inverter
- low-voltage battery module connect/disconnect
- heterogeneous battery support
- module-level monitoring
- battery 與 inverter architecture convergence

References：

- https://stabl.com/en/technology/
- https://stabl.com/en/technology/working-principle/

### 5.4 Pulsetrain — Direct / High Relevance，尤其 EV

核心方向：

- software-defined powertrain
- BMS + inverter + charger integration
- MOSFET matrix
- runtime cell enable/disable 與 series/parallel sequencing

References：

- https://pulsetrain.com/
- https://pulsetrain.com/technology/

### 5.5 Other Adjacent References

- Brill Power
- B2U Storage Solutions
- Smartville
- Connected Energy
- Moment Energy
- Voltfang
- TWAICE
- ACCURE
- Circunomics

這些案例應仔細區分為 direct architecture competitor、partial substitute、second-life integrator 或 intelligence/software player。

---

## 6. Current RAIBA 2.0 Architecture Thesis

不要只重做 old switch-only system，也不要一開始就在每一個 cell/module 上放完整 converter。

目前建議：

> **Low-frequency Reconfiguration + Selective Bidirectional DC/DC + Deterministic Safety + Battery Digital Twin**

概念：

```text
                 RAIBA 2.0
                     |
        +------------+-------------+
        |            |             |
   Topology       Power        Intelligence
    Control       Control          Layer
        |            |             |
 Enable/Bypass   Selective      Digital Twin
                  DC/DC        SOH/RUL/Risk
```

### 6.1 為什麼是 Selective DC/DC

不是：

```text
100 modules -> 100 DC/DC converters
```

而是研究：

```text
100 modules
  |- healthy modules -> direct path
  `- mismatch/degraded modules -> converter assistance
```

更進一步可以研究 **shared DC/DC resource pool**：

```text
Battery Modules
      |
Switch / Routing Matrix
      |
 +----+----+
 |         |
DC/DC #1  DC/DC #2
```

由 optimizer 決定哪一個 module 取得 converter assistance。

Potential value：

- 保留大部分 power-control benefit
- 降低 converter count
- 降低 BOM 與 conversion loss
- 建立 differentiated architecture/IP space

這是 **2026 engineering hypothesis**，不是 historical RAIBA fact。

---

## 7. Recommended Software / Control Stack

```text
EMS / Cloud
  |- fleet analytics
  |- TCO / warranty
  |- maintenance planning
  `- long-term RUL
        |
RAIBA Intelligence
  |- Digital Twin
  |- SOH / RUL
  |- ISC / anomaly
  `- degradation model
        |
Optimization Engine
  |- module selection
  |- duty allocation
  |- DC/DC allocation
  |- charge strategy
  `- maintenance decision
        |
Safety Supervisor
  |- hard constraints
  |- fault containment
  |- state machine
  `- action validation
        |
Switch Scheduler / Power Controller
        |
RAIBA Hardware
  |- sensors
  |- enable/bypass
  |- contactor/MOSFET network
  `- selective DC/DC
```

Critical principle：

> AI/optimization 永遠不能直接掌控 hard safety loop。

Deterministic control 應負責：

- OV/UV
- OC/short circuit
- OT/UT
- isolation fault
- contactor/MOSFET sequencing
- precharge
- minimum enabled-module count
- bus-voltage window
- fail-safe state

Optimization/MPC 適合：

- module duty assignment
- degradation balancing
- thermal balancing
- selective converter allocation
- charge-power allocation
- load-aware scheduling

ML 更適合：

- SOH
- RUL
- anomaly detection
- ISC detection
- capacity/impedance model correction

---

## 8. Multi-objective Formulation

Optimizer 不應只 maximize usable energy。

可用概念 objective：

```text
maximize:
  delivered energy
  system availability
  remaining useful life

minimize:
  degradation spread
  thermal imbalance
  switching count
  converter loss
  risk
```

概念 cost/reward function：

```text
J =
 + w1 * delivered_energy
 + w2 * availability
 + w3 * RUL
 - w4 * degradation_spread
 - w5 * thermal_imbalance
 - w6 * switching_cost
 - w7 * conversion_loss
 - w8 * risk
```

Hard constraints 永遠優先於 objective。

至少包括：

- `Vmin <= Vi <= Vmax`
- `Imin <= Ii <= Imax`
- `Tmin <= Ti <= Tmax`
- `SOCmin <= SOCi <= SOCmax`
- module power limit
- converter power limit
- minimum series-module requirement
- bus-voltage window
- switch current/voltage rating
- precharge state
- zero/low-current switching condition
- isolation constraint
- fault exclusion

---

## 9. Scheduler Time Scales

不同 control loop 應分層：

```text
µs-ms       hardware protection / short-circuit response
ms-100 ms   BMS protection and fast power control
s-min       topology scheduling / bypass / duty rotation
min-hours   degradation and thermal optimization
hours-days  lifetime / maintenance planning
```

不要把所有 decision 都丟進同一個 AI scheduler。

---

## 10. Application Hypotheses

每個 application 應用一致 scoring framework：

- battery replacement cost
- weakest-member pain
- lifetime-extension potential
- availability value
- tolerance for extra power electronics
- certification difficulty
- software value
- maintainability
- market size
- willingness to pay
- development cycle

目前 priority hypothesis：

### P0 — New-battery C&I / Utility ESS

原因：

- battery asset value 高
- 10–20 year service target
- mismatch 不可避免
- 額外 electronics 的容忍度比 EV 高
- maintenance 與 software 有清楚 TCO value

### P0 — UPS / Data Center / Telecom

原因：

- availability 是 first-class KPI
- graceful degradation 與 fault isolation 有直接 business value

### P1 — Swap / Battery-as-a-Service

原因：

- modules 天然累積不同 history
- centralized charging 與 fleet operation 提供 data/control opportunity

### P1 — Industrial Fleets

例如：

- AGV / AMR
- forklift
- mining
- port equipment
- commercial vehicles

原因：

- downtime 成本高
- battery 成本高
- maintenance 可集中管理

### P2 — EV Traction

價值高但困難，因為：

- automotive qualification
- ASIL / functional safety
- crash/HV/isolation requirements
- weight 與 cost constraint
- OEM integration cycle

### Aviation / Drone

除非特殊 niche 足以支撐 certification burden，否則先視為 long-term research。

### Second-life ESS

視 region / regulation / product category 而定。不要把 RAIBA 2.0 business economics 建立在「可以無限制取得便宜 used batteries」這個假設上。

---

## 11. 12-Month Prototype Hypothesis

建議第一個 prototype：

> **RAIBA 2.0 C&I / UPS Battery Rack Prototype**

不要先從 EV 開始。

合理的 initial platform hypothesis：

- 8–16 modules
- 例如 16S1P 或 8S2P module-level platform
- 約 48–120 VDC
- 約 5–20 kW
- 約 10–50 kWh

這些是 engineering hypotheses，應依 lab equipment 與 available modules 調整。

同一 battery population 應測試三個 configuration：

### Baseline A

- fixed topology
- conventional BMS

### Baseline B

- fixed topology
- conventional BMS + active balancing

### RAIBA

- Enable/Bypass
- Health-aware Scheduler
- Selective Bidirectional DC/DC

刻意建立 mismatch：

- capacity mismatch
- resistance mismatch
- SOC mismatch
- temperature mismatch
- weak module
- communication fault
- sensor fault
- fault proxy / fault injection

Key KPIs：

- usable energy before cutoff
- availability / uptime
- degradation spread
- thermal spread
- conversion efficiency
- switching count
- fault recovery time
- lifetime delivered energy / lifetime throughput

North-star metric：

> **Lifetime Safe Delivered Energy**

第二個 north-star metric：

> **System Availability over Lifetime**

商業問題不是某一個 cycle 是否多 0.5% efficiency，而是 system 在完整 lifetime 能否交付顯著更多 safe energy 與 service。

---

## 12. Kill Criteria / Decision Thresholds

Prototype 必須有明確 stopping rules。

例如：

- hardware + converter + switching + maintenance cost 高於 lifetime-value gain，則 pivot 或停止；
- realistic module mismatch 只有 negligible lifetime-energy benefit，architecture 可能不值得；
- control complexity 或 failure probability 壓過 availability benefit，應簡化 architecture；
- 若 FTO analysis 顯示 core architecture 被阻擋，轉向 differentiated allocation/control layer。

值得探索的 project target（**不是歷史結果**）：

- <5% lifetime-energy gain：business case 弱
- >10%：值得繼續開發
- >20%：具吸引力
- >30%：可能具 disruptive potential

這些是 project decision thresholds，不是 validated RAIBA performance claims。

---

## 13. Product Vision — Long-Life Battery Chassis

Traditional battery pack model：

```text
Pack -> Age -> Degrade -> Replace Pack
```

RAIBA product model：

```text
Long-Life Battery Chassis
  |- replaceable battery modules
  |- reconfigurable switching fabric
  |- selective power-electronic resources
  |- controller / Battery OS
  `- Digital Twin / lifecycle software
```

Chassis、power stage、BMS/controller 與 battery modules 可以有不同 service life。

因此可形成 hardware sales 之外的 business model：

- RAIBA controller / switching hardware
- selective DC/DC hardware
- Battery OS license
- fleet analytics SaaS
- warranty intelligence
- maintenance planning
- availability / lifetime-throughput performance contract

---

## 14. IP / FTO Direction

不要假設 simple bypass switching 還有足夠 patentability；這個領域已有大量 prior art。

Potential differentiating IP combination：

```text
State Estimation
   -> Health/Risk Model
   -> Topology Selection
   -> Selective or Shared DC/DC Allocation
   -> Safe Switching Sequence
   -> Lifetime Optimization
```

Possible patent themes：

1. health-aware topology reconfiguration
2. selective/shared bidirectional DC/DC allocation
3. mixed CC/CV operation in a reconfigurable series architecture
4. degradation-aware battery duty scheduling
5. safe bypass/rejoin sequences
6. digital-twin-driven reconfiguration
7. lifetime-energy optimization
8. maintainable long-life battery chassis

FTO priority companies/areas：

- Relectrify
- Element Energy
- STABL Energy
- Pulsetrain
- modular multilevel battery patents
- cell-switching patents
- battery-integrated inverter patents
- distributed DC/DC BMS patents

Architecture freeze 前需要 proper claim-level patent timeline。

---

## 15. Facts vs Hypotheses vs Unknowns

| Item | Status | Note |
|---|---|---|
| RAIBA dynamically reconfigurable battery array | Prior RAIBA fact | prior material 已確認 |
| Per-module Enable/Bypass | Prior RAIBA fact | 已確認 |
| Charge/discharge research split | Prior RAIBA fact | 已確認 |
| Mixed CC/CV direction | Prior RAIBA fact | 已確認 |
| Per-column DC/DC concept | Prior RAIBA fact | 已確認 |
| EIS/RC/SOH-aware control | Prior RAIBA fact | 已確認 |
| Lifetime prediction / micro-short work | Prior RAIBA fact | 已確認 |
| 3S2P demo | Prior material | 需重查原始 artifact |
| 5S5P simulation | Prior material | 需重查原始 artifact |
| ~75% disparity / ~98% utilization | Prior material | exact condition 必須重新驗證 |
| Relectrify cell-level adaptive architecture | 2026 research | high confidence |
| Element module-level conversion | 2026 research | high confidence |
| STABL modular multilevel approach | 2026 research | high confidence |
| Pulsetrain software-defined powertrain | 2026 research | high confidence |
| “China bans all used battery reuse” | incorrect/overstatement | 不可使用 |
| first-life extension as strategic focus | strategy hypothesis | strong current thesis |
| selective/shared DC/DC | engineering hypothesis | 需 prototype |
| Long-Life Battery Chassis | product hypothesis | 需 TCO/customer interview 驗證 |
| C&I/UPS as first MVP | current hypothesis | 需 commercial validation |

---

## 16. 不可自行捏造的 Unknowns

在原始 source files / repository 取回之前，不得宣稱知道：

- original RAIBA source code
- original state machine
- original optimization cost function
- EBM/SPM schematics
- exact switch topology
- MOSFET/relay part numbers
- historical CAN protocol
- exact switching waveform
- exact validation environment
- exact definition of 75% mismatch
- exact method behind 98% utilization
- historical efficiency / BOM
- existing patent-claim scope

---

## 17. Recommended Repo Structure

```text
RAIBA/
├── README.md
├── handoff.md
├── docs/
│   ├── architecture/
│   │   ├── RAIBA2_ARCHITECTURE.md
│   │   ├── SWITCH_TOPOLOGY.md
│   │   └── SELECTIVE_DCDC.md
│   ├── algorithm/
│   │   ├── OPTIMIZATION.md
│   │   ├── STATE_MACHINE.md
│   │   └── SAFETY_CONSTRAINTS.md
│   ├── market/
│   │   ├── COMPETITORS.md
│   │   └── APPLICATION_MATRIX.md
│   ├── regulation/
│   │   └── BATTERY_REUSE_2026.md
│   ├── ip/
│   │   ├── PATENT_LANDSCAPE.md
│   │   └── FTO.md
│   ├── prototype/
│   │   ├── MVP_SPEC.md
│   │   ├── TEST_PLAN.md
│   │   └── ACCEPTANCE_CRITERIA.md
│   ├── research/
│   └── sources/
├── simulation/
├── firmware/
├── hardware/
├── data/
└── experiments/
```

---

## 18. Recommended Next-Session Modes

新的 AI session 應先讀完本檔，再進入以下模式之一。

### MODE A — Architecture

設計 RAIBA 2.0 switching、selective DC/DC、electrical interface 與 safety architecture。

### MODE B — Algorithm

建立 module-selection、degradation-aware optimization、scheduler、state machine 與 Digital Twin。

### MODE C — Prototype

定義 8–16 module prototype、BOM、topology、test matrix 與 acceptance criteria。

### MODE D — Patent/FTO

針對 Relectrify、STABL、Element、Pulsetrain 與相關 patent 做 claim-level landscape。

### MODE E — Market/TCO

建立 C&I ESS、UPS/Data Center、Telecom、Swap/BaaS 與 industrial fleet 的 customer/TCO case。

### MODE F — Full Project

建立整合式 12/24/36-month R&D plan 與 repository artifacts。

---

## 19. Recommended Opening Prompt for a New Session

```text
我正在延續一個名為 RAIBA 2.0 的 battery technology project。
請先完整讀完 handoff.zh-TW.md，再開始新工作。

這個專案不是 conventional active balancing；核心是 health-aware dynamically reconfigurable battery architecture。

Current product thesis:
Low-frequency Reconfiguration + Selective Bidirectional DC/DC + Deterministic Safety + Battery Digital Twin.

Primary business objective:
first-life / in-situ asset life extension，而不是依賴 second-life reuse。

所有分析都必須區分四個 evidence layer：
1. Prior RAIBA Facts
2. 2026 External Research Findings
3. Engineering Hypotheses
4. Unknowns

不要把 hypothesis 寫成 historical fact。
對 current market、law、standard、company、product 或 patent，要重新確認最新 primary source。
所有技術產出應包含 assumptions、architecture、interfaces、algorithms、tests、acceptance criteria、risks 與 kill criteria。
```

---

## 20. Project North Star

如果 handoff 最後只能保留一個觀念，請保留：

> **RAIBA 2.0 要讓 weakest battery member 失去「過早迫使整套昂貴 system retirement」的權力。**

主要用以下指標衡量成功：

1. **Lifetime Safe Delivered Energy**
2. **System Availability over Lifetime**

如果 RAIBA 能在可接受的 cost、complexity 與 safety risk 下顯著提升這兩個指標，它就不只是一個 BMS feature，而是一套 battery-system architecture 與 asset-management platform。
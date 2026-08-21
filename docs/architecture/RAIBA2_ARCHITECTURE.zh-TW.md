# RAIBA 2.0 Architecture Thesis

> English source: [`RAIBA2_ARCHITECTURE.md`](./RAIBA2_ARCHITECTURE.md)
>
> Evidence class：**2026 engineering hypothesis**，建立於歷史 RAIBA baseline 與市場研究之上。

## 1. 建議方向

目前建議的 architecture 為：

> **RAIBA 2.0 = Low-frequency Reconfiguration + Selective Bidirectional DC/DC + Deterministic Safety + Battery Digital Twin**

設計意圖是避免兩個極端：

- 不只停留在 binary bypass-only architecture；
- 也不預設每一個 cell/module 都配置完整 converter，承擔不必要的 cost、loss 與 reliability penalty。

## 2. 三種 Architecture Option

### A. Switch-only RAIBA

```text
Module -> Enable/Bypass switch -> shared string/bus
```

**優點**
- converter cost 最低；
- 若設計良好，可維持低 conduction/conversion overhead；
- 可對弱化或 faulty module 做 graceful isolation；
- 最接近原始 RAIBA DNA。

**缺點**
- control granularity 較粗；
- module 被移除後，bus/string voltage 會改變；
- 獨立控制 module current 的能力有限；
- 在高 power demand 下，較難充分利用 heterogeneous modules。

### B. Hybrid RAIBA 2.0 — 建議方案

```text
                   +-------------------------+
Battery Modules -->| Reconfigurable Fabric  |-----> DC Bus / PCS / UPS
                   +-------------------------+
                          |          |
                          |          +---- direct paths
                          |
                          +---- Selective Bidirectional DC/DC Pool
```

健康 module 可走高效率 direct path；degraded、mismatched，或因策略需要被選中的 module，只有在能產生實際價值時才獲得 converter assistance。

可能的延伸：

```text
Modules 1..N
    |
Switch / Routing Matrix
    |
+---+------------------+
|                      |
Direct Path        Shared DC/DC #1..M
                       M << N
```

這會新增一個重要 optimization variable：**在某一時間點，哪一個 module 應取得稀缺的 converter resource？**

### C. Converter-dominant RAIBA 3.0

```text
Each Module -> dedicated bidirectional converter -> common bus / waveform synthesis
```

**優點**
- 最大 power controllability；
- 對 heterogeneous modules 支援能力強；
- 可獨立控制 current/power；
- 有機會整合 BMS 與 inverter function。

**缺點**
- semiconductor count 增加；
- cost 增加；
- switching loss；
- EMI；
- thermal design 複雜；
- reliability 與 certification burden 增加。

這個 architecture 技術能力最強，但不能因此假設它就是經濟上最適合 RAIBA 的實作方式。

## 3. 建議 Control Stack

```text
+-------------------------------------------------+
| EMS / Fleet / Cloud                            |
| TCO, warranty, maintenance, fleet analytics    |
+-----------------------+-------------------------+
                        |
                        v
+-------------------------------------------------+
| Battery Digital Twin / Intelligence            |
| SOH, RUL, anomaly, ISC risk, degradation model |
+-----------------------+-------------------------+
                        |
                        v
+-------------------------------------------------+
| Optimization Engine                             |
| duty allocation, topology selection, DC/DC use |
+-----------------------+-------------------------+
                        |
                        v
+-------------------------------------------------+
| Deterministic Safety Supervisor                 |
| hard limits, state machine, transition checks  |
+-----------------------+-------------------------+
                        |
                        v
+-------------------------------------------------+
| Switch Scheduler / Power Controller             |
+-----------------------+-------------------------+
                        |
                        v
+-------------------------------------------------+
| Hardware: sensing, switches, contactors, DC/DC  |
+-------------------------------------------------+
```

## 4. Safety Principle

AI/ML 在未經 deterministic validation 前，**絕對不能直接控制 switching hardware**。

```text
AI / Optimizer
     |
     v
Proposed Action
     |
     v
Safety Supervisor
     |
     +-- voltage limits
     +-- current limits
     +-- thermal limits
     +-- topology validity
     +-- isolation state
     +-- precharge / transition rules
     +-- switch ratings
     |
     v
Switch Scheduler
     |
     v
Hardware
```

## 5. Deterministic 與 Optimization / ML 的分工

### Deterministic Safety Layer

必須負責：

- over/under-voltage；
- over-current；
- over-temperature；
- short-circuit response；
- isolation fault；
- contactor/MOSFET sequencing；
- precharge；
- 必要時的 low/zero-current transition rule；
- minimum enabled-module constraint；
- DC bus voltage window；
- fail-safe state。

### Optimization / MPC Candidate

適合用於：

- module duty assignment；
- degradation balancing；
- thermal balancing；
- charge-power allocation；
- selective DC/DC allocation；
- future-load-aware scheduling；
- lifetime-energy optimization。

### ML Candidate

在 model-based estimator 不足，或 data 能提供額外價值時，可用於：

- SOH / RUL；
- anomaly detection；
- micro-short indicator；
- degradation residual；
- capacity / impedance pattern estimation。

ML 應提供 state/risk estimate，而不能繞過 hard safety constraint。

## 6. Multi-objective Control Formulation

可採用的概念 objective：

```text
maximize:
  delivered energy
  system availability
  remaining useful life

minimize:
  degradation spread
  thermal imbalance
  switching count
  conversion loss
  safety risk
```

概念上：

```text
J = + w1*DeliveredEnergy
    + w2*Availability
    + w3*RUL
    - w4*DegradationSpread
    - w5*ThermalImbalance
    - w6*SwitchingCost
    - w7*ConversionLoss
    - w8*Risk
```

並受下列 hard constraints 限制：

- `Vmin <= Vi <= Vmax`
- `Imin <= Ii <= Imax`
- `Tmin <= Ti <= Tmax`
- `SOCmin <= SOCi <= SOCmax`
- module 與 converter power limit
- minimum active-series-module count
- DC-bus voltage window
- switch voltage/current rating
- precharge / safe-transition condition
- isolation 與 fault-exclusion constraint

## 7. Control Time Scales

不同 decision 應分屬不同時間尺度。

| Layer | 典型概念時間尺度 | Role |
|---|---:|---|
| Hardware protection | us–ms | short circuit / destructive fault |
| BMS protection & power loop | ms–100 ms | current/voltage/thermal regulation |
| Topology scheduler | seconds–minutes | enable/bypass/rejoin/duty rotation |
| Degradation optimizer | minutes–hours | aging/thermal/lifetime allocation |
| Maintenance planner | hours–days+ | service and replacement planning |

精確數值必須依 hardware 與 application requirement 定義。

## 8. Fault State Machine Concept

```text
INIT
 |
 v
SELF_TEST
 |
 v
NORMAL -------------------+
 |                         |
 +--> DEGRADED             +--> EMERGENCY_SHUTDOWN
 |      |                  |
 |      +--> DERATE        |
 |      +--> DC/DC_ASSIST  |
 |      +--> BYPASS -------+
 |      +--> ISOLATE
 |
 +--> MAINTENANCE_REQUIRED
        |
        +--> RECOVERY / REJOIN after validation
```

## 9. North-Star Design Metric

Architecture 不應只針對 single-cycle efficiency 最佳化。

Primary system metrics 應為：

> **Lifetime Safe Delivered Energy**

以及：

> **System Availability over Lifetime**

若小幅 efficiency penalty 能顯著提升 lifetime throughput，並避免過早 rack/pack replacement，從經濟角度可能完全合理。
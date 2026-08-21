# RAIBA 2.0 — 12 個月 MVP / Prototype Hypothesis

> English source: [`RAIBA2_MVP_SPEC.md`](./RAIBA2_MVP_SPEC.md)
>
> 狀態：engineering proposal，**不是**歷史 RAIBA specification。

## 1. 建議的第一個 Target

建議先做 **C&I ESS / UPS-oriented rack-scale prototype**。

為什麼不先做 EV：

- automotive qualification 與 functional-safety burden 高；
- HV isolation 與 crash requirement；
- packaging / weight / cost constraint；
- OEM integration cycle 長。

C&I ESS / UPS 提供較乾淨的環境，可優先證明 RAIBA 最根本的 value proposition：

- weak-module tolerance；
- graceful degradation；
- higher availability；
- recovered usable energy；
- increased lifetime energy throughput；
- selective power-electronic assistance。

## 2. Prototype Scale

建議起始 architecture：

- **8–16 battery modules**；
- candidate topology：module-level **16S1P** 或 **8S2P**；
- conceptual DC range：**48–120 VDC**；
- conceptual power：**5–20 kW**；
- conceptual energy：**10–50 kWh**。

這些數值都是 hypothesis。最終規格應依可取得 module、lab safety limit、Chroma/load capability、switching device 與 converter availability 決定。

## 3. 三種必要 Comparison Configuration

三種 case 應使用相同或等效的 battery population。

### Baseline A — Fixed Topology

```text
Traditional BMS + fixed series/parallel topology
```

### Baseline B — Fixed Topology + Active Balancing

```text
Traditional BMS + active balancing
```

### RAIBA 2.0

```text
Enable / Bypass
+ Health-aware Scheduler
+ Selective Bidirectional DC/DC
+ Deterministic Safety Supervisor
```

如果沒有這些 baseline，RAIBA 就無法證明 incremental value。

## 4. Hardware Blocks

```text
Battery Modules
    |
    +--> V/I/T sensing
    |
    +--> Enable / Bypass switching stage
    |
    +--> optional routing matrix
    |
    +--> selective/shared bidirectional DC/DC
    |
    v
DC Bus
    |
    v
Programmable load / charger / PCS emulator

Controller stack:
BMS -> State Estimator -> Optimizer -> Safety Supervisor -> Switch Scheduler
```

## 5. Intentional Heterogeneity / Fault Injection

Prototype 不能只用 matched new modules 測試。

必須刻意建立 controlled mismatch，例如：

- capacity：100%、95%、90%、85%、80%、70%、60%；
- SOC offset；
- elevated DCIR；
- temperature mismatch；
- weak module；
- sensor bias / dropout；
- CAN / communication loss；
- 在安全條件允許下的 contactor/switch failure mode；
- simulated 或 proxy internal-short/anomaly signature。

目的就是重建 fixed battery topology 在真實世界中逐漸變得經濟效率低落的情境。

## 6. Core Experiments

### E1 — Weakest-member Usable-Energy Test

比較 fixed topology 與 RAIBA 在 system cutoff 前可交付的 kWh。

### E2 — Graceful Degradation

導入 weak/faulted module，量測系統是否能將它 isolation，並持續在安全的 derated operating point 運作。

### E3 — Selective DC/DC Allocation

量化只有有限 converter resource 時，把 converter 配置給特定 mismatched modules 所產生的價值。

### E4 — Charge Optimization

比較 fixed-string charging 與 health/state-aware charging + selective assistance。

### E5 — Accelerated Aging

重複執行 duty cycle，觀察 RAIBA 是否能降低 degradation spread，或在定義的 EOL 前增加 total energy throughput。

### E6 — Fault Injection / Fail-safe

測試 controller、sensor、communication 與 switching fault，確認系統能以 deterministic 方式進入 safe state。

## 7. KPI Set

| KPI | Definition |
|---|---|
| Usable Energy | 在必要 cutoff 前可交付的 kWh |
| Availability | requested operating time 中可正常提供服務的比例 |
| Lifetime Throughput | 在定義 system EOL 前累積交付的 MWh |
| Degradation Spread | modules 間 SOH / capacity / resistance 的差異 |
| Thermal Spread | representative duty 下的 delta T |
| Conversion Efficiency | 包含 DC/DC 使用時的 battery-to-bus efficiency |
| Switching Cost | topology transition 次數及其相關 loss/stress |
| Fault Recovery Time | 從 fault 可被偵測到穩定 degraded operation 的時間 |
| Safety Violations | 在接受的 test envelope 內必須為 0 |
| Maintenance Events | test horizon 內所需的 module/system intervention 次數 |

## 8. North-Star Acceptance Target

關鍵比較不是 instantaneous balancing speed，而是：

> **Lifetime Safe Delivered Energy versus conventional BMS architecture**

可採用的 project decision framework（hypothesis，非量測結果）：

- **<5%** lifetime-energy improvement：business case 弱，kill 或 pivot；
- **>10%**：視 BOM/TCO 而定，可能具可行性；
- **>20%**：有強烈 commercial interest；
- **>30%**：若可重現且安全，可能改變 battery architecture。

這些 threshold 只是 management target，仍必須做 TCO validation。

## 9. Functional Acceptance Criteria

MVP 應證明：

- 可對任一受支援 module 執行 bypass；
- safe module rejoin；
- 所有 commanded transition 都保持 valid topology；
- controlled transient behavior；
- weak/faulty module isolation；
- 不必要 full shutdown 前能 graceful derating；
- optimizer action 無法違反 deterministic safety constraint；
- controller / communication / sensor failure 時可進入 safe state。

## 10. 12 個月 Execution Sketch

### Month 0–3

- architecture freeze；
- patent landscape / FTO review；
- simulation model；
- battery aging/mismatch dataset；
- switch-stage bench prototype；
- safety state machine。

### Month 3–6

- 8–16 module rig；
- Enable/Bypass control；
- fault injection；
- Health-aware Scheduler；
- baseline comparison automation。

### Month 6–9

- selective/shared bidirectional DC/DC；
- Digital Twin / state-estimation integration；
- charge optimization；
- accelerated-aging campaign。

### Month 9–12

- repeatability study；
- lifetime-energy 與 TCO analysis；
- safety/failure report；
- IP capture；
- 決定是否進入 50–200 kWh pilot。

## 11. Kill Criteria

若經過可信的 engineering optimization 後，下列情況仍成立，應停止、重新設計或 pivot：

- hardware + converter + maintenance cost 高於 lifetime-extension value；
- realistic mismatch 只能恢復非常有限的 lifetime energy；
- reconfiguration 帶來無法接受的 reliability 或 safety risk；
- conversion/switching loss 抵消 usable-energy advantage；
- architecture 無法通過目標市場 certification；
- FTO constraint 消除原本預期的 differentiation。
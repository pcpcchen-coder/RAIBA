# RAIBA 2.0 Architecture Thesis

> Evidence class: **2026 engineering hypothesis**, built on the historical RAIBA baseline and market research.

## 1. Recommended direction

The current recommended architecture is:

> **RAIBA 2.0 = Low-frequency Reconfiguration + Selective Bidirectional DC/DC + Deterministic Safety + Battery Digital Twin**

The intent is to avoid both extremes:

- not stopping at a binary bypass-only architecture;
- not paying the cost, loss and reliability penalty of placing a full converter on every cell/module by default.

## 2. Three architecture options

### A. Switch-only RAIBA

```text
Module -> Enable/Bypass switch -> shared string/bus
```

**Strengths**
- lowest converter cost;
- low conduction/conversion overhead if implemented well;
- graceful isolation of weak/faulty modules;
- closest to original RAIBA DNA.

**Weaknesses**
- coarse control;
- bus/string voltage changes when modules are removed;
- limited ability to independently control module current;
- difficult to fully exploit heterogeneous modules under demanding power conditions.

### B. Hybrid RAIBA 2.0 — recommended

```text
                   +-------------------------+
Battery Modules -->| Reconfigurable Fabric  |-----> DC Bus / PCS / UPS
                   +-------------------------+
                          |          |
                          |          +---- direct paths
                          |
                          +---- Selective Bidirectional DC/DC Pool
```

Healthy modules can use efficient direct paths. Degraded, mismatched or strategically selected modules receive converter assistance only when useful.

Potential extension:

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

This creates a new optimization variable: **which module receives scarce converter resources at a given time**.

### C. Converter-dominant RAIBA 3.0

```text
Each Module -> dedicated bidirectional converter -> common bus / waveform synthesis
```

**Strengths**
- maximum power controllability;
- strong support for heterogeneous modules;
- independent current/power regulation;
- potentially integrates BMS and inverter functions.

**Weaknesses**
- semiconductor count;
- cost;
- switching loss;
- EMI;
- thermal design;
- reliability and certification burden.

This architecture is technically powerful but should not be assumed to be the economically optimal RAIBA implementation.

## 3. Recommended control stack

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

## 4. Safety principle

AI/ML must never directly command switching hardware without deterministic validation.

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

## 5. Deterministic versus optimization / ML

### Deterministic safety layer

Must own:

- over/under-voltage;
- over-current;
- over-temperature;
- short-circuit response;
- isolation fault;
- contactor/MOSFET sequencing;
- precharge;
- low/zero-current transition rules where required;
- minimum enabled-module constraints;
- DC bus voltage window;
- fail-safe state.

### Optimization / MPC candidates

Useful for:

- module duty assignment;
- degradation balancing;
- thermal balancing;
- charge-power allocation;
- selective DC/DC allocation;
- future-load-aware scheduling;
- lifetime-energy optimization.

### ML candidates

Useful when model-based estimators become insufficient or when data adds value:

- SOH / RUL;
- anomaly detection;
- micro-short indicators;
- degradation residuals;
- capacity / impedance pattern estimation.

ML should provide state/risk estimates, not bypass hard safety constraints.

## 6. Multi-objective control formulation

A useful conceptual objective is:

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

Conceptually:

J = + w1*DeliveredEnergy
    + w2*Availability
    + w3*RUL
    - w4*DegradationSpread
    - w5*ThermalImbalance
    - w6*SwitchingCost
    - w7*ConversionLoss
    - w8*Risk

Subject to hard constraints:

- Vmin <= Vi <= Vmax
- Imin <= Ii <= Imax
- Tmin <= Ti <= Tmax
- SOCmin <= SOCi <= SOCmax
- module and converter power limits
- minimum active-series-module count
- DC-bus voltage window
- switch voltage/current ratings
- precharge / safe-transition conditions
- isolation and fault-exclusion constraints

## 7. Control time scales

Different decisions should live at different time scales.

| Layer | Typical conceptual timescale | Role |
|---|---:|---|
| Hardware protection | us–ms | short circuit / destructive fault |
| BMS protection & power loop | ms–100 ms | current/voltage/thermal regulation |
| Topology scheduler | seconds–minutes | enable/bypass/rejoin/duty rotation |
| Degradation optimizer | minutes–hours | aging/thermal/lifetime allocation |
| Maintenance planner | hours–days+ | service and replacement planning |

Exact values must be defined by hardware and application requirements.

## 8. Fault state machine concept

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

## 9. North-star design metric

The architecture should not be optimized only for single-cycle efficiency.

Primary system metrics should be:

> **Lifetime Safe Delivered Energy**

and

> **System Availability over Lifetime**

A small efficiency penalty may be economically justified if it substantially increases lifetime throughput and avoids premature rack/pack replacement.

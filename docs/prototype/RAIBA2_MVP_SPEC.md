# RAIBA 2.0 — 12-Month MVP / Prototype Hypothesis

> Status: engineering proposal, not historical RAIBA specification.

## 1. Recommended first target

**C&I ESS / UPS-oriented rack-scale prototype**.

Why not EV first:

- automotive qualification and functional-safety burden;
- HV isolation and crash requirements;
- packaging / weight / cost constraints;
- long OEM integration cycles.

C&I ESS / UPS gives a cleaner environment to prove the fundamental RAIBA value proposition:

- weak-module tolerance;
- graceful degradation;
- higher availability;
- recovered usable energy;
- increased lifetime energy throughput;
- selective power-electronic assistance.

## 2. Prototype scale

Recommended starting architecture:

- **8–16 battery modules**;
- candidate topology: **16S1P** or **8S2P** at module level;
- conceptual DC range: **48–120 VDC**;
- conceptual power: **5–20 kW**;
- conceptual energy: **10–50 kWh**.

These values are hypotheses. Final numbers should be selected based on available modules, lab safety limits, Chroma/load capability, switching devices and converter availability.

## 3. Three mandatory comparison configurations

Use the same or equivalent battery population for all cases.

### Baseline A — Fixed topology

```text
Traditional BMS + fixed series/parallel topology
```

### Baseline B — Fixed topology + active balancing

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

Without these baselines, RAIBA cannot prove incremental value.

## 4. Hardware blocks

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

## 5. Intentional heterogeneity / fault injection

The prototype must not be tested only with matched new modules.

Create controlled mismatch such as:

- capacity: 100%, 95%, 90%, 85%, 80%, 70%, 60%;
- SOC offset;
- elevated DCIR;
- temperature mismatch;
- weak module;
- sensor bias / dropout;
- CAN / communication loss;
- contactor/switch failure mode where safe;
- simulated or proxy internal-short/anomaly signatures.

The purpose is to recreate the real condition that makes fixed battery topology economically inefficient.

## 6. Core experiments

### E1 — Weakest-member usable-energy test

Compare delivered kWh before system cutoff under fixed topology versus RAIBA.

### E2 — Graceful degradation

Introduce a weak/faulted module and measure whether the system can isolate it and continue operating at a safe derated point.

### E3 — Selective DC/DC allocation

Measure the value of assigning limited converter resources to only selected mismatched modules.

### E4 — Charge optimization

Compare fixed-string charging with health/state-aware charging and selective assistance.

### E5 — Accelerated aging

Run repeated duty cycles to observe whether RAIBA reduces degradation spread or increases total energy throughput before defined EOL.

### E6 — Fault injection / fail-safe

Test controller, sensor, communication and switching faults and verify deterministic transition to a safe state.

## 7. KPI set

| KPI | Definition |
|---|---|
| Usable Energy | kWh delivered before required cutoff |
| Availability | fraction of requested operating time serviceable |
| Lifetime Throughput | cumulative MWh before defined system EOL |
| Degradation Spread | delta SOH / capacity / resistance across modules |
| Thermal Spread | delta T under representative duty |
| Conversion Efficiency | battery-to-bus efficiency including DC/DC where used |
| Switching Cost | number of topology transitions and associated loss/stress |
| Fault Recovery Time | time from detectable fault to stable degraded operation |
| Safety Violations | must be zero under accepted test envelope |
| Maintenance Events | module/system interventions required per test horizon |

## 8. North-star acceptance target

The key comparison is not instantaneous balancing speed. It is:

> **Lifetime Safe Delivered Energy versus conventional BMS architecture.**

A useful project decision framework (hypothesis, not measured result):

- **<5%** lifetime-energy improvement: weak business case; kill or pivot;
- **>10%**: potentially viable depending on BOM/TCO;
- **>20%**: strong commercial interest;
- **>30%**: potentially architecture-changing if repeatable and safe.

These thresholds are management targets only and require TCO validation.

## 9. Functional acceptance criteria

The MVP should demonstrate:

- arbitrary supported module bypass;
- safe module rejoin;
- valid topology at all commanded transitions;
- controlled transient behavior;
- weak/faulty module isolation;
- graceful derating rather than unnecessary full shutdown;
- optimizer actions unable to violate deterministic safety constraints;
- safe state under controller/communication/sensor failure.

## 10. 12-month execution sketch

### Month 0–3

- architecture freeze;
- patent landscape / FTO review;
- simulation model;
- battery aging/mismatch dataset;
- switch-stage bench prototype;
- safety state machine.

### Month 3–6

- 8–16 module rig;
- enable/bypass control;
- fault injection;
- health-aware scheduler;
- baseline comparison automation.

### Month 6–9

- selective/shared bidirectional DC/DC;
- digital-twin/state-estimation integration;
- charge optimization;
- accelerated-aging campaign.

### Month 9–12

- repeatability study;
- lifetime-energy and TCO analysis;
- safety/failure report;
- IP capture;
- decision on 50–200 kWh pilot.

## 11. Kill criteria

Stop, redesign or pivot if any of the following remain true after credible engineering optimization:

- hardware + converter + maintenance cost exceeds lifetime-extension value;
- realistic mismatch produces negligible recovered lifetime energy;
- reconfiguration introduces unacceptable reliability or safety risk;
- conversion/switching losses erase the usable-energy advantage;
- architecture cannot be certified for the selected target market;
- FTO constraints eliminate the intended differentiation.

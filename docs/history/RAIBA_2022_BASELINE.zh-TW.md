# RAIBA 2022 技術 Baseline — 去機密化重建

> English source: [`RAIBA_2022_BASELINE.md`](./RAIBA_2022_BASELINE.md)
>
> 狀態：依據先前 ChatGPT session 中檢視過的歷史 RAIBA 資料重建。
> Evidence class：除非明確標示為 inference，否則屬於 **Prior RAIBA material**。
> Security note：本文件刻意排除 company-confidential screenshot、人名、內部組織資訊與原始 source files。

## 1. 定義

RAIBA 曾被描述為 **dynamically reconfigurable battery array architecture and management algorithm**，並使用過 **Reconfigurable Array of Inexpensive Battery Architecture** 這個名稱。

核心概念是：避免單一弱化 cell/module 決定整個固定 battery string 的 usable energy、power capability、availability 或 End-of-Life（EOL）。

## 2. 核心 Topology Concept

關鍵 actuator 概念為 **per-module Enable / Bypass**。

```text
Battery Module i
      |
      v
+------------------+
| Enable / Bypass  |
+------------------+
      |
      +----> active electrical path
      |
      +----> bypass path
```

RAIBA Controller 可以依照 battery state 與 system requirement，選擇哪些 module 參與目前有效的 series/parallel battery array。

## 3. 歷史 Control Directions

### 3.1 Discharge Optimization

過去規劃曾把 discharge-side 工作獨立列為一個技術方向。已知主題包括：

- 即時 battery-state measurement；
- SOC / SOH aware decision；
- EIS / RC / ECM 相關 estimation concept；
- health-aware Enable / Bypass；
- linear / nonlinear switching behavior；
- switching timing；
- measurement frequency；
- surge / transient handling；
- current limiting。

概念 control loop：

```text
V / I / T / SOC / SOH / impedance-related state
                    |
                    v
             State Estimator
                    |
                    v
          Health-aware Scheduler
                    |
                    v
          Enable / Bypass Decision
                    |
                    v
          Optimized Discharge Path
```

### 3.2 Charge Optimization

另一條歷史方向是 **Mixed CC/CV Operations for Batteries Connected in Series**。

主要問題是 fixed series string 中所有 member 共用同一電流，但不同 cell/module 的 SOC、capacity、resistance、temperature 與 aging state 可能存在明顯差異。某些較健康的 member 仍具 charge acceptance 時，限制性 member 可能已先到達 voltage 或 thermal limit。

因此當時的技術方向，是建立 battery-state-aware charging，而不是把整個 string 視為單一、均質的 electrochemical object。

### 3.3 DC/DC 與 Current Limiting

歷史規劃也曾提到 **per-column DC/DC conversion and current limiting source**。

這代表 RAIBA 的發展已從純 binary insertion/bypass，開始走向具備兩個獨立 control dimension 的系統：

```text
Topology control:   Enable / Bypass / Connect / Disconnect
Power control:      Current / Voltage / Power / Charge rate
```

## 4. RAIBA 與 Conventional BMS 的差異

Conventional BMS 通常是在 **fixed topology** 中執行 monitor、protect、estimate 與 balance。

RAIBA 增加了另一個可控制變數：**topology itself**。

```text
Traditional BMS

Fixed topology -> Monitor / Protect / Estimate / Balance

RAIBA

Battery state -> Estimate -> Topology decision -> Enable / Bypass / Power allocation
```

因此，RAIBA 不應被縮減理解成 active balancing；它是一種 topology 與 resource management architecture。

## 5. RAIBA 與 Continuous Converter Architecture 的差異

歷史資料曾將 RAIBA 與 MVAC 等 converter-dominant concept 做比較。

可用以下抽象方式理解：

- **RAIBA：**相對低頻的 topology reconfiguration；
- **converter-dominant architecture：**持續性的高頻 power conversion / waveform synthesis。

這個差異對 RAIBA 2.0 很重要，因為 hybrid architecture 可以把兩者結合，而不是只能二選一。

## 6. 歷史上與 RAIBA 關聯的 Battery Intelligence

先前 RAIBA 相關資料包含下列 battery intelligence 主題：

- battery lifetime / remaining-life prediction；
- voltage、current、temperature、time 與 DCIR 相關 feature；
- CNN / LSTM 探索；
- 利用 charge-cycle behavior 進行 micro internal-short detection；
- battery condition / anomaly evaluation。

這些資料支持一個重要理解：RAIBA 從來不只是 switching hardware，而是下列完整閉環：

```text
Sensing -> Estimation / Intelligence -> Decision -> Electrical Actuation
```

## 7. 需要重新確認的歷史 Demo / Acceptance Claims

下列內容出現在 prior material 中，但在對外使用前必須視為 **historical claims pending original-document re-verification**：

- 3S2P hardware/demo 背景；
- 5S5P simulation 背景；
- 約 75% module-capacity disparity 下，約 98% array-capacity utilization 的驗收方向。

在重新取得原始 test condition 與定義之前，不應把這些數字用於 customer claim、paper、patent 或 formal presentation。

## 8. 目前仍未知的內容

目前的重建**無法確認**：

- 原始 source code；
- 精確 scheduler / state machine；
- 原始 optimization objective function；
- 完整 EBM/SPM schematics；
- 原始 switching device 選型；
- 精確 CAN / firmware protocol；
- switching waveform 細節；
- 歷史 BOM / efficiency；
- 75% / 98% acceptance claim 背後的精確定義。

## 9. Engineering Interpretation（非歷史事實）

對原始 RAIBA 概念最有力的現代詮釋是：

> Battery system 不應只管理 SOC 與 protection limit，還應主動管理**哪些 electrochemical assets 處於 electrically active、各自承擔多少 duty，以及何時應將有限的 power-electronic resources 配置給它們**。

這個 interpretation 構成 RAIBA 2.0 的基礎。
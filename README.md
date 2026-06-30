# 📦 Automatic Conveyor Box Sorting System

A PLC ladder logic simulation of an automatic box sorting system built using **CodeSys**. The system runs a conveyor to detect boxes, uses a pneumatic cylinder to push detected boxes off the line, counts processed boxes, and includes full Emergency Stop fault handling.

---

## 📹 Demo Video

🎥 [Watch Simulation Video on YouTube](#) ← *Replace this with your YouTube link*

---

## 📌 Project Overview

| Field | Details |
|---|---|
| **Platform** | CodeSys (Simulation) |
| **Language** | Ladder Diagram (LD) |
| **Function** | Detects boxes on conveyor and sorts them using a pneumatic cylinder |
| **Counting** | Tracks sorted boxes using CTU counter |
| **Safety** | Emergency Stop with Fault Lamp and latched fault handling |

---

## 🔌 I/O Table

### Inputs

| Variable | Type | Description |
|---|---|---|
| `START` | BOOL | Start button — initiates the conveyor |
| `STOP` | BOOL | Stop button — halts the system |
| `E_STOP` | BOOL | Emergency stop — triggers fault condition |
| `BOX_SENSOR` | BOOL | Detects a box on the conveyor |
| `F_LIMIT` | BOOL | Forward limit switch — stops cylinder extension |
| `B_LIMIT` | BOOL | Backward limit switch — stops cylinder retraction |
| `RESET_FAULT` | BOOL | Resets the fault lamp after E_STOP condition |
| `RESET_CTR` | BOOL | Resets the box counter |

### Outputs

| Variable | Type | Description |
|---|---|---|
| `CONVEYOR` | BOOL | Conveyor motor |
| `FAULT_LAMP` | BOOL | Indicates fault condition (set by E_STOP) |
| `Cylinder_Extend` | BOOL | Extends the pneumatic cylinder to push the box |
| `Cylinder_Retract` | BOOL | Retracts the cylinder back to home position |
| `STOP_REQUEST` | BOOL | Internal flag — requests conveyor stop when box detected |
| `COUNT_REQUEST` | BOOL | Internal flag — triggers box counter |

### Timers & Counters

| Block | Type | Setting | Description |
|---|---|---|---|
| `CTU_0` | CTU | `PRESENT_VALUE := 10` | Counts sorted boxes, preset to 10 |
| `CURRENT_VALUE` | WORD | — | Current box count |
| `R_TRIG_0` | R_TRIG | — | Rising edge trigger — fires once when cylinder reaches back limit |

---

## 🔄 System Sequence

1. START pressed → CONVEYOR turns ON
2. BOX_SENSOR detects a box → STOP_REQUEST set → conveyor stops, cylinder begins extending
3. Cylinder extends until F_LIMIT is reached → Cylinder_Extend resets, Cylinder_Retract sets
4. Cylinder retracts until B_LIMIT is reached → Cylinder_Retract resets
5. On reaching B_LIMIT, rising edge trigger fires → COUNT_REQUEST set
6. CTU_0 increments the box count
7. Cycle repeats for the next box

### Emergency Stop Sequence
1. E_STOP pressed → FAULT_LAMP set
2. Cylinder_Extend and Cylinder_Retract both reset — cylinder halts immediately
3. Conveyor stops (FAULT_LAMP blocks conveyor rung)
4. RESET_FAULT pressed → FAULT_LAMP resets → system ready to restart

---

## 🧩 Ladder Logic — Rung Breakdown

| Rung | Description |
|---|---|
| 1 | START with seal-in (CONVEYOR + COUNT_REQUEST) energizes CONVEYOR. STOP_REQUEST, STOP, E_STOP and FAULT_LAMP all break the circuit |
| 2 | E_STOP sets FAULT_LAMP and resets both Cylinder_Extend and Cylinder_Retract — immediate safe stop |
| 3 | BOX_SENSOR sets STOP_REQUEST and sets Cylinder_Extend — begins push sequence |
| 4 | F_LIMIT resets Cylinder_Extend and sets Cylinder_Retract — cylinder reverses direction |
| 5 | B_LIMIT resets Cylinder_Retract. Rising edge on B_LIMIT triggers COUNT_REQUEST |
| 6 | COUNT_REQUEST increments CTU_0. RESET_CTR clears the counter |
| 7 | RESET_FAULT resets FAULT_LAMP, clearing the fault condition |

---

## 🖥️ Visualization

A simple HMI was built in CodeSys with manual test buttons for START, STOP, E_STOP, FAULT RESET, CTR RESET, and simulated sensor inputs (F_LIMIT, B_SENSOR, B_LIMIT) along with three status indicator lamps.

---

## 📁 Repository Structure

```
conveyor-box-sorting-plc/
├── media/
│   ├── ladder_logic.pdf
│   ├── plc_program_screenshot.png
│   └── visualization_screenshot.png
├── README.md
```

---

## 🛠️ Tools Used

- **CodeSys 3.5** — PLC programming and simulation
- **Ladder Diagram (LD)** — Programming language
- **Soft PLC simulation** — No physical hardware required

---

## 👤 Author

**S Navaneeth Krishna**
B.Tech — Electrical and Electronics Engineering
Muthoot Institute of Science & Technology, Kerala

[![GitHub](https://img.shields.io/badge/GitHub-navaneeth--linh-181717?style=flat&logo=github)](https://github.com/navaneeth-linh)

---

## 📜 License

This project is open source and available for educational use.

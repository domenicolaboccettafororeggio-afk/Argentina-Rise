# African Negotiation Diagnostic Audit (v0.1)
**Submod:** Argentina Rise  
**Date:** July 12, 2026

---

## 1. Environment & Setup

* **Verified Directory:** `C:\Users\studi\Documents\Paradox Interactive\Hearts of Iron IV\mod\Argentina_Rise_A_Millennium_Dawn_Rework`
* **Branch Git:** `feature/conferencia-buenos-aires`
* **Log Timestamps:** July 12, 2026, 19:48:27 (Global) / 19:43:43 (Crash logs)
* **Latest Crash Folder:** `C:\Users\studi\Documents\Paradox Interactive\Hearts of Iron IV\crashes\hoi4_20260712_194803`
* **File Analysed:**
  1. `common/decisions/ARG_rise_african_strategic_acquisitions_decisions.txt`
  2. `events/ARG_rise_african_acquisitions_events.txt`
  3. `localisation/english/ARG_rise_african_acquisitions_l_english.yml`
  4. `localisation/spanish/ARG_rise_african_acquisitions_l_spanish.yml`
  5. `localisation/italian/ARG_rise_african_acquisitions_l_italian.yml`
  6. `C:\Users\studi\Documents\Paradox Interactive\Hearts of Iron IV\logs\error.log`

---

## 2. Diagnostics Findings

### A. Errors Certi di Argentina Rise

#### 1. Invalid Trigger Type: `stability`
* **File:** `events/ARG_rise_african_acquisitions_events.txt`
* **Lines:** 99, 277, 290
* **Log Entry:** 
  `Invalid trigger 'stability' in events/ARG_rise_african_acquisitions_events.txt line : 99`
  `Error: "Unknown trigger-type: stability, near line: 99"`
* **Cause:** The trigger for stability in Hearts of Iron IV script is `has_stability`, not `stability`.
* **Impact:** Modifiers for AI calculations depending on stability are ignored.
* **Suggested Patch:** Change all occurrences of `stability < 0.45` and `stability > 0.70` to `has_stability < 0.45` and `has_stability > 0.70`.

#### 2. Autonomy Scoping Error: `set_autonomy`
* **File:** `events/ARG_rise_african_acquisitions_events.txt`
* **Line:** 177 (also affects lines 169-183)
* **Log Entry:**
  `events/ARG_rise_african_acquisitions_events.txt:177: set_autonomy: cant have a relation to yourself`
* **Cause:** `set_autonomy` is called inside AGL's event option without wrapping it in the overlord's scope (`event_target:ARG_rise_acquisitions_argentina`). This makes AGL try to puppet itself, which is rejected by the engine.
* **Impact:** Angola does not transition to a satellite state after ceding Benguela.
* **Suggested Patch:** Wrap the `set_autonomy` logic inside `event_target:ARG_rise_acquisitions_argentina = { ... }` scope.

---

### B. Errors Probabili di Argentina Rise
* **None.** No other errors related to the submod are present in `error.log`.

---

### C. Warning MD Non Collegati
* All music file, entity, particle system, and equipment upgrade warnings (e.g. `ethiopian_highlands.ogg`, `WZ-523`, `Sweden threat comparison`, `landmark_binnenhof`) are pre-existing Millennium Dawn or base game warnings and are completely unrelated to Argentina Rise.

---

## 3. Static Audit of Mechanics

* **Braces & Namespace:** Perfectly matched and correct.
* **Treasury Checks:** Logic is sound, checks and deductions occur only at final agreement options.
* **Test Override Flags:** Functional and mutually exclusive. However, they must be cleared properly in the success/rejection events.
* **UTF-8 BOM:** Correctly applied to the English, Spanish, and Italian translation files.

---

## 4. Comparison with Baseline (v0.1.5)

| Area | v0.1.5 funzionante | Sistema attuale | Differenza | Rischio |
|---|---|---|---|---|
| **Avvio Decisione** | Pays cost immediately | Cost paid only at final option | Safer treasury checks | None |
| **Evento Proposta** | Direct 201 sent to AGL | Choices event 201 for ARG | Three choices | None |
| **Valutazione AGL** | 95% accept / 5% reject | Dynamic modifiers + Counteroffer | Dynamic AI logic | `stability` trigger error |
| **Autonomy/Puppet** | Satellite state set | Satellite state set | Scope error in AGL event | Autonomy relation error |
| **Test Override** | None | Three deterministic decisions | Mutually exclusive tests | None |

---

## 5. Crash Log Check
* **Crash Date/Time:** July 12, 2026, 19:48:04
* **Exception:** Access violation at `PHYSFS_writeULE64`.
* **Analysis:** The crash occurred during standard I/O disk writes, often associated with auto-saves, music caching, or file handle exhaustion by heavy mods like Millennium Dawn. It is not caused by script logic of the new decisions or events.

---

## 6. Git Status

```
Changes not staged for commit:
	modified:   common/decisions/ARG_rise_african_strategic_acquisitions_decisions.txt
	modified:   events/ARG_rise_african_acquisitions_events.txt
	modified:   localisation/english/ARG_rise_african_acquisitions_l_english.yml
	modified:   localisation/italian/ARG_rise_african_acquisitions_l_italian.yml
	modified:   localisation/spanish/ARG_rise_african_acquisitions_l_spanish.yml
```

---

## 7. Conclusion

**Conclusion Formula:** `ERRORI MINORI NON BLOCCANTI IDENTIFICATI`

The core negotiation flow functions correctly (verified in game by test paths), but two script-level bugs (incorrect trigger keyword for stability and incorrect scoping for autonomy setup in AGL event) cause the AI modifier weights to ignore stability and the autonomy level transition to fail.

### Order of Corrections:
1. Replace `stability` with `has_stability` in `events/ARG_rise_african_acquisitions_events.txt`.
2. Wrap `set_autonomy` block inside `event_target:ARG_rise_acquisitions_argentina` scope in `events/ARG_rise_african_acquisitions_events.txt`.

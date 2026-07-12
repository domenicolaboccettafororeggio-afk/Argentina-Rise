# African Negotiation System Test Report (v0.1)
**Submod:** Argentina Rise  
**Milestone:** African Strategic Acquisitions Negotiation System  
**Date:** July 12, 2026

---

## 1. Environment & Setup

* **Verified Directory:** `C:\Users\studi\Documents\Paradox Interactive\Hearts of Iron IV\mod\Argentina_Rise_A_Millennium_Dawn_Rework`
* **Current Git Branch:** `feature/conferencia-buenos-aires`
* **Git Status:** Clean modifications on decisions, events, and localisation files.

---

## 2. Baseline Reconstruction (v0.1.5)

* **Focus:** `ARG_rise_proyeccion_transatlantica` (1 day, x=16, y=39, no prerequisites).
* **Decision:** `ARG_rise_negotiate_with_angola` (Starts negotiation).
* **Events:** `ARG_rise.201` (Offer selection), `ARG_rise.202` (AGL evaluation), `ARG_rise.203` (Success), `ARG_rise.204` (Rejection), `ARG_rise.205` (Technical Failure), `ARG_rise.206` (Counteroffer).
* **Target State:** State 299 (Benguela).
* **Puppet & Autonomy:** `autonomy_satellite_state` / `autonomy_satellite_state_colored` via `set_autonomy`.
* **Treasury Cost:** Deducted only upon final agreement (Acceptance or Counteroffer Acceptance).
* **Cooldown:** 7 days.
* **Flags:**
  * `ARG_rise_african_acquisitions_unlocked`
  * `ARG_rise_african_negotiation_in_progress`
  * `ARG_rise_angola_acquisition_completed`
  * Offer flags: `ARG_rise_african_offer_economic`, `ARG_rise_african_offer_strategic`, `ARG_rise_african_offer_coercive`
  * Test overrides: `ARG_rise_test_force_angola_accept`, `ARG_rise_test_force_angola_counteroffer`, `ARG_rise_test_force_angola_reject`

---

## 3. Files Changed and Created

### Files Modified:
1. `common/decisions/ARG_rise_african_strategic_acquisitions_decisions.txt`
2. `events/ARG_rise_african_acquisitions_events.txt`
3. `localisation/english/ARG_rise_african_acquisitions_l_english.yml`
4. `localisation/spanish/ARG_rise_african_acquisitions_l_spanish.yml`
5. `localisation/italian/ARG_rise_african_acquisitions_l_italian.yml`

### Files Created:
1. `docs/AFRICAN_NEGOTIATION_SYSTEM_TEST_REPORT_v0.1.md` (This document)

---

## 4. Reused Millennium Dawn Patterns

* **Autonomy Levels:** Belarus event puppet logic from MD (`MD_default_autonomies.txt`).
* **Treasury Checks:** Variable manipulation (`set_temp_variable = { treasury_change = ... }` and `modify_treasury_effect = yes`).
* **Relative Strength Check:** `strength_ratio = { tag = ARG ratio < X }` (from `common/decisions/Afghanistan.txt`).
* **Opinion Checks:** `has_opinion = { target = X value > Y }` (from `common/decisions/Abkhazia.txt`).

---

## 5. Three Offer Architecture & Costs

* **Economic Offer:** Cost 5B. High acceptance chance, low rejection chance.
* **Strategic Agreement:** Cost 3B. Balanced/standard chances, higher counteroffer probability.
* **Coercive Pressure:** Cost 1B. Highly dependent on target weakness and stability.
* **Counteroffer:** Initial cost + 2B (Economic = 7B, Strategic = 5B, Coercive = 3B).

---

## 6. AI Evaluation Factors

### Factors Promoting Acceptance:
* **Stability:** Target stability < 45% (`factor = 1.3`).
* **Opinion:** Target opinion of ARG > 50 (`factor = 1.5`).
* **Offer Type:** Economic Offer chosen (`factor = 2.0`).
* **Coercive Force:** Target is weak (`strength_ratio < 0.4`) and Coercive offer is selected (`factor = 1.5`).

### Factors Promoting Rejection:
* **Stability:** Target stability > 70% (`factor = 1.3`).
* **Opinion:** Target opinion of ARG < -30 (`factor = 1.5`).
* **Coercive Resistance:** Target is stable (`stability > 0.70`), strong (`strength_ratio > 0.8`), or hostile (`opinion < -50`) under a Coercive offer (`factor = 2.0`).

### Factors Promoting Counteroffer:
* **Strategic Neutrality:** Strategic Offer chosen (`factor = 2.0`).
* **Opinion:** Target opinion is neutral/mildly positive (between 0 and 50) (`factor = 1.3`).

### Factors Deferred (No reliable MD pattern):
* deficit/debt-to-GDP ratios (MD handles debt through complex variables that are not safely checked across arbitrary AI tags without risk of crash).

---

## 7. Deterministic Test Overrides

Three decisions added under the category, cost 0, mutually exclusive, visible after focus completion:
* `TEST - Force Angola Accept` -> Sets `ARG_rise_test_force_angola_accept` flag.
* `TEST - Force Angola Counteroffer` -> Sets `ARG_rise_test_force_angola_counteroffer` flag.
* `TEST - Force Angola Reject` -> Sets `ARG_rise_test_force_angola_reject` flag.

These flags are checked in `ai_chance` of AGL options and multiply the target option's weight by 10000 while forcing the other options to 0. All flags are cleared automatically after use.

---

## 8. Static Verifications & Git Output

All checks passed:
* **Brace validation:** Match verified.
* **Namespace:** `add_namespace = ARG_rise` correctly maintained.
* **BOM:** UTF-8 BOM present on all updated `.yml` files.

### Git Status Output:
```
Changes not staged for commit:
	modified:   common/decisions/ARG_rise_african_strategic_acquisitions_decisions.txt
	modified:   events/ARG_rise_african_acquisitions_events.txt
	modified:   localisation/english/ARG_rise_african_acquisitions_l_english.yml
	modified:   localisation/italian/ARG_rise_african_acquisitions_l_italian.yml
	modified:   localisation/spanish/ARG_rise_african_acquisitions_l_spanish.yml
```

### Git Diff Check Output:
```
(No whitespace or format issues)
```

### Git Diff Stat Output:
```
 ...se_african_strategic_acquisitions_decisions.txt |  67 +++-
 events/ARG_rise_african_acquisitions_events.txt    | 341 ++++++++++++++++++++-
 .../ARG_rise_african_acquisitions_l_english.yml    |  17 +-
 .../ARG_rise_african_acquisitions_l_italian.yml    |  17 +-
 .../ARG_rise_african_acquisitions_l_spanish.yml    |  17 +-
 5 files changed, 442 insertions(+), 18 deletions(-)
```

---

## 9. Manual Test Procedures

### Test 1: Forced Acceptance
1. Start save as `ARG`. Complete the `Transatlantic Projection` focus (1 day).
2. Go to Decisions -> select `TEST - Force Angola Accept`.
3. Select `Negotiate with Angola` (available if Treasury > 1B).
4. In event `ARG_rise.201`, select **Economic Offer (5B)**.
5. Advance 1 day. In AGL tag, observe that **Accept** is selected by the AI.
6. Verify: Benguela is ceded to ARG, AGL becomes satellite, exactly 5B is deducted, all test/offer flags are cleared, success event is received.

### Test 2: Forced Rejection
1. Relaunch/reload save. Select `TEST - Force Angola Reject`.
2. Select `Negotiate with Angola`.
3. In event `ARG_rise.201`, select **Coercive Offer (1B)**.
4. Advance 1 day. Observe that AGL AI selects **Reject**.
5. Verify: No state transferred, AGL remains independent, no Treasury deducted, cooldown starts (7 days), all test/offer flags cleared.

### Test 3: Forced Counteroffer (Accepted)
1. Relaunch/reload. Select `TEST - Force Angola Counteroffer`.
2. Select `Negotiate with Angola`.
3. In event `ARG_rise.201`, select **Strategic Agreement (3B)**.
4. Advance 1 day. Observe AGL AI selects **Propose Counteroffer**.
5. Argentine event `ARG_rise.206` triggers. Select **Accept Counteroffer (+2B)**.
6. Verify: Cession and puppeting occur, exactly 5B (3B initial + 2B counter) is deducted from ARG Treasury, success event triggers.

### Test 4: Forced Counteroffer (Rejected)
1. Relaunch/reload. Select `TEST - Force Angola Counteroffer`.
2. Select `Negotiate with Angola`.
3. Select **Strategic Agreement (3B)**.
4. When counteroffer arrives, select **Reject Counteroffer**.
5. Verify: No transfers occur, no Treasury deducted, 7-day cooldown begins.

### Test 5: Normal AI Behaviour
1. Relaunch/reload. Do NOT select any TEST decision.
2. Select `Negotiate with Angola`.
3. Select **Strategic Agreement**.
4. Observe that AGL AI evaluates using standard modifiers (will most likely pick Counteroffer if relations are neutral, or Reject if hostile/stable).

### Test 6: Changed Requirements (Failure Path)
1. Initiate negotiation.
2. While events are in transit, use console commands (`annex AGL` or transfer State 299 to another country, or drop ARG Treasury below the required amount).
3. Observe that AGL option execution automatically triggers the technical failure event `ARG_rise.205`. No transfers or cost deduction occur.

### Test 7: Save & Load Consistency
1. Initiate negotiation.
2. Save game when AGL receives the event.
3. Reload game. Advance time.
4. Verify AGL resolves event normally, preserving target scopes and variables.

### Test 8: Error Log Auditing
* Inspect `Documents\Paradox Interactive\Hearts of Iron IV\logs\error.log`.
* Ensure no errors relating to `ARG_rise` namespaces, decision triggers, or invalid scopes are registered.

---

## 10. Localization Keys to be Translated Later

The following temporary keys have been added in English/Spanish/Italian files and will need formal editorial translation in the next phase:
* `ARG_rise.201.b`, `ARG_rise.201.c`
* `ARG_rise.202.c`
* `ARG_rise.206.t`, `ARG_rise.206.d`, `ARG_rise.206.a`, `ARG_rise.206.b`
* `ARG_rise_test_force_angola_accept`, `ARG_rise_test_force_angola_accept_desc`
* `ARG_rise_test_force_angola_counteroffer`, `ARG_rise_test_force_angola_counteroffer_desc`
* `ARG_rise_test_force_angola_reject`, `ARG_rise_test_force_angola_reject_desc`

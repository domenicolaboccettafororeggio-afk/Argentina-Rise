# Argentina Rise - Post-Mexico Strategic Autopsy v0.1.4

## Status and method

**Scope:** read-only strategic analysis of the post-Mexico campaign. No gameplay, save, MD or vanilla file was modified while preparing this report.

**Workspace analysed:**

- Submod: `C:\Users\studi\Documents\Paradox Interactive\Hearts of Iron IV\mod\Argentina_Rise_A_Millennium_Dawn_Rework`
- Millennium Dawn, read-only: `E:\Steam\steamapps\workshop\content\394360\2777392649`
- Vanilla, read-only: `E:\Steam\steamapps\common\Hearts of Iron IV`
- Saves, read-only: `C:\Users\studi\Documents\Paradox Interactive\Hearts of Iron IV\save games`
- Logs, read-only: `C:\Users\studi\Documents\Paradox Interactive\Hearts of Iron IV\logs`

The conclusion is deliberately based on two classes of evidence:

1. **Observed:** the current game log, the binary save headers, the implemented Argentina Rise Mexico/energy code, and MD scripts.
2. **Unknown until playtest:** exact save-state values that cannot be safely decoded from HOI4bin files. They are not estimated as facts.

The key design conclusion is that Mexico has changed the campaign from continental consolidation into a global deterrence problem. The next act should not repeat the Mexican campaign with a larger map.

## 1. Saves analysed

| Save | Modified | Size | Format | What can be established safely |
|---|---:|---:|---|---|
| `...\ARG 7.hoi4` | 2026-07-11 16:19:17 | 127,314,921 B | `HOI4bin`, uncompressed binary container | Header identifies player tag `ARG`; latest named manual save. |
| `...\autosave.hoi4` | 2026-07-11 16:18:09 | 127,637,079 B | `HOI4bin`, uncompressed binary container | Header identifies `ARG`; latest autosave. |
| `...\ARG ASSALTO MEX.hoi4` | 2026-07-11 15:47:37 | 122,897,932 B | `HOI4bin`, uncompressed binary container | Mexico campaign-era manual save. |
| `...\ARG RISE ASSALTO AL MEX.hoi4` | 2026-07-11 02:16:29 | 122,594,660 B | `HOI4bin`, uncompressed binary container | Earlier Mexico assault save; current logs record one `Invalid subunit.: none` warning while loading it. |
| `...\ARG RISE 6.hoi4` | 2026-07-07 00:48:35 | 121,843,486 B | `HOI4bin`, uncompressed binary container | Earlier campaign checkpoint. |
| `...\ARG RISE 5.hoi4` and earlier named ARG RISE saves | 2026-06-09 to 2026-07-05 | 96.5-122.1 MB | `HOI4bin`, uncompressed binary containers | Historical checkpoints, not decoded. |

### Save limitations

These saves begin with `HOI4bin`, are not ZIP files, and no trusted binary save decoder is present in the environment. No improvised parsing was used. Consequently, this report cannot safely extract exact division counts, manpower, stockpiles, owned-state lists, faction membership, diplomatic relations, factories, energy totals, naval order of battle, or current war score.

The current log proves that the campaign continued to at least **4 August 2025**, after loading the Mexico campaign save. It also proves the player country is Argentina. Exact date/internal values for `ARG 7` and `autosave` must be read in-game.

## 2. Observed campaign chronology

The log establishes this sequence:

| Date | Observed milestone |
|---|---|
| 3 Nov 2024 | `ARG_rise.91.a`: the primordial nuclear era event was accepted. |
| 4 Dec 2024 | `ARG_rise.92.a`: the renewable era event was accepted. |
| 29 Jan 2025 | `ARG_rise_renewable_energy_drive_5` was added. |
| 3 Mar 2025 | Argentina used military-industry expansion, nuclear construction, and `ARG_rise_send_ultimatum_to_mexico`. Mexico refused; `ARG_rise.86` granted annex-everything war goal. |
| 15 Mar 2025 | Argentina-Mexico war news fired. Russia, China and the USA received the international news event; this is observation, not intervention. |
| 27 Apr and 23 Jun 2025 | Argentina again expanded military industry. |
| 2 Jul 2025 | `ARG_rise.88`: Mexico City fell. |
| 4 Aug 2025 | Game log still advances normally. No crash report was generated in the successful test session. |

The user-reported outcome is that Mexico was subsequently defeated and integrated. The code supports this outcome: `ARG_rise_una_nacion_dos_pueblos` adds Argentine cores to states 828-840 and removes Mexican claims.

## 3. Strategic photograph of Argentina

### Territory and borders

**Observed/implemented:**

- The Mexico branch claims 828-840 in `common/national_focus/ARG_rise_focus_tree.txt`, then cores those same states after integration.
- The Mexico branch therefore creates a direct strategic approach to the United States if all relevant northern Mexican states are owned and controlled, as described by the campaign context.
- The wider submod already contains integration patterns for Uruguay, Paraguay, Bolivia, Chile, Brazil-related regions, Peru and the Caribbean. The Conferencia branch also contains an American Confederation charter and spirit.
- Cuba/Caribbean integration and the conference route create an Atlantic-Caribbean staging area in addition to the Mexican land axis.

**Not safely readable from HOI4bin:** exact owned states, current occupation, cores actually selected, remaining independent American states, and any African footholds. In-game verification should use the diplomacy map, occupation screen, cores map mode, and faction map.

### Diplomacy

**Observed:** Russia, China, the USA and other countries received the Mexico-war and Mexico-City news. The USA did not visibly enter the Mexico war in the log. That is evidence that the Mexico conflict remained below an automatic American intervention threshold in this campaign.

**Not established:** Argentina's current faction, membership of the Conferencia, guarantees, non-aggression pacts, access rights, opinion values, ideology, BRICS/SCO/CSTO status, or active wars. Those must be checked in the diplomacy screen.

### Military lessons from the campaign

The campaign narrative and log support five real lessons:

1. **Land access matters.** Mexico was won through a costly terrestrial campaign, not a decisive naval shortcut.
2. **Logistics is a victory condition.** The US border is longer, richer and more hostile to a single-overland-axis plan than the Mexican interior.
3. **Air and energy are now strategic, not supporting systems.** Argentina deliberately completed nuclear, renewable and surplus-commercialisation milestones before/during the campaign.
4. **Mass without recovery is brittle.** The reported loss of 36 divisions in a failed landing means future expansion cannot price amphibious operations as disposable probes.
5. **The navy has a specific role.** Coastal bombardment was valuable; it did not replace air superiority, ports, supply and control of the landing zone.

Exact divisions, templates, manpower, aircraft, naval hulls, deficits, missile inventory and nuclear weapons are unavailable without in-game screens. The required screenshots are listed in section 14.

### Economy and energy

**Observed Argentina actions:** nuclear era, renewable era, maximum renewable drive, nuclear construction, hydroelectric dam, repeated military-industry expansion, and energy-surplus commercialisation.

**Implemented capacity:**

- The nuclear program places reactors in Argentine nuclear states 450 and 453; five programme ideas provide cumulative nuclear-generation modifiers.
- The primordial-nuclear event applies the Argentine nuclear spirit after removing the legacy baseline when present.
- The renewable event applies its dedicated spirit, while the drive progression adds renewable capacity.
- Mexico integration adds the following deliberate resource rewards to cores 828-840: oil 120, aluminium 80, steel 150. These are not a measurement of total Mexican resources; they are the extra Argentina Rise integration rewards.

**Not readable from the binary save:** current civilian factories, military factories, dockyards, Treasury, debt, power generation, consumption, fuel, resource balance and production lines. The architecture is clearly stronger than pre-Mexico, but the exact ability to sustain a world war must be measured, not inferred.

## 4. The United States: real risk assessment

### What the log says about this run

The USA was not passive during 2025. It repeatedly executed energy actions, intelligence/cyber actions, military/civilian/GNSS access actions, and its recurring post-9/11 decision. Weekly log values show a Treasury generally in the low hundreds with interest around 5 and debt ratio around 0.73-0.79 during the Mexico campaign. This indicates financial stress, not military weakness.

The log does **not** reveal US division count, fleet, aircraft, faction composition or border deployment. It is therefore wrong to conclude that the USA is weak from Treasury alone.

### Technical MD systems that matter

| System | MD location | Actual pattern | Implication for an ARG attack |
|---|---|---|---|
| NATO membership | `common/scripted_effects/00_startup_effects.txt` lines 6934+; `common/ideas/Factions.txt` | MD populates a global NATO-member array including USA, CAN, ENG, FRA, GER, ITA, TUR and others. | High probability of a coalition response if the underlying faction/call logic is active in this save. |
| NATO events | `events/NATO_system.txt` | `NATO_join` and membership events use the `NATO_member` idea; MD has ongoing NATO event logic. | NATO is not merely a label. It has member state and event hooks. |
| War/on-action logic | `common/on_actions/MD_on_actions.txt` | Includes NATO-related checks and many country/system-specific war hooks. | A declaration can trigger more than vanilla faction calls; exact branch depends on save state. |
| Guarantees/faction calls | vanilla HOI4 diplomacy plus MD scripted effects and national content | MD regularly uses `give_guarantee`, `leave_faction`, `create_faction_from_template`, and country-specific diplomacy. | A direct offensive war can cascade beyond the bilateral target. |
| Nuclear/escalation | MD energy, special-project and country systems | Nuclear capacity is materially linked to energy/special projects, not an Argentina-only finale. | Any US war needs explicit escalation gates and no assumption of a contained conventional conflict. |
| Conditional Peace Deals | `common/scripted_diplomatic_actions/01_peace_deal_diplomatic_actions.txt` in the submod override | The broken CPD action is now globally disabled for compatibility. | Do not use it as a resolution mechanism for the final war. Use established war/peace patterns. |

### Probability assessment

Without decoding faction membership, the exact outcome cannot be certified. The probability bands below are therefore design-risk estimates grounded in the active NATO array and standard HOI4 behaviour:

| Outcome | Assessment | Why |
|---|---|---|
| ARG vs USA only | Low | USA is a NATO member in MD's startup configuration; bilateral isolation is an exceptional save condition. |
| ARG vs American faction | High | Normal faction call behaviour makes this the base expectation. |
| Continental North American war | Very high | The direct Mexican border, US/Canadian geography and American allies make North America the immediate theatre. |
| NATO war | Medium-high | Depends on live faction/call state, but must be designed as likely, not surprising. |
| Multipolar world war | Medium | Becomes high if Russia/China are given automatic offensive obligations, or if European/Asian tensions are already active. |

**Conclusion:** attacking immediately should be a player-enabled high-risk option, not the canonical next step. It should never be presented as a simple continuation of Mexico.

## 5. Russia, China, and multipolar patterns

### What is established in MD

| Pattern | Files | Behaviour | Reuse value |
|---|---|---|---|
| CSTO membership | `common/scripted_effects/02_CSTO_effects.txt`; `common/ideas/Factions.txt`; `events/CSTO_system.txt` | `CSTO_join` adds `CSTO_member`, stores a global array entry and adds CSTO technology sharing. `CSTO_leave` reverses these and leaves the faction. | Strong pattern for staged membership/technology cooperation, but not a generic ARG-RUS-CHI alliance. |
| SCO membership variants | `common/ideas/Factions.txt`; startup arrays in `00_startup_effects.txt` | MD has economic, political and military SCO member ideas, not just one diplomatic label. | Good model for staged cooperation before formal defence. |
| BRICS identity | `common/ideas/Factions.txt` and country-specific content | BRICS is represented as an institutional/economic idea pattern. | Good model for a conference, investment and recognition phase; not proof of automatic war intervention. |
| Faction replacement | `common/scripted_effects/99_SOV_scripted_effects.txt` | Existing SOV paths use `leave_faction` followed by `create_faction_from_template`. | Direct technical model for a later bloc, but must be conditional on live faction status. |
| Guarantees | SOV scripted effects and country paths | MD uses `give_guarantee` for specific targets. | Safer first military commitment than immediate universal call-to-war. |

### Current strategic reading

The log shows both Russia and China as globally active, with Russia operating on a constrained Treasury and China holding far greater Treasury. Both received the Mexico war news, but neither joined. This is a valuable narrative fact: they noticed Argentine power, but have not paid its cost.

Russia and China must not become free expeditionary manpower for an Argentine offensive. The correct fantasy is mutual leverage:

- **Argentina offers:** continental access, Atlantic/Caribbean presence, food/resources, a non-Western regional order, industrial-energy ambition and a new political centre.
- **Russia wants:** strategic distraction against NATO, arms/technology markets, political recognition, and a partner not subordinate to Washington.
- **China wants:** secure trade, investment protection, resource access, ports/logistics, and a predictable partner rather than automatic escalation.

### Feasibility rules

1. Russia and China can be put into a new faction only through a deliberate branch that first checks `is_in_faction`, faction-leader status, active wars, ideology and their current institutional ideas.
2. A guarantee/mutual-defence relationship is technically lower-risk than automatic offensive entry into an ARG-US war.
3. No existing MD pattern proves that Russia and China will reliably join an Argentine offensive against the USA. This must be a conditional choice, with refusal and neutrality outcomes.
4. BRICS/SCO/CSTO should be used as diplomatic and economic preconditions, not treated as interchangeable military alliances.

## 6. Africa: value and danger

### West Africa

**Strategic value:** Atlantic-facing ports, routes toward Europe/Caribbean, oil and minerals in selected states, and political distance from the immediate US front.

**Risk:** Francophone networks, UK/French interests, unstable minor-state wars and high administrative surface. It can become map-painting with no direct answer to US air/naval superiority.

**Design use:** a limited Atlantic logistics compact or one resource/port objective, not a continental conquest chain.

### Central Africa

**Strategic value:** minerals, depth, and the possibility of an industrial-resource narrative.

**Risk:** the worst supply terrain and lowest payoff per division committed. It repeats the Mexican lesson without the Mexican industrial/strategic payoff.

**Design use:** not recommended as the principal next act. At most, a diplomatic/resource agreement or a narrowly scoped protectorate pattern.

### Southern Africa

**Strategic value:** better ports, industrial base relative to the region, minerals, sea-lane leverage and a clearer power-political opponent structure.

**Risk:** long naval logistics, possible Western ties, and a campaign that may pull the player away from the newly acquired US border at exactly the wrong moment.

**Design use:** best African candidate for a limited optional theatre, provided it is framed as an Atlantic/Indian Ocean strategic foothold rather than territorial accumulation.

### Africa verdict

Africa provides real resources and basing only if the branch is narrow. A full African conquest before the USA would dilute the Argentine narrative, consume the army in poor logistics, and turn the post-Mexico act into more of the same. It should be an **optional, capped preparation theatre** within a broader multipolar strategy.

## 7. Four strategy comparison

Scores use 1 (poor) to 5 (strong) and evaluate the current campaign, not an abstract alternate game.

| Strategy | Fun | Distinct from existing content | Power fantasy | Narrative coherence | Technical fit | Stability risk | Duration/repetition | Verdict |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| A. Immediate USA war | 3 | 2 | 5 | 3 | 3 | 2 | 2 | A legitimate dangerous option, but not the main authored route. It risks turning Mexico into a tutorial for a larger slog. |
| B. ARG-RUS-CHI pact, then USA | 5 | 5 | 5 | 5 | 4 | 3 | 4 | Strong principal route if alliances are conditional and staged. |
| C. Africa first, then USA | 3 | 3 | 3 | 2 | 3 | 3 | 2 | Too likely to become a long sequence of conquests detached from Argentina's central drama. |
| D. Pact + limited Africa + final USA decision | 5 | 5 | 5 | 5 | 4 | 3 | 5 | Recommended. It adds a new diplomatic game, preserves optional military agency, and makes the final conflict earned. |

### Mandatory project criteria

| Criterion | A | B | C | D |
|---|---|---|---|---|
| Is it fun to play? | High tension, but punishing | High | Uneven | Highest variety |
| Is it different from existing branches? | Not enough | Yes | Partly | Yes |
| Does it offer a new power fantasy? | Conqueror | Diplomatic architect | Overseas imperial player | Multipolar architect and continental shield |
| Is it worth implementation effort? | Only as optional climax | Yes | Not as a main route | Yes, if scope is capped |

## 8. Recommended world consequences

The recommended route is **D: multipolar pact, limited African option, then a deliberate final decision on the USA.** Consequences should be distributed across existing MD patterns rather than a new parallel geopolitical engine.

| Layer | Consequence structure | Existing-pattern basis |
|---|---|---|
| Narrative | Mexico turns Argentina from regional victor into a country watched by every capital; the conference is a summit of interests, not a coronation. | News events, country events, diplomatic-event chains. |
| Diplomatic | USA/NATO condemnation; Russian and Chinese negotiation with refusal branches; Argentine partners asked to choose commitments. | NATO/CSTO/SCO/BRICS ideas, guarantees, faction leave/create patterns. |
| Economic | Trade access, energy and investment agreements precede defence terms; sanctions/market friction are consequences of escalation. | MD Treasury, investment, trade/resource and energy systems. |
| Military | Defensive guarantees first; formal calls only after a defined US escalation or a player choice to turn the pact offensive. | Vanilla guarantees/faction calls plus MD faction templates. |
| Global | International news at the summit, treaty signature, US ultimatum, and point of no return. | Existing news-event pattern already used by Mexico branch. |
| Internal | Conferencia members may dissent, stay neutral or receive compensating roles; no automatic unanimous empire. | Existing conference decision/flag architecture and country-specific conditions. |

The key rule is that **the alliance changes the diplomatic geometry before it changes the division count**.

## 9. Proposed architecture of the next act

This is an architecture, not a focus implementation.

### Entry conditions

- Mexico integration complete or Mexico no longer a strategic threat.
- Conferencia progression retained as a prerequisite/context, not duplicated.
- Minimum readiness gates based on existing MD-visible systems: energy surplus, Treasury/debt health, logistics preparation, and a player-visible military readiness threshold.
- Live checks for USA existence, Russia/China existence, faction status and active major wars.

### Macro-phases

1. **The Northern Frontier** (2-3 focuses): recognise that Mexico changed the geopolitical horizon; assess the border and domestic war exhaustion. No war declaration.
2. **The Third Pillar** (3-4 focuses): separate economic-energy approach to China and military-industrial approach to Russia, each with possible refusal/deferral.
3. **The Three Capitals Conference** (2 focuses plus events): negotiate the form of cooperation. Branch between economic alignment, mutual defence, or stalled diplomacy.
4. **Atlantic Depth** (optional 2-3 focuses/decisions): one limited African or Atlantic basing/resource compact. Hard cap: no generic conquest ladder.
5. **The American Reckoning** (2-3 focuses): USA reaction, sanctions/ultimatum, and a point-of-no-return choice between deterrence, defensive containment, or offensive war.
6. **Finals:**
   - defensive multipolar bloc and cold-war containment;
   - negotiated hemispheric settlement if MD/war state permits;
   - high-risk continental war against the USA/faction;
   - failed conference/Argentine strategic autonomy finale.

### Indicative content budget

- 11-15 focuses total.
- 8-12 country/news events, including refusal and escalation outcomes.
- 3-5 decisions, mainly diplomatic preparation and summit choices.
- 2-3 national spirits at most, each representing a real institutional condition rather than generic bonuses.
- 3-4 international news items.

### Non-negotiable design constraints

- Do not reimplement Conferencia de Buenos Aires as another generic regional congress.
- Do not grant Russia and China as unconditional allies.
- Do not force an immediate USA war.
- Do not make Africa a second Mexico.
- Do not use the disabled Conditional Peace Deals system.
- Use MD faction/guarantee/tech-sharing/investment patterns after testing their current save-state prerequisites.

## 10. Technical risks

1. **Save-state uncertainty:** exact factions and relations must be checked before designing conditions around them.
2. **MD version coupling:** NATO/CSTO/SCO/faction systems are broad and sensitive to IDs, on-actions and game rules.
3. **War escalation:** an offensive call-to-war can pull a large coalition in ways that are both narratively correct and technically difficult to test.
4. **Compatibility layer:** CPD is deliberately disabled due to an observed broken GUI flow. New diplomacy must not depend on it.
5. **Existing save warning:** `ARG RISE ASSALTO AL MEX.hoi4` logged `Invalid subunit.: none`; it loaded and ran, but should be retained as a compatibility test case rather than altered casually.
6. **Performance:** the campaign already has global MD AI/energy activity. New daily loops or broad every-country scans would be the wrong implementation style.

## 11. Required follow-up playtest information

Before any implementation, capture these in-game screens from the latest `ARG 7` or autosave:

1. Argentina diplomacy overview: faction, ideology, guarantees, pacts, access and relations with USA/RUS/CHI.
2. Faction map and diplomacy screens for USA, Russia and China.
3. Argentina construction/industry: civilian factories, military factories, dockyards and active production.
4. Army overview: division count, manpower, equipment deficits, recent casualties and primary templates.
5. Air and navy overview: aircraft by mission/type, naval task forces and fuel.
6. Energy GUI: generation, consumption, surplus, nuclear-reactor count, renewable infrastructure and fuel.
7. Treasury/debt/GDP screens for Argentina, USA, Russia and China.
8. World tension, active wars, guarantees and major faction membership.
9. Occupation/core map modes for Mexico and the US border.
10. Any Argentine bases, ports or controlled states outside the Americas.

## 12. Lead Game Designer recommendation

**Do not attack the USA immediately as the authored next act.** Keep it available only as an explicit, visibly dangerous player choice for a campaign that wants the gamble.

**Build the Russia-China pact first, but as a negotiation rather than a gift.** The post-Mexico fantasy should be that Argentina has become important enough to convene powers that normally treat Latin America as peripheral. Russia supplies the security dilemma; China supplies the material and economic dilemma; Argentina supplies the continental political project.

**Do not conquer Africa first.** Add at most one optional, strategically legible African/Atlantic compact whose value is ports, resources or diplomatic depth. It must not become a mandatory conquest ladder.

**Choose the combined route.** It is the only option that simultaneously respects the brutal lesson of Mexico, uses MD systems that already exist, gives the player a new form of agency, and makes the eventual American confrontation feel like a world-historical decision rather than the next war goal in a row.

The final war should be possible, frightening and earned. The more important victory is that, before that decision, the player has made Argentina impossible for the old order to ignore.

## Sources consulted

- `docs/ARGENTINA_RISE_DESIGN_BIBLE_v1.0.md`
- `docs/ARGENTINA_RISE_FORENSIC_ARCHITECTURE_v0.1.3.md`
- `docs/FOCUS_TREE_REVERSE_ENGINEERING_v0.1.3.md`
- `docs/CONQUISTA_DEL_MESSICO_DESIGN_v0.1.3.md`
- `docs/PROJECT_STATUS_v0.1.3.md`
- `common/national_focus/ARG_rise_focus_tree.txt`
- `common/decisions/ARG_rise_mexico_decisions.txt`
- `events/ARG_rise_events.txt`
- `common/on_actions/ARG_rise_on_actions.txt`
- `common/ideas/ARG_rise_ideas.txt`
- MD `common/scripted_effects/00_startup_effects.txt`
- MD `common/scripted_effects/02_CSTO_effects.txt`
- MD `common/scripted_effects/99_SOV_scripted_effects.txt`
- MD `common/ideas/Factions.txt`
- MD `events/NATO_system.txt` and `events/CSTO_system.txt`
- MD `common/on_actions/MD_on_actions.txt`
- HOI4 `logs/game.log` and `logs/error.log`

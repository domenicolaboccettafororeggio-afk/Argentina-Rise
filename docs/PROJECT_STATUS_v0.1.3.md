# Argentina Rise - Project Status v0.1.3

Audit statico del workspace corrente, eseguito senza modificare codice della mod madre Millennium Dawn. Le affermazioni di esecuzione in gioco restano da testare in HOI4.

## 1. Identita della release

- Workspace: `C:\Users\studi\Documents\Paradox Interactive\Hearts of Iron IV\mod\Argentina_Rise_A_Millennium_Dawn_Rework`.
- Branch rilevato da `.git/HEAD`: `feature/conferencia-buenos-aires`.
- HEAD rilevato: `c7d308ef57e2d5f13318957b57b442c215086154`.
- Tag `v0.1.3`, rapporto con remoto e working tree: **NON VERIFICATO**; `git.exe` non e disponibile nell'ambiente.
- Non sono presenti history country/state della submod: i contenuti di setup e gli stati ARG sono ereditati da MD.

## 2. Executive summary

Argentina Rise e una submod ampia, centrata su un focus tree di 353 focus, 97 blocchi evento e 217 idee ARG. I sistemi con presenza concreta sono: crisi iniziale, ricostruzione, Pampas Engine, El Nuevo Triunfo, Proyecto Nacional, energia, nucleare, Conferencia de Buenos Aires, espansione regionale/Cuba, economia e ricerca. Il codice non sostiene l'ipotesi che El Proyecto Nacional sia solo un nodo: ha un ramo discendente, eventi e flag; la biforcazione Milei/comunista-populista invece non e stata trovata e resta pianificata/non implementata. El Nuevo Triunfo e esteso e con reward, ma include 41 reward vuote nell'intero tree e richiede test di raggiungibilita. Il sistema nucleare contiene reward concreti; i cinque focus assegnano 25 reattori a 450/453. Fusion e special project sono dipendenze MD, non tecnologie custom della submod. I rischi principali sono la tracciabilita Git non verificabile, due chiavi di localizzazione duplicate, un backup `.bak` distribuito e asset root duplicati/sospetti.

## 3. Contenuti implementati

- Focus tree: `common/national_focus/ARG_rise_focus_tree.txt` (353 ID, 352 `completion_reward`, 389 prerequisite, 6 mutue esclusioni; circa righe 1-9300).
- Idee e spirit: `common/ideas/ARG_rise_ideas.txt` (217 ID `ARG_rise_*`; inclusi programmi energia/nucleare, spirit politici/economici e strutture di ricerca).
- Eventi: `events/ARG_rise_events.txt` (97 blocchi `country_event`, 82 ID `ARG_rise.*`).
- Decisioni: 11 file in `common/decisions`; 84 ID ARG, con costruzione energia, procurement, bonds, lavoro, risorse e Conferencia.
- On actions: `common/on_actions/ARG_rise_on_actions.txt`, startup, daily, monthly e restore post-guerra civile.

## 4. Contenuti quasi completi

### El Nuevo Triunfo

Il gate e reale: `ARG_rise_el_nuevo_triunfo` richiede `ARG_rise_the_pampas_engine` e il flag `ARG_rise_path_nuevo_triunfo` (`ARG_rise_focus_tree.txt`, circa 3342-3365). L'evento `ARG_rise.4` assegna il flag (`events/ARG_rise_events.txt`, circa 88-99). Il tree contiene sottorami politici, economici, sociali, regionali, militari e ricerca. Le idee `ARG_rise_new_triumph_pressure_*` sono definite e ripristinate dal restore effect. Stato: **IMPLEMENTATO MA DA TESTARE**; non e possibile certificare gameplay, coordinate e tutte le catene solo con analisi statica.

## 5. Contenuti parziali

- 41 `completion_reward = { }` nel focus tree: **PLACEHOLDER/STRUTTURALI**, da valutare uno per uno prima di considerarli contenuto completo.
- Il restore `ARG_rise_restore_completed_focus_ideas_effect` (`common/scripted_effects/ARG_rise_restore_effects.txt:5`) ripristina idee da focus dopo guerra civile, ma non e una verifica di tutte le side effect one-shot.
- GFX: cinque DDS e due file interface GFX presenti; referenze e rendering in gioco: **NON VERIFICATO**.

## 6. Contenuti pianificati ma non implementati

- Non e stata trovata alcuna ID/focus/evento con `Milei`, `libertar*` o biforcazione libertaria esplicita: **PIANIFICATO, NON IMPLEMENTATO**.
- Non e stata trovata una biforcazione comunista/populista esplicitamente denominata per Proyecto Nacional: **PIANIFICATO, NON IMPLEMENTATO**.
- Il design in `MARKDAWN.md` e documentazione, non prova di implementazione; ogni voce assente dal codice e classificata pianificata.

## 7. Focus tree

| Ramo | Evidenza | Stato statico |
|---|---|---|
| Gli Anni della Cenere | focus iniziali e eventi crisi in `ARG_rise_focus_tree.txt` / `ARG_rise_events.txt` | Implementato, da testare |
| The Pampas Engine | gate di entrambi i percorsi politici | Implementato |
| El Nuevo Triunfo | ID circa 3342, flag evento `ARG_rise.4` | Quasi completo / da testare |
| El Proyecto Nacional | ID circa 2209 e discendenti da circa 2236 | Implementato parzialmente, non solo nodo |
| Conferencia Buenos Aires | focus, `ARG_rise_buenos_aires_conference_decisions.txt`, categorie dedicate | Implementato, da testare |
| Espansione/Cuba | focus e DDS dedicati `gfx/interface/goals` | Implementato, da testare |
| Energia/nucleare | focus circa 1613, 4721-4825; decisione energia | Implementato |
| Fusione | dipendenze special-project/facility MD; nessuna `common/technologies` submod | Dipendente da MD / non completamente verificato |

## 8. Eventi

`events/ARG_rise_events.txt` contiene crisi, scelta del percorso (`ARG_rise.4`), sequenze di Proyecto Nacional (`ARG_rise.5` e seguenti), El Nuevo Triunfo, Conferencia e Cuba. Gli eventi sono principalmente `is_triggered_only`; raggiungibilita effettiva va verificata contro focus, decisioni e salvataggio. Nessun ID evento duplicato e stato rilevato nello scan statico degli ID `ARG_rise.*`.

## 9. Decisioni

Categorie/decisioni concrete: bonds, energia, surplus, labor-office, procurement militare, risorse, export materiale reattore e Conferencia. La decisione nucleare `ARG_rise_build_nuclear_power_plant` usa `free_building_slots` e costruisce un `nuclear_reactor` (`ARG_rise_energy_construction_decisions.txt:67-125`). Stato: **FUNZIONANTE STATICAMENTE / DA TESTARE**; costi Treasury e scripted effects MD sono dipendenze esterne.

## 10. Idee e spirit

Le idee sono definite nel solo `ARG_rise_ideas.txt`. I cinque programmi nucleari usano `nuclear_energy_generation_modifier` (circa 378-450) e sono assegnati dai focus, poi ripristinati dal restore effect (circa 895-919). La baseline iniziale `ARG_rise_nuclear_generation_baseline` e assegnata dall'on-startup solo a `original_tag = ARG` (`ARG_rise_on_actions.txt:5-12`). Stacking: programmi 1-5 sommano +22% nucleare e programmi 3-5 +4% a tutta l'energia; e intenzionale nel codice ma richiede validazione di bilanciamento.

## 11. Nucleare e fusione

MD definisce `nuclear_reactor` in `E:\Steam\steamapps\workshop\content\394360\2777392649\common\buildings\00_buildings.txt:158-176`: 2 GW base, `shares_slots = no`, `state_max = 20`. Il calcolo e in `common/scripted_effects/!_energy_effects.txt:219-237` della mod madre. La submod non fa override globale del building.

I focus nucleari correnti aggiungono cinque reattori istantanei ciascuno: 450 per i primi tre e 453 per gli ultimi due (`ARG_rise_focus_tree.txt:1690-1696, 4735-4741, 4762-4768, 4789-4795, 4816-4822`). Con i due iniziali MD: Cordoba 16, Pampas 11, totale 27, sotto cap 20 per stato. La presenza del progetto Fusion dopo dieci tecnologie/facility: **NON VERIFICATO** con test UI; la submod non contiene un file `common/technologies` proprio.

## 12. Localizzazione

Tre file: due YAML attivi e un backup `.bak`; tutti UTF-8 validi nello scan. `ARG_rise_l_english.yml` contiene 1567 chiavi. Duplicati rilevati: `ARG_rise.10.a` e `ARG_rise.10.d` (due definizioni ciascuna). Il `.bak` e un duplicato potenzialmente caricato/ambiguo a seconda dell'estensione e va rimosso dalla distribuzione in una patch separata. Caratteri visualizzati come `Ã` nel terminale: rischio di rendering/encoding da verificare nel gioco.

## 13. GFX e asset

Cinque DDS sotto `gfx/interface`; due definizioni `.gfx` in `interface`. In root sono presenti immagini workshop/supporto. `thumbnail.png` e `Argentina_Rise_Workshop_Preview_512.png` hanno la stessa dimensione, e `ChatGPT Image 6 lug 2026, 18_44_35.png` / `support_argetina_rise_kofi.png` hanno stessa dimensione: **BASSO, possibile duplicato o naming errato**. File `support_argetina_rise_kofi.png` contiene probabile typo nel nome.

## 14. Dipendenze Millennium Dawn

- Treasury: scripted effect `modify_treasury_effect` e variabili Treasury, richiamati dai focus/decisioni.
- Energia: `calculate_energy_use`, `energy_gain_multiplier`, `nuclear_energy_generation_modifier` in MD `common/scripted_effects/!_energy_effects.txt` e `common/modifier_definitions/energy_modifier_definitions.txt`.
- Party arrays, influenza, GDP, welfare, elezioni e leader: usati dai focus/eventi ARG e forniti dai sistemi MD; compatibilita versione: **NON VERIFICATA**.
- Special projects/facility: utilizzati come ID MD; nessun override integrale MD rilevato nella struttura submod.

## 15. Problemi aperti

### Medio

1. Localizzazioni duplicate `ARG_rise.10.a` e `ARG_rise.10.d` in `localisation/english/ARG_rise_l_english.yml`.
2. `ARG_rise_l_english.yml.bak` e distribuito con la mod: rischio di confusione/versione divergente.
3. Nessuna verifica automatica del tag `v0.1.3`, remoto o working tree: release identity non certificabile senza Git eseguibile.

### Basso

1. Asset root probabilmente duplicati e `support_argetina_rise_kofi.png` con nome sospetto.
2. 41 reward focus vuote: distinguere placeholder intenzionali da reward mancanti.
3. Non esistono directory `history` e `docs` pre-audit; `history` e coerente con ereditarieta MD, `docs` e stato creato per questo report.

Nessun problema critico o alto e stato provato dallo scan statico. Riferimenti runtime, scope, GFX e percorsi UI restano da testare.

## 16. Debito tecnico

Il focus tree e molto concentrato in un singolo file; la tracciabilita tra focus, idee, eventi e restore effect richiede automazione. Mancano test di integrazione HOI4, inventario asset e controllo Git eseguibile. La logica energia/nucleare dipende da modifier e scripted effects MD sensibili alla versione.

## 17. Baseline stabile confermata

Staticamente confermati: struttura focus, idee, decisioni, on-actions, eventi, cinque reward nucleari da 5 reattori, stati 450/453 e baseline nucleare ARG +100%. Il funzionamento in partita, progetto Fusion in UI, rendering GFX, salvataggi e compatibilita MD sono **NON VERIFICATI**.

## 18. Prossimo obiettivo consigliato

Eseguire un playtest mirato del percorso `El Proyecto Nacional`: scelta evento `ARG_rise.4`, completamento del gate, catena `ARG_rise.5+`, controllo dei flag e confronto con l'ipotesi progettuale Milei/comunista. Solo dopo, implementare la biforcazione mancante con requisiti verificati.

## 19. File principali del progetto

- `common/national_focus/ARG_rise_focus_tree.txt`: albero nazionale.
- `common/ideas/ARG_rise_ideas.txt`: idee e spirit.
- `events/ARG_rise_events.txt`: eventi ARG.
- `common/decisions/`: decisioni e categorie.
- `common/on_actions/ARG_rise_on_actions.txt`: trigger startup/daily/monthly/civil war.
- `common/scripted_effects/ARG_rise_restore_effects.txt`: ripristino idee post-guerra civile.
- `localisation/english/ARG_rise_l_english.yml`: localizzazione principale.
- `interface/*.gfx` e `gfx/interface/**`: sprite e texture.
- `MARKDAWN.md`: documentazione, non sorgente di implementazione.

## 20. Appendice - elementi non verificati

- Tag `v0.1.3`, stato Git, remoto e file non tracciati.
- Raggiungibilita runtime di tutti i 353 focus e 97 eventi.
- Referenze di ogni sprite, icona e DDS.
- Validita runtime di ogni modifier MD, scope e scripted effect.
- Presenza/visibilita del progetto Fusion dopo dieci tecnologie.
- Encoding/rendering dei caratteri accentati nel client HOI4.

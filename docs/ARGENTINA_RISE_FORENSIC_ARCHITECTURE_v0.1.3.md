# Argentina Rise - Forensic Architecture v0.1.3

Analisi architetturale statica del workspace corrente. Non valuta bilanciamento o qualita narrativa di singoli reward: descrive come i sistemi sono composti, connessi e rischiosi da evolvere.

## 1. Executive architecture map

```text
Focus tree (9,436 linee)
  -> idee/spirit persistenti (3,426 linee)
  -> eventi politici e di transizione (1,491 linee)
  -> decisioni operative (11 file)
  -> flag / variabili / party arrays MD
  -> on_actions startup, daily, monthly, civil-war end
  -> restore effect post-guerra civile (1,469 linee)
  -> sistemi Millennium Dawn: Treasury, energia, influenza, elezioni,
     GDP, welfare, leader, special projects e modifier definitions

GFX/localizzazione -> rappresentazione dei sistemi sopra, non logica primaria
```

L'architettura e una submod di orchestrazione: definisce contenuto ARG e usa MD come piattaforma di economia, energia, politica e progetti speciali. Non replica integralmente sistemi della mod madre; il suo rischio principale e la dipendenza semantica da ID ed effetti MD.

## 2. Macro-sistemi e accoppiamento

| Sistema | File principali | Ruolo | Accoppiamento |
|---|---|---|---|
| Progressione nazionale | `common/national_focus/ARG_rise_focus_tree.txt` | Orchestratore di quasi tutti i percorsi | Rosso: idee, eventi, decisioni, MD |
| Stato persistente | `common/ideas/ARG_rise_ideas.txt` | Spirit, programmi evolutivi, modifier | Rosso: focus, eventi, restore, MD modifiers |
| Transizioni politiche | `events/ARG_rise_events.txt` | Scelte, leader, flag, catene | Rosso: focus, on_actions, party arrays MD |
| Conferencia | focus + decisioni Conferencia + categorie + GFX | Diplomazia e integrazione regionale | Giallo/Rosso: piu file e condizioni |
| Economia operativa | nove file decisioni | Treasury, bonds, lavoro, procurement, risorse | Giallo: dipendenza forte da MD |
| Energia/nucleare | focus, idee, due decision file | Building, energia, combustibile e export | Rosso: scripted effects MD |
| Recovery | `ARG_rise_restore_effects.txt` + on_actions | Ripristino idee dopo guerra civile | Rosso: duplicazione del tree |
| Presentazione | localisation, interface, gfx | Testi e sprite | Giallo: errori visibili, poca logica |

Sistemi relativamente indipendenti: assets e localizzazione. Sistemi fortemente accoppiati: focus-idee-eventi-restore e energia-Treasury-MD. Il piu grande collo di bottiglia e il focus tree: molti sistemi sono raggiungibili solo da reward e prerequisite in quel file.

## 3. Grafo delle dipendenze

### Dipendenze forti

1. **Focus -> idee -> restore effect.** I focus assegnano idee; il restore effect replica le condizioni `has_completed_focus` e riaggiunge le idee dopo civil war. Ogni modifica a un reward idea richiede valutazione dell'effetto di restore.
2. **Focus -> eventi -> flag -> on_actions.** Il bivio post-Pampas usa `ARG_rise.4`, flag di percorso e trigger successivi. Proyecto Nacional usa inoltre il daily on-action per `ARG_rise.9`.
3. **Decisioni/focus -> MD Treasury e variabili.** Molte azioni chiamano `modify_treasury_effect`, quindi il contratto MD e operativo, non cosmetico.
4. **Nucleare -> modifier MD -> scripted energy.** Il building, la generazione e il carburante sono MD; ARG aggiunge focus/idee e non possiede il motore energetico.

### Dipendenze deboli

- Localizzazione e GFX dipendono dagli ID, ma non cambiano l'esecuzione logica.
- Defines e dynamic modifier ARG sono piccoli e isolati; una modifica puo comunque avere effetto globale sul comportamento, quindi non sono automaticamente sicuri.

### Circuiti e colli di bottiglia

Non e stato provato un ciclo sintattico distruttivo. Esiste tuttavia un **circuito di recupero controllato**: focus assegna idea -> guerra civile puo rimuoverla -> `on_civil_war_end` chiama restore -> restore ricava l'idea dal focus completato. E utile ma duplica la conoscenza del tree. I gate `The Pampas Engine`, i flag `ARG_rise_path_*` e `ARG_rise_proyecto_nacional_started` sono colli di bottiglia politici.

## 4. Analisi dei file importanti

| File | Funzione / dipendenti | Criticita | Rischio |
|---|---|---|---|
| `common/national_focus/ARG_rise_focus_tree.txt` (9,436 linee, 1,382 ref ARG) | hub di 353 focus, eventi, idee, building e flag | Rosso | Modifica locale puo spezzare grafi e restore |
| `common/ideas/ARG_rise_ideas.txt` (3,426 linee, 635 ref) | stato persistente e modifier | Rosso | stacking, modifier MD, rimozioni incoerenti |
| `common/scripted_effects/ARG_rise_restore_effects.txt` (1,469 linee, 656 ref) | recovery post-civil-war | Rosso | copia logica dei reward idea |
| `events/ARG_rise_events.txt` (1,491 linee) | transizioni/leader/flag | Rosso | trigger e catene asincrone |
| `common/on_actions/ARG_rise_on_actions.txt` (65 linee) | ingressi automatici | Rosso | piccolo ma ad alta leva; daily trigger |
| `decisions/ARG_rise_buenos_aires_conference_decisions.txt` (627 linee, 116 ref) | sistema regionale | Rosso | condizioni, target e ripetibilita |
| `decisions/ARG_rise_military_procurement_decisions.txt` (618 linee) | procurement | Giallo | Treasury/building/slot MD |
| `common/defines/zz_ARG_rise_defines.lua` (58 linee) | tuning globale | Rosso | define errata influenza comportamento ampio |
| `interface/ARG_rise_goals.gfx` (138 linee) | mappa sprite focus | Giallo | rendering e riferimenti mancanti |
| `localisation/english/ARG_rise_l_english.yml` | 1,567 chiavi | Giallo | chiavi duplicate/encoding |

Verde: DDS, immagini workshop/supporto e category files, salvo rinomini o referenze. Giallo: decisioni isolate e localizzazione. Rosso: tree, idee, eventi, on-actions, restore, defines e Conferencia.

## 5. Debito tecnico

### Duplicazione strutturale

- Il restore effect ripete la relazione focus -> idea del tree. Non e un errore: e una compensazione per tag handling MD, ma ogni nuovo spirit permanente aumenta il costo di sincronizzazione.
- Molti programmi evolutivi hanno la stessa struttura `idea_n -> modifier_n -> focus_n`; il pattern e chiaro ma non centralizzato.
- Due chiavi di localizzazione duplicate: `ARG_rise.10.a` e `ARG_rise.10.d`. E presente anche `ARG_rise_l_english.yml.bak` nella distribuzione.

### File troppo grandi / hot spots

- Focus tree da 9,436 linee: alto costo cognitivo, conflitti Git e rischio coordinate/prerequisite.
- Idee da 3,426 linee: alto rischio di stacking invisibile.
- Restore da 1,469 linee: fragilita per divergenza dal tree.

### Centralizzazione consigliabile, ma non implementata qui

Un inventario dichiarativo dei reward-idea permanenti permetterebbe di derivare il restore effect. Prima di qualsiasi rifattorizzazione serve testare il comportamento di civil war e save compatibility.

## 6. Riutilizzo di Millennium Dawn

| Sistema ARG | Provenienza MD | Valutazione |
|---|---|---|
| Treasury | `modify_treasury_effect`, variabili Treasury | Uso corretto, dipendenza forte |
| Energia/nucleare | buildings, `calculate_energy_use`, modifier definitions | Uso corretto; evitare override globali |
| Party arrays/elezioni | scripted effects e array politici | Potente ma fragile a cambi MD |
| GDP/welfare/cartelli/influenza | sistemi economico-politici MD | Riutilizzo coerente, molto accoppiato |
| Special projects/facility | contenuto e trigger MD | Riutilizzo sottile; UI/runtime da testare |
| Decision categories/modifier | standard Paradox + MD | Rischio medio di naming e scope |

La submod contiene poco codice di piattaforma originale e molto contenuto/orchestrazione originale. Questo scala bene finche gli ID MD sono stabili; diverge rapidamente quando MD rinomina modifier, scripted effect o variabili.

## 7. Complessita dei sistemi, dal minore al maggiore

1. Asset e localizzazione: molte chiavi, poca logica.
2. Decisioni isolate (bonds, risorse): condizioni/costi lineari.
3. Idee evolutive: stacking e assegnazione, ma struttura ripetibile.
4. Energia/nucleare: submod semplice, dipendenza MD alta.
5. Focus tree: grafo largo con reward eterogenei.
6. Eventi politici: stato asincrono, flag, leader e party arrays.
7. Conferencia: focus + decisioni + categorie + target regionali.
8. Civil-war restore: sincronizza stato derivato tra sistemi e rappresenta il punto piu delicato da evolvere.

## 8. Rischio evolutivo al raddoppio della mod

### Scalano bene

- Nuovi rami focus se isolati da gate chiari.
- Idee e localizzazioni, se convenzioni ID restano coerenti.
- Decisioni per sistemi locali con una sola dipendenza MD.

### Creano problemi

- Il singolo focus tree diventera il principale punto di conflitto e navigazione.
- Il restore effect crescera quasi linearmente con ogni nuovo spirit permanente.
- Eventi/on-actions possono moltiplicare trigger duplicati e condizioni di flag.
- Conferencia e sistemi regionali aumentano combinatoriamente con nuovi paesi/target.
- Le dipendenze MD richiedono una matrice di compatibilita per release.

## 9. Ordine consigliato dei prossimi sei mesi

1. **Baseline e test harness manuale:** checklist in-game per startup, civil war, energia, Conference e salvataggi.
2. **Mappa delle dipendenze e convenzioni:** registrare per ogni nuovo focus idee/eventi/flag/restore richiesti.
3. **Proyecto Nacional foundation:** completare prima gate, eventi, leader e stato persistente; e il ramo con maggiore bisogno di identita.
4. **Due percorsi politici separati:** Milei e comunista/populista solo dopo che il tronco comune e testato.
5. **Hardening del restore:** audit automatico delle idee da focus e test civil war.
6. **Conferencia/espansione:** estendere dopo stabilizzazione dei contratti flag/evento.
7. **Polish tecnico:** localizzazione, GFX, asset root e documentazione di compatibilita MD.

Questo ordine riduce rework: prima stabilisce contratti e test, poi aggiunge contenuto con una struttura gia provata.

## 10. Risposte architetturali finali

### 1. Punto piu forte

La submod usa MD come piattaforma invece di duplicarla: focus, idee, eventi e decisioni costruiscono una forte identita ARG sopra sistemi maturi di Treasury, energia e politica.

### 2. Punto piu debole

La conoscenza del flusso e distribuita fra focus, idee, eventi, on-actions e restore effect. Il costo di cambiare una singola ricompensa persistente e maggiore di quanto suggerisca il singolo file.

### 3. Sistema da rifattorizzare

Il contratto **focus -> idea -> restore**. Non va riscritto ora: va prima catalogato e testato, quindi reso dichiarativo o almeno verificato automaticamente.

### 4. Migliore base per Proyecto Nacional

Il pattern post-Pampas: evento di scelta -> flag di percorso -> focus gate -> evento -> on-action mirata. E gia funzionante per Proyecto Nacional e mantiene separazione fra narrazione, stato e progressione.

### 5. Roadmap tecnica verso 1.0

Stabilizzare contratti MD e test di regressione; completare Proyecto Nacional con due identita realmente distinte; consolidare restore/eventi; validare Conferencia e contenuti regionali; poi ridurre debito di localizzazione/assets e pubblicare una matrice di compatibilita per la versione MD supportata.

## Evidenze principali

- `common/national_focus/ARG_rise_focus_tree.txt`
- `common/ideas/ARG_rise_ideas.txt`
- `common/scripted_effects/ARG_rise_restore_effects.txt`
- `events/ARG_rise_events.txt`
- `common/on_actions/ARG_rise_on_actions.txt`
- `common/decisions/ARG_rise_buenos_aires_conference_decisions.txt`
- `E:\Steam\steamapps\workshop\content\394360\2777392649\common\scripted_effects\!_energy_effects.txt`
- `E:\Steam\steamapps\workshop\content\394360\2777392649\common\modifier_definitions\energy_modifier_definitions.txt`
